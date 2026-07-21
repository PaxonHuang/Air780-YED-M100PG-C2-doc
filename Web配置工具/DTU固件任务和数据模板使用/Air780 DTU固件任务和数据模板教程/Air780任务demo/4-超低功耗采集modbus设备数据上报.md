任务和数据模板代写:可以先参考demo自己修改一下，如果不会修改，可以提交需求文档，然后让银尔达工程师评估代写！根据工作量，简单的不收费，难的可能会收费。



# 零、任务教学视频

讲解了任务能做什么，如果提需求，如何看API，如何检查语法，调试打印日志

https://www.bilibili.com/video/BV1pN1aBuEr7

# 一、功能描述

使用场景是30分钟以上定时周期采集数据，本demo使用YED-D780L1Y 作为demo

首先开启可控电源给外部传感器供电;延迟5秒等传感器数据稳定,等待网络正常，清除串口脏数据,读取485数据,组装数据传给服务器,关闭可控电源输出,进入超级低功耗，再休眠之前可以接收服务器的返回数据确认收到后进入休眠。

本demo是把设备应答的串口数据，通过json传给服务器解析了，其实设备本地也能解析后上传

注意如果过快进入低功耗，可能会导致DTU更新不了参数(还没更新参数，DTU就休眠了,如果后续要更新参数需要预留足够的时间)

超低功耗耗电量特别低，休眠后，设备的电容里的电会导致设备外部断电后设备还是进入休眠的。在调试的时候可以按RST复位按键唤醒设备，也可以短路VIN GND，放电后重新上电。

# 二、代码

```lua
function 
	mobile.flymode(0, false)
	local taskname="userTask"
	log.info(taskname,"start")
	local nid=1
	local uid=1
	local cmd={
		{0x01,0x03,0x00,0x00,0x00,0x02,0xC4,0x0B},
		{0x04,0x03,0x00,0x00,0x00,0x02,0xC4,0x5E}
	}
	local uartr={}
	local count =0
	local netsta =0
	
	PronetStopProRecCh(1)
	UartStopProRecCh(1)
	PerCtlPowerOut(1)
	while count < 60 do 
		netsta = PronetGetNetSta(nid)
		if netsta ==1 then 
			break
		end 
		count =count +1
		sys.wait(1000)
	end 
	
	if count < 15 then 
		sys.wait((15-count)*1000)
	end 
	
	while true do 
		local u=UartGetRecChAndDel(uid)
		if u ==nil then 
			break
		end 
		sys.wait(100)
	end
		
	for i=1,#cmd,1 do 
		local ncmd=ToolTableToHexStr(cmd[i])
		UartSetSendCh(uid,ncmd)
		sys.wait(500)
		uartr[i] = UartGetRecChAndDel(uid)
		if uartr[i] then 
			log.info(taskname,"uartr[i]",i,string.toHex(uartr[i]))
		else 
			log.info(taskname,"uartr[i]",i,"nodata")
		end 
	end 
	
	PerCtlPowerOut(0)
	
	local d ={}
	d.datetime=os.date("%Y-%m-%d %H:%M:%S")
	d.csq=mobile.csq()
	d.tarkey="LGWY"
	d.sn=mobile.imei()
	d.iccid = PerGetIccid()
	d.vol=PerGetAdcGatherValByAdcId(0)
	if uartr[1] then 
		d.id1=string.toHex(uartr[1]) 
	else 
		d.id1=""
	end 
	
	if uartr[2] then 
		d.id4=string.toHex(uartr[2]) 
	else 
		d.id4=""
	end 
	local updata = json.encode(d)
	netsta = PronetGetNetSta(nid)
	log.info(taskname,"updata",updata,"netsta",netsta)
	if updata and netsta ==1 then 
		PronetSetSendCh(nid,updata)
	end
	SysNeedInSuperPower()
	log.info(taskname,"superlowpower")
	PerFeedHwdg()
	sys.wait(2000)
	SysInSuperLowPower(1800)--老Air780E用这个，air780EP,Air780EPM的重新封装
end
```