# 自定义代码

## B

### #bodyget_rs()

**示例1**
```enjoy
#bodyget_rs()
{
  json
}
```
渲染模板字符串，赋值给rs


**示例2**
```enjoy
#bodyget_rs("1") 
```
如果type参数为"1"，并且发生了MsgException异常，那么异常消息会被存储到scope中的"rs"变量中。否则会抛出异常

**示例3**

```enjoy
#bodyget_rs()
{
    "uModel": {
      "XM": "#(fzxm)", 
    }, 
}
#end
```

```enjoy
#bodyget_rs(res)   //结果存在res里
xxxxx
#end    
```



### #@busi_get_obj()

```enjoy
#@busi_get_obj("dictmanage.dict_outpat_clinic",["ZS_ID"],[zs_id],"outpat_clinic")
```

接受四个参数

- `p_tablename`:要查询的表名
- `p_ids`:要作为查询条件的字段名列表
- `p_values`:对应字段的值列表
- `p_var`:用于存储查询结果的变量名
  在 Enjoy 模板语言中,方括号 [ ] 用于表示列表(List)类型的数据。

第二个参数 ["ZS_ID"] 表示一个只含有一个元素 "ZS_ID" 的列表,这个列表代表了要作为查询条件的字段名。

第三个参数 [zs_id] 同样表示一个只含有一个元素 zs_id 的列表,这个列表代表了对应字段的值。

**#@busi_get_obj("some_table",["field1", "field2"],[value1, value2],"result")**

```enjoy
#@busi_get_obj("dictmanage.dict_outpat_clinic",["ZS_ID", "CLINIC_CODE"],[zs_id, clinic_code],"outpat_clinic")

SELECT * FROM dictmanage.dict_outpat_clinic 
WHERE ZS_ID = ? AND CLINIC_CODE = ?
```



## C

### #cache_plug(~)

```enjoy
#cache_plug("20000","//",["ip","wind"],[])
```

- "20000"表示缓存的超时时间，单位为毫秒。
- "//"表示缓存键的前缀，用于生成最终的缓存键。
- `["ip","wind"]`表示输入参数列表，指定了需要从scope中获取的参数名称。
- `[]`表示输出参数列表，指定了需要将哪些参数存储到缓存中并在后续使用时从缓存中获取。



### #@cf_get(~)

```enjoy
#@cf_get("sys.log.save.type","日志保存方式","1")
```

**查询系统配置**

**在系统功能>>>系统配置内查看**

#@cf_get(p_key,p_name,"p_value")

- `p_key`: 配置项的名称。
- `p_name`: 配置项的描述。
- `p_value`: 配置项的默认值。如果该配置项不存在将这个值存入。



### #@cf_set(~)

```enjoy
#@cf_set("sys.schedule.file.config","线程定时任务配置信息",cf)
```

**设置，更新系统配置**

**在系统功能>>>系统配置内查看**

#@cf_set(p_key,p_name,"p_value")

- `p_key`: 配置项的名称。

- `p_name`: 配置项的描述。

- `p_value`: 配置项的值。





### #@check_empty(~)

```enjoy
#@check_empty("账号或密码错误")
```

判断lsit是否为空，如果为空返回信息

```enjoy
#define check_empty(p_msg)
#if(isEmpty(list))
#@throwBusi(p_msg)
#end
#end
```



## M

### #@msg(p_data)   

输出



## R

### \#@response_success_rs_data(~)

```enjoy
#@response_success_rs_data("成功",rs)
```





## S

### #@setAttribute(~)

```enjoy
#@setMsgNotify?("弹出消息","info",10)
```

```enjoy
#define setAttribute(attributeKey,attributeValue)
#set(_req=kitutil.getRequest())
#if(_req!=null)
#set(_req.setAttribute(attributeKey,attributeValue))
#end
#end
```

将指定的属性键和属性值设置到请求对象中。

### #@setMsgNotify?(~)

```enjoy
#@setMsgNotify?("弹出消息","info",10)
```

弹出消息， (内容,弹窗类型,持续时间)



### #@setParam(~)

**设置参数**

```enjoy
#@setParam(["LINE_ID","solve_type"]): 设置参数 "LINE_ID" 和 "solve_type"。
```

等同于

```
#set(LINE_ID=param.LINE_ID)
#set(solve_type=param.solve_type)
```



### #@setReqLog()

```enjoy
#define setReqLog()
#@setAttribute("setNoLog",null)
#@setAttribute("setNoSqlLog",null)
#@setAttribute("setNoPlumeSqlLog",null)
#end
```

- `#@setAttribute("setNoLog",null)`将请求的"setNoLog"属性设置为null，表示不记录日志。
- `#@setAttribute("setNoSqlLog",null)`将请求的"setNoSqlLog"属性设置为null，表示不记录SQL日志。
- `#@setAttribute("setNoPlumeSqlLog",null)`将请求的"setNoPlumeSqlLog"属性设置为null，表示不记录Plume SQL日志



### #sql_query_list("his")

```enjoy
#sql_query_list("his")
#[[
select * from dictmanage.dict_zjpmachine_relationship
where sjip=#para(ip) and wind = #para(wind)
]]#
#end
```
查询his数据库，结果存在list中

**示例1**
#sql_query_list("his",10,0)

**分页查询**

```enjoy
#sql_query_list("his", 10, 0)
#[[
SELECT * FROM student
]]#
#end
```

- 第一个参数(可选):数据库名称,默认为"main"。
- 第二个参数(可选):查询结果的最大数量,默认为0,表示查询所有结果。
- 第三个参数(可选):查询结果的起始索引,默认为0。



### #sql_query_obj ("his")

用于执行SQL查询并将结果存储为一个单个对象



### #sql_query_record ("his")

用于执行SQL查询并将结果存储为一个单个记录



### #sql_update_rs ("his")

```enjoy
#sql_update_rs("his")
#[[
UPDATE student SET age = 20 WHERE id = 1
]]#
#end

#sql_update_rs("his")
#[[
insert into dictmanage.dict_zjpmachine_relationship(machineip,zjpip,del_flag,cjsj,cjr,zhgxsj,zhgxr,sjip,
wind
)
values('auto',#para(ip+"-"+wind),'1',sysdate,'',sysdate,'',#para(ip),
#para(wind)
)
]]#
```


### #sql_while_list ("his")



### #sql_pro_out ("his")



### #sql_trans_rs ("his") 



### #sql_transblock_rs ("his")



### #sql_readblock_rs ("his")

初始变量
`D:\work\api-view-base\jarboot\services\api-view-base\webapp\WEB-INF\api\api_login\_init`

定时任务
`D:\work\api-view-base\jarboot\services\api-view-base\webapp\WEB-INF\api\api_login\_schedule`



### #@sys_get_timeout (~)

获取全局定时参数

sys_get_timeout(p_config_type,p_config_key,p_isupdate)

1. `p_config_type`：配置类型。
2. `p_config_key`：配置键。
3. `p_isupdate`：是否更新标志。

- 示例

```enjoy
#@sys_get_timeout("his_pdjh", ip,false)
```

"his_pdjh"`：表示配置类型为 "his_pdjh"。

ip`：表示配置键为 "ip"。

`false`：表示是否更新标志为 false，即不需要更新。



### #@sysout(~)  打印

打印到控制台



## T

### #@throwRs(p_content_type, p_rs_flag)

###  #@throwBusi(~)



## 获取时间

```enjoy
#set(week_index=cn.hutool.core.date.DateUtil::dayOfWeek(dbutil.getNow()))
#set(week_dict=["星期日","星期一","星期二","星期三","星期四","星期五","星期六"])
#set(week=week_dict[week_index-1])

```



## 示例代码

```enjoy
#bodyget_rs("1")
#render("list.html")
#end
```

```enjoy
#set(jx_entity = {
    zsmc: "",
    name: "",
    ks: "",
    js: "",
    jzr1_content: "",
    jzr2_content: "",
    jzr2_content1: "",
    jzr2_content_status: "",
    jzr3_content: "",
    jzr3_content1: "",
    jzr3_content_status: ""
})

#set(jx_entity.put("jzr2_content_status", "(优先)"))
```

要用put赋值，不能直接写等于号



## 设置带超时时间的系统级别缓存

```enjoy
#sys_set_timeout ("his_pdjh", ip, kitutil.currentTimeMillis(), 3000)
```

用于设置一个带有超时时间的系统级别缓存。它接受四个参数：
- p_config_type：配置类型
- p_config_key：配置键 
- p_df：默认值
- p_timeout：超时时间（毫秒）

调用 setSysUidByValue 函数来设置缓存值，如果超时时间内没有获取到数据，则会移除缓存

#@sys_get_timeout("his_pdjh", ip,false)



## 获取数组长度

list.size()



## 自定义代码位置

D:\work\api-view-base\jarboot\services\api-view-base\webapp\WEB-INF\common\_def.html



##  心跳

- 示例

```enjoy
if(kp_arr.size()>9)
#@cf_get("his_pdjh.track_time","排队叫号诊间屏切换时间","50000")
#set(g_time=dbutil.toInteger(rs))
#set(g_flag=dbutil.currentTimeMillis())
#set(g_flag=g_flag%g_time)
#if(g_flag < g_time/2)
#set(cur_data=kp_arr.subList(0, 9))
#else
#set(cur_data=kp_arr.subList(9,kp_arr.size()))
#end
```



## 数组剪切

#set(cur_data=kp_arr.subList(g_index, g_index+g_size))

#  工具

## dbutil

### dbutil.getNowStr("yyyy-MM-dd") 获取日期

#set(dqsj=dbutil.getNowStr("yyyy-MM-dd")): 获取当前日期，并格式化为指定格式的字符串，赋值给变量 "dqsj"。



## kitutil

### kitutil.toJSONObject(rs)

#set(json=kitil.toJSONObject(rs))



### kitutil.toList(obj)



### kitutil.getStr(~)



## TableDbUtil

### TableDbUtil::insert(~)

```enjoy
insert(Map<String, Object> jsonMap, String configName, String tableName)
```

1. jsonMap - 包含要插入的数据的 Map 对象。

2. configName - 数据库配置名称。

3. tableName - 要插入数据的目标表名。

- 示例

```enjoy
#set(com.ld.plug.TableDbUtil::insert(param,"his","dictmanage.dict_linetype"))
```





# enjoy语法



# 初始化变量

```enjoy
query_param:  查询参数,url的?后面的部分
```



# other

## 开始流程

```enjoy
#@setCross()  //允许跨域等

#set(method=query_param.method??"") //设置method变量
#if(method.indexOf("?")>=0)
#@throwBusi("method is error")
#end
```

## 数据库删除

```enjoy
#@setParam(["name"])

#sql_update_rs("his")
delete from his.test1 where name = #@para("name") and rownum=1
#end

#@msg("")
```

