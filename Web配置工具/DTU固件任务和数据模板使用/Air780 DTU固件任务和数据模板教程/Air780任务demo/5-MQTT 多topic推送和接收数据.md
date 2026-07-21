任务和数据模板代写:可以先参考demo自己修改一下，如果不会修改，可以提交需求文档，然后让银尔达工程师评估代写！根据工作量，简单的不收费，难的可能会收费。

# 一、代码一（拼接字符串处理）

--本demo使用场景是当订阅多个topic的时候可以透传topic名字和内容出来(通过函数PronetMqttProReciveTopic 确认是否需要包含topic名字和topic与数据的分隔符)

--可以指定的topic 发布数据到服务器(提前配置参数 通过ID确认topic)

--可以自定义topic 发布到服务器(不提前配置topic参数)

--下面的demo实现 打印服务器收到的topic***发布的数据

--通过判断串口收到的数据头部，如果是bbb1开头的给topic1推送,bbb2开头的给topic2推送,bbb3开头的给topic3推送,并且过滤了头部

--注意指定topic是必须提前配置

--注意如果配置了注册包,默认使用第个一个topic发送注册包

```lua
function 
	local taskname="userTask"
	log.info(taskname,"start")
	local nid=1
	local uid=1
	local netsta =0
	PronetStopProRecCh(1)
	UartStopProRecCh(1)
	PronetMqttProReciveTopic(nid,1,"***")
	while true do 
		local netr = PronetGetRecChAndDel(nid)
		if netr then 
			log.info(taskname,"netr",netr)
			UartSetSendCh(uid,netr)
		end 
		
		local uartr = UartGetRecChAndDel(uid)
		if uartr then 
			log.info(taskname,"uartr",uartr)
			local netsta = PronetGetNetSta(nid)
			log.info(taskname,"netsta",netsta)
			if netsta ==1 then 
				if string.startsWith(uartr,"bbb1") then
					local bbb1={}
					bbb1[1]=1
					bbb1[2]=string.sub(uartr,5,-1)
					PronetSetSendCh(nid,bbb1)
				elseif string.startsWith(uartr,"bbb2") then 
					local bbb2={}
					bbb2[1]=2
					bbb2[2]=string.sub(uartr,5,-1)
					PronetSetSendCh(nid,bbb2)
				elseif string.startsWith(uartr,"bbb3") then 
					local bbb3={}
					bbb3[1]=3
					bbb3[2]=string.sub(uartr,5,-1)
					PronetSetSendCh(nid,bbb3)
				else
					PronetSetSendCh(nid,uartr)
				end 
			end 
		end 
		
		sys.wait(1000)
	end 
end
```

# 二、代码二（从json结构中提取topic）

下面的代码是实现将topic放在json结构体中时，将topic提取出来作为发布topic推送数据到服务器

 例如串口发送的数据为{"topic":"sss","cmd":"req_temp_hum"}   

将 sss作为发布topic  推送到服务器的实际数据为 {"cmd":"req_temp_hum"}  

```lua
function
	local taskname="userDataTask"
	log.info(taskname,"start")
	local nid = 1
	local uid = 1
	UartStopProRecCh(1)
	while true do
		local uartr=UartGetRecChAndDel(uid)
		if uartr then 
			local j =json.decode(uartr)
			if j and type(j)=="table" and j.topic then 
				local d = {}
				d[1]=j.topic
				j.topic = nil
				local updata = json.encode(j)
				d[2]= updata
				if 1==PronetGetNetSta(nid) then 
					PronetSetSendCh(nid,d)
				end
			end 
		end 
		sys.wait(100)
	end
end
```