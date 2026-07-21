在Lua语言中，主要通过`json.encode`和`json.decode`这两个API来实现JSON数据的序列化与反序列化操作。

JSON作为一种轻量级的数据交换格式，具有易于人类阅读和编写、机器解析和生成的特点。在实际的Lua编程应用中，解析JSON数据是一项十分常见的需求，尤其是在涉及数据处理的场景中。例如，许多云平台的物模型通常采用json结构进行数据组织和传输。

银尔达旗下的不同系列产品所搭载的Lua版本有所不同，其中Air724系列采用的是Lua5.1版本，Air780系列则使用的是Lua5.3版本。不过，这两个版本的json处理API是通用的。

接下来，我们将重点讲解如何在银尔达的任务和模板功能中对json数据进行简单的解析和组装。关于json数据的具体格式以及Lua语法的详细内容，在此不做过多阐述。如果读者对json数据格式有疑问，可通过搜索引擎自行查找相关资料；若对Lua语法感兴趣，也可自行查阅相关学习资料进行深入学习。

Lua5.1官方API资料

https://doc.openluat.com/wiki/21?wiki_page_id=2261

Lua5.3官方API资料

https://wiki.luatos.com/api/json.html

## 一、json.encode(t) 

json.encode的作用是将表格数据编码为json字符串，传入Lua中table类型的数据，得到序列化后的json字符串。

| 传入值 | t，table类型                                                 |
| ------ | ------------------------------------------------------------ |
| 返回值 | 1.序列化后的json字符串，失败返回nil，string类型2.序列化失败的报错信息 |

用法示例：

1. **一层json**

```lua
local data={}    --定义一个table 在表里添加三个对象为a b c分别赋值 1 2 3
data.a=1
data.b=2
data.c=3
local json_data=json.encode(data)--使用json.encode 将表序列化为json字符串
log.info("json_data:",json_data)
--打印出来结果为：{"a":1,"b":2,"c":3}
```

1. **多层json嵌套**

```lua
local data={}    --定义一个table 在表里添加多个对象 
data.a=1
data.b={}        --将需要嵌套的对象定义为table 再添加相应的对象
data.b.a=2
data.c={}
data.c.a=3
local json_data=json.encode(data)--使用json.encode 将表序列化为json字符串
log.info("json_data:",json_data)
--打印出来结果为：{"a":1,"b":{"a":2},"c":{"a":3}}
local data={}    
data.a=1
data.b={}
data.b.a={}
data.b.a.b=2
data.c={}
data.c.a={}
data.c.a.b=3
local json_data=json.encode(data)--使用json.encode 将表序列化为json字符串
log.info("json_data:",json_data)
--打印出来结果为：{"a":1,"b":{"a":{"b":2}},"c":{"a":{"b":3}}}
```

1. **嵌套json中的数组**

```lua
local data={}    
data.a=1
data.b={}
data.b[1]=2   --将数组第一个对象赋值为2，第二个对象定位为table,再为对象c赋值为3
data.b[2]={}
data.b[2].c=3
local json_data=json.encode(data)--使用json.encode 将表序列化为json字符串
log.info("json_data:",json_data)
--打印出来结果为：{"a":1,"b":[2,{"c":3}]}
local data={}    
data[1]=1
data[2]=2
data[3]={}
data[3][1]={}
data[3][1].a=3
local json_data=json.encode(data)--使用json.encode 将表序列化为json字符串
log.info("json_data:",json_data)
--打印出来结果为：[1,2,[{"a":3}]]
local data={}    
data.a="json数据"
data.b={}
data.b[1]={}
data.b[1].value="10"
data.b[1].id=1
data.b[2]={}
data.b[2].value="20"
data.b[2].id=2
local json_data=json.encode(data)--使用json.encode 将表序列化为json字符串
log.info("json_data:",json_data)
--打印出来结果为：{"a":"json数据","b":[{"id":1,"value":"10"},{"id":2,"value":"20"}]}
```

## 二、json.decode(str)

json.decode的作用是将json字符串解码为Lua中table类型的数据，得到反序列化后的表格对象。

| 传入值 | 需要反序列化的json字符串，string类型                         |
| ------ | ------------------------------------------------------------ |
| 返回值 | 1.反序列化后的对象(通常是table), 失败的话返回nil2.反序列化结果，1成功，0失败3.反序列化失败的报错信息 |

用法示例：

1. **一层json**

这里我们假设json数据为：{"a":1,"b":2,"c":3}

```lua
local data='{"a":1,"b":2,"c":3}'
local j=json.decode(data)    --获取返回值 只定义一个变量接收第一个返回值 然后分别判断json对象是否存在
if j and type(j)=="table" then
  if j.a then
    log.info("j.a=",j.a)
  end
  if j.b then
    log.info("j.b=",j.b)
  end
  if j.c then
    log.info("j.c=",j.c)
  end
end
--打印出来结果为：j.a=1,j.b=2,j.c=3
```

1. **多层json嵌套**

这里我们假设json数据为：{"a":1,"b":{"a":2},"c":{"a":3}}

```lua
local data='{"a":1,"b":{"a":2},"c":{"a":3}}'
local j=json.decode(data)
if j and type(j)=="table" then
  if j.a then
    log.info("j.a=",j.a)
  end
  if j.b then
    if j.b.a then
      log.info("j.b.a=",j.b.a)
    end
  end
  if j.c then
    if j.c.a then
      log.info("j.c.a=",j.c.a)
    end
  end
end
--打印出来结果为：j.a=1,j.b.a=2,j.c.a=3
```

1. **JSON数组中的嵌套结构**

这里我们假设json数据为：{"a":"json数据","b":[{"id":1,"value":"10"},{"id":2,"value":"20"}]}

```lua
local data='{"a":"json数据","b":[{"id":1,"value":"10"},{"id":2,"value":"20"}]}'
local j=json.decode(data)
if j and type(j)=="table" then
  if j.a then
    log.info("j.a=",j.a)
  end
  if j.b then
    if j.b[1] then
      if j.b[1].id then
        log.info("j.b[1].id=",j.b[1].id)
      end
      if j.b[1].value then
        log.info("j.b[1].value=",j.b[1].value)
      end
      if j.b[2].id then
        log.info("j.b[2].id=",j.b[2].id)
      end
      if j.b[2].value then
        log.info("j.b[2].value=",j.b[2].value)
      end
    end
  end
end
--打印出来结果为：j.a=json数据,j.b[1].id=1,j.b[1].value=10,j.b[2].id=2,j.b[2].value=20
```

## 三、特殊的json对象处理

有的时候要定义特殊符号、字符、数字这些在一起的json对象。

例如：{"2025-数据-#":"123456"} 

```lua
local data={} 
data.2025-数据-#="xxxx"
local json_data=json.encode(data)
log.info("json_data:",json_data)
--[string "local data={} ..."]:2: syntax error near '.2025'
--这样是不支持的，会报错。
```

**正确的序列化处理方式**

```lua
local data={}
data["2025-数据-#"]="xxxx"
local json_data=json.encode(data)
log.info("json_data:",json_data)
--打印出来的结果为：{"2025-数据-#":"xxxx"}
```

**json反序列化也是一样的**

```lua
local data='{"2025-数据-#":"xxxx"}'
local j=json.decode(data)    
if j and type(j)=="table" then
  if j["2025-数据-#"] then
    log.info('j["2025-数据-#"]=',j["2025-数据-#"])
  end
end
--打印出来的结果为：xxxx
```

## 四、代码调试方法

合宙官方提供了Lua5.3的模拟场景，您可以将本文中的代码复制到该测试环境中进行实际操作。需要特别注意的是，完整的银尔达任务脚本并不能在这个测试环境中完全运行。若要进行有效的调试，请参照[任务和数据模板调试方法章节](https://yinerda.yuque.com/yt1fh6/4gdtu/oa1g22bovngd04ig)，通过打印日志的方式来辅助完成调试工作。

https://wiki.luatos.com/_static/luatos-emulator/lua.html

![img](https://cdn.nlark.com/yuque/0/2025/png/42958080/1739526601592-0466d55a-9951-4544-8779-a0264c9e0e92.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_33%2Ctext_eWluZXJkYQ%3D%3D%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)