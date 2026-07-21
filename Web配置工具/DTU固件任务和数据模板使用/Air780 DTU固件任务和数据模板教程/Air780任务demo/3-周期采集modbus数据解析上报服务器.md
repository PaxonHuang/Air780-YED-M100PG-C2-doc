任务和数据模板代写:可以先参考demo自己修改一下，如果不会修改，可以提交需求文档，然后让银尔达工程师评估代写！根据工作量，简单的不收费，难的可能会收费。



# 零、任务教学视频

讲解了任务能做什么，如果提需求，如何看API，如何检查语法，调试打印日志

https://www.bilibili.com/video/BV1pN1aBuEr7

# 一、功能描述

本demo实现 Modbus RTU 协议 03/04 功能码通用寄存器读取，采用键值对统一管理寄存器地址，可以区分整型大小端、abcd/cdab浮点字节序解析，自动拼接 CRC 校验，支持 2/4 字节、有 / 无符号整型与 IEEE754 浮点解析，解析数据组装 JSON 上传云端，适配各类 Modbus 工控传感器设备采集场景。

# 二、代码

```lua
function
    sys.wait(5000)
    local taskname="userTask"
    log.info(taskname, "Modbus读取任务启动")
    local CONFIG = {
        nid = 1,               -- 网络通道ID
        uid = 1,               -- 串口通道ID
        dev_addr = 1,          -- 设备站地址
        wait_time = 500,       -- 串口等待时间(ms)
        loop_wait = 60000      -- 周期上报间隔(ms)
    }

    local THRESHOLD = {
        temperature = 2, 
        humidity = 3,  
        voltage = 0.5, 
        power = 10,  
        pressure = 1 
    }

    -- 寄存器映射表
    local mreg = {
        ["temperature"] = 0x0001,  -- 温度
        ["humidity"]    = 0x0000,  -- 湿度
        ["voltage"]     = 0x0002,  -- 电压
        ["power"]       = 0x0003,  -- 功率(4字节)
        ["pressure"]    = 0x0004   -- 压力(浮点)
    }

    PronetStopProRecCh(1)
    UartStopProRecCh(1)

    -- Modbus RTU 寄存器读取函数
    local function ModbusRTU_ReadRegister(reg_key, func_code, reg_num, is_signed, data_type, endian, float_byte_seq)
        local reg_addr = mreg[reg_key]
        if not reg_addr then
            log.info(taskname, "寄存器键名不存在：", reg_key)
            return nil
        end
        if func_code ~= 0x03 and func_code ~= 0x04 then
            log.info(taskname, "仅支持03/04功能码")
            return nil
        end
        if reg_num ~= 1 and reg_num ~= 2 then
            log.info(taskname, "仅支持1个或2个寄存器读取")
            return nil
        end

        local frame = pack.pack('>bbHH', CONFIG.dev_addr, func_code, reg_addr, reg_num)
        local crc = crypto.crc16_modbus(frame)
        local send_cmd = frame .. pack.pack('<H', crc)

        UartSetSendCh(CONFIG.uid, send_cmd)
        sys.wait(CONFIG.wait_time)
        local recv_data = UartGetRecChAndDel(CONFIG.uid)

        if not recv_data then
            log.info(taskname, reg_key, "串口无返回数据")
            return nil
        end
        if #recv_data < 7 then
            log.info(taskname, reg_key, "数据长度异常")
            return nil
        end
        local data_table = ToolHexStrToTable(recv_data)
        if not ToolCheckModbusTableCRC(data_table, #data_table, 0) then
            log.info(taskname, reg_key, "CRC校验失败")
            return nil
        end

        local raw_data = string.sub(recv_data, 4, -3)
        local result = 0

        if data_type == "int" then
            if reg_num == 1 then
                if is_signed then
                    _, result = pack.unpack(raw_data, endian == "le" and "<h" or ">h")
                else
                    _, result = pack.unpack(raw_data, endian == "le" and "<H" or ">H")
                end
            end
            if reg_num == 2 then
                if is_signed then
                    _, result = pack.unpack(raw_data, endian == "le" and "<i" or ">i")
                else
                    _, result = pack.unpack(raw_data, endian == "le" and "<I" or ">I")
                end
            end
			log.info(taskname,"读取：",reg_key, "值：",result)
        end

		if data_type == "float" and reg_num == 2 then
			local b1,b2,b3,b4 = string.byte(raw_data, 1, 4)
			local valid_data		
			if float_byte_seq == "abcd" then
				valid_data = string.char(b1, b2, b3, b4)
			elseif float_byte_seq == "badc" then
				valid_data = string.char(b2, b1, b4, b3)
			elseif float_byte_seq == "cdab" then
				valid_data = string.char(b3, b4, b1, b2)
			elseif float_byte_seq == "dcba" then
				valid_data = string.char(b4, b3, b2, b1)
			else
				-- 非法字节序，默认使用abcd
				log.warn(taskname, reg_key, "不支持的浮点数字节序，默认使用abcd")
				valid_data = string.char(b1, b2, b3, b4)
			end
			_, result = pack.unpack(valid_data, ">f")
			log.info(taskname,"读取：",reg_key, "值：",result)
		end
        return result
    end

    -- 数值差值判断函数
    function TaskGet2NumGreaterThanV(now,old,v)
        local r =false
        if type(now)=="number" and type(old) =="number" and type(v) =="number" then
            local d = math.abs(now-old)
            if d > v then
                r = true
                log.info("TaskGet2NumGreaterThanV ok",now,old,v)
            end
        end
        return r
    end

    -- 清空串口缓存
    local function ClearUart()
        while UartGetRecChAndDel(CONFIG.uid) do sys.wait(20) end
    end

    local last_data = {  -- 上次上报的数据
        temperature = nil, humidity = nil, voltage = nil, power = nil, pressure = nil
    }
    local last_report_time = 0  -- 上次的时间
    local report_interval = CONFIG.loop_wait / 1000

    while true do
        ClearUart()
        -- 持续采集传感器数据
        local temp     = ModbusRTU_ReadRegister("temperature", 0x03, 1, true,  "int",   "be", nil)
        local humi     = ModbusRTU_ReadRegister("humidity",    0x03, 1, false, "int",   "le", nil)
        local volt     = ModbusRTU_ReadRegister("voltage",     0x04, 1, false, "int",   "be", nil)
        local power    = ModbusRTU_ReadRegister("power",       0x03, 2, false, "int",   "le", nil)
        local pressure = ModbusRTU_ReadRegister("pressure",    0x04, 2, false, "float", nil, "abcd")

        local need_immediate = false
        -- 首次采集立即上报
        if last_data.temperature == nil then
            need_immediate = true
            log.info(taskname, "首次启动，立即上报")
        else
            -- 任意参数超差值触发立即上报
            if temp and TaskGet2NumGreaterThanV(temp, last_data.temperature, THRESHOLD.temperature) then need_immediate = true end
            if humi and TaskGet2NumGreaterThanV(humi, last_data.humidity, THRESHOLD.humidity) then need_immediate = true end
            if volt and TaskGet2NumGreaterThanV(volt, last_data.voltage, THRESHOLD.voltage) then need_immediate = true end
            if power and TaskGet2NumGreaterThanV(power, last_data.power, THRESHOLD.power) then need_immediate = true end
            if pressure and TaskGet2NumGreaterThanV(pressure, last_data.pressure, THRESHOLD.pressure) then need_immediate = true end
        end

        -- 周期上报时间判断
        local current_time = os.time()
        local need_cycle = (current_time - last_report_time >= report_interval)

        -- 执行上报
        if (need_immediate or need_cycle) and PronetGetNetSta(CONFIG.nid) == 1 then
            local up = {
                datetime = os.date("%Y-%m-%d %H:%M:%S"),
                csq = mobile.csq(), imei = mobile.imei(), iccid = PerGetIccid(),
                temperature = temp, humidity = humi, voltage = volt, power = power, pressure = pressure
            }
            local json_data = json.encode(up)
            if json_data then
                PronetSetSendCh(CONFIG.nid, json_data)

                -- 上报成功后更新缓存和时间
                last_data.temperature = temp
                last_data.humidity = humi
                last_data.voltage = volt
                last_data.power = power
                last_data.pressure = pressure

                -- 仅周期上报更新时间
                if need_cycle then
                    last_report_time = current_time
                    log.info(taskname, "周期上报完成")
                end
                if need_immediate then
                    log.info(taskname, "阈值触发，立即上报完成")
                end
            end
        end
        sys.wait(100)
    end
end
```

# 三、函数功能讲解和上报示例

## 1、基本参数

使用到的串口通道id，网络通道id，要读取的从站地址，发送命令后等待设备返回的间隔，周期上报间隔。

![img](https://cdn.nlark.com/yuque/0/2026/png/42958080/1778298157045-bfdd3506-55cf-48b3-a0c4-09af383eb24a.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_18%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

## 2、清除缓存函数

![img](https://cdn.nlark.com/yuque/0/2026/png/42958080/1778297922948-72de9ad7-a21b-4b12-9e4a-0e07fdbc3eb0.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_21%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

![img](https://cdn.nlark.com/yuque/0/2026/png/42958080/1778061286648-ab3cc637-d64b-4fc5-886f-6d50d0d7ff1b.png?x-oss-process=image%2Fcrop%2Cx_0%2Cy_0%2Cw_1238%2Ch_181)

## 3、Modbus读取函数

调用函数读取数据时根据读取的寄存器类型，数据类型，字节序等传入对应的值，解析成功返回数值，失败/异常返回nil。
![img](https://cdn.nlark.com/yuque/0/2026/png/42958080/1778061116768-337be277-898f-4138-b312-0b929e4f9c2f.png?x-oss-process=image%2Fcrop%2Cx_0%2Cy_207%2Cw_1342%2Ch_163)

| 参数名         | 参数数据类型 | 含义       | 备注                                     |
| -------------- | ------------ | ---------- | ---------------------------------------- |
| reg_key        | string       | 寄存器键名 | 对应mreg表，例：temperature              |
| func_code      | number       | 功能码     | 0x03=读保持寄存器 0x04=读输入寄存器      |
| reg_num        | number       | 寄存器数量 | 1=2字节  2=4字节                         |
| is_signed      | boolean      | 是否有符号 | true=有符号  false=无符号（仅整形生效）  |
| data_type      | string       | 数据类型   | int=整型  float=IEEE754单精度浮点        |
| endian         | string       | 大小端模式 | be=大端(默认)  le=小端（浮点忽略,填nil） |
| float_byte_seq | string       | 字节序模式 | abcd/badc/cdab/dcba（整形忽略,填nil）    |

![img](https://cdn.nlark.com/yuque/0/2026/png/42958080/1778061315424-86a76b8f-a9fa-4cfb-bc6f-caefdfaa79e2.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_32%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

## 4、数值差值判断函数

程序中还可以判断上次采集的数据和本次采集的数据的差值是否超过提前设定的阈值，差值超过阈值后立即上报，上报后将当前数据存储再与下次采集的数据进行对比。

![img](https://cdn.nlark.com/yuque/0/2026/png/42958080/1778296566246-b7d449b2-0e23-4411-8b11-5863168d8e85.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_20%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

![img](https://cdn.nlark.com/yuque/0/2026/png/42958080/1778296513216-117fe7cb-9a11-4be0-9416-c312e5430290.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_28%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

![img](https://cdn.nlark.com/yuque/0/2026/png/42958080/1778306424148-feacd751-b7c9-4f90-b6de-896517971112.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_43%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

## 5、推送到服务器的示例数据

![img](https://cdn.nlark.com/yuque/0/2026/png/42958080/1778306652267-f876e0fe-6432-4b2c-8ae6-06151198b6d1.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_36%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)