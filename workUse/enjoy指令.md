# 自定义代码

## B

### #bodyget_rs()

- **示例1**

```enjoy
#bodyget_rs()
{
  json
}
```
渲染模板字符串，赋值给rs

- **示例2**

```enjoy
#bodyget_rs("1") 
```
如果type参数为"1"，并且发生了MsgException异常，那么异常消息会被存储到scope中的"rs"变量中。否则会抛出异常

- **示例3**

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
#bodyget_rs(res)   ### 结果存在res里
xxxxx
#end    
```



### \#@busi_get_cache(~)

从缓存中获取业务数据

```enjoy
#define busi_get_cache(p_table,p_param,p_value,p_var)
```

- `p_table`: 表名
- `p_param`: 参数名
- `p_value`: 参数值
- `p_var`: 存储结果的变量名





- 源码

```enjoy
#define busi_get_cache(p_table,p_param,p_value,p_var)
### 设置缓存的超时时间 p_busi_timeout 的默认值为 "30000" (30秒)。
#set(p_busi_timeout="30000")
### 如果存在变量 busi_cache_timeout,则使用 busi_cache_timeout 的值作为缓存超时时间。
#if(busi_cache_timeout!=null)
#set(p_busi_timeout=busi_cache_timeout)
#end
#cache_plug(p_busi_timeout,"//busi_get_cache",["p_param","p_table","p_value"],[p_var])
#@busi_get_obj(p_table,p_param,p_value,p_var)
#end
#end
```



### #@busi_get_obj(~)

根据给定的表名、字段名和字段值,从数据库中查询符合条件的记录,并将查询结果赋值给指定的变量

```enjoy
#@busi_get_obj(p_tablename,p_ids,p_values,p_var)
#@busi_get_obj("some_table",["field1", "field2"],[value1, value2],"result")
```

接受四个参数

1. `p_tablename`:要查询的表名

2. `p_ids`:要作为查询条件的字段名列表

3. `p_values`:对应字段的值列表

4. `p_var`:用于存储查询结果的变量名

   

- 示例1

```enjoy
#@busi_get_obj("dictmanage.dict_outpat_clinic",["ZS_ID"],[zs_id],"outpat_clinic")
```

第二个参数 ["ZS_ID"] 表示一个只含有一个元素 "ZS_ID" 的列表,这个列表代表了要作为查询条件的字段名。

第三个参数 [zs_id] 同样表示一个只含有一个元素 zs_id 的列表,这个列表代表了对应字段的值。

- 示例2

```enjoy
#@busi_get_obj("dictmanage.dict_outpat_clinic",["ZS_ID", "CLINIC_CODE"],[zs_id,clinic_code],"outpat_clinic")

### SELECT * FROM dictmanage.dict_outpat_clinic WHERE ZS_ID = ? AND CLINIC_CODE = ?
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

查询配置项，结果存入rs

```enjoy
#@cf_get("sys.log.save.type","日志保存方式","1")
```

**查询系统配置**

**ldpai配置/系统配置**

#@cf_get(p_key,p_name,"p_value")

- `p_key`: 配置项的名称。
- `p_name`: 配置项的描述。如果该配置项不存在将这个值存入。
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



## H

### #http_post_rs(~)

```enjoy
### 构建建档接口的请求JSON参数
#bodyget_rs(json_param)
{
  "patientInfo": {
  "RZ_FLAG": "1",
  "JZKH": "", 
  "PAT_NAME": "#@toJsonStr(name)", 
  "CS_RQ":"#@toJsonStr(csrq)", 
  "XB_ID": "#@toJsonStr(sex)", 
  "HY_ID": "", 
  "PAT_XZID": "1000", 
  "GJ_ID": "",
  "XX_ID": "", 
  "MZ_ID": "#@toJsonStr(mzid)", 
  "PROVINCE_ID": "", 
  "CITY_ID": "", 
  "AREA_ID": "", 
  "TOWN_ID": "", 
  "JT_DZ": "#@toJsonStr(address)", 
  "ZJLX_ID": "01", 
  "ZJ_HM": "#@toJsonStr(zjid)", 
  "ZY_ID": "", 
  "JT_DH": "#@toJsonStr(tel)", 
  "GZ_DW": "", 
  "PSN_NO": "", 
  "YBKH": "", 
  "INSUPLC_ADMDVS": "", 
  "LXR": "#@toJsonStr(lxr)", 
  "GX_ID": "#@toJsonStr(lxr)", 
  "LXR_SFZH": "", 
  "LXR_DH": "", 
  "RZR": "TJ", 
  "LXR_DZ": "", 
  "BIRTHPLACE": "", 
  "BIRTHPLACE_CODE": "", 
  "BIRTHPLACE_PROVINCE": "",
  "BIRTH_PLACE_PROVINCE_CODE": "", 
  "BIRTHPLACE_CITY": "",
  "BIRTH_PLACE_CITY_CODE": "", 
  "BIRTHPLACE_COUNTY": "",
  "BIRTH_PLACE_COUNTY_CODE": "", 
  "HOME_ADDRESS_CODE": "", 
  "HOME_ADDRESS": "", 
  "CURRENT_ADDRESS_CODE": "", 
  "CURRENT_ADDRESS": "", 
  "CURRENT_ADDRESS_DETAILS": "", 
  "CURRENT_ADDRESS_PROVINCE": "", 
  "CURRENT_ADDRESS_PROVINCE_CODE": "", 
  "CURRENT_ADDRESS_CITY": "", 
  "CURRENT_ADDRESS_CITY_CODE": "", 
  "CURRENT_ADDRESS_COUNTY": "", 
  "CURRENT_ADDRESS_COUNTY_CODE": "", 
  "CURRENT_ADDRESS_TOWN": "", 
  "CURRENT_ADDRESS_TOWN_CODE": "", 
  "BUSINESS_ADDRESS": "", 
  "BUSINESS_ADDRESS_CODE": "", 
  "BUSI_ADDRESS_PROVINCE": "", 
  "BUSI_ADDRESS_PROVINCE_CODE": "", 
  "BUSI_ADDRESS_CITY": "", 
  "BUSI_ADDRESS_CITY_CODE": "", 
  "BUSI_ADDRESS_COUNTY": "", 
  "BUSI_ADDRESS_COUNTY_CODE": "", 
  "BUSI_ADDRESS_TOWN": "", 
  "BUSI_ADDRESS_TOWN_CODE": "", 
  "CONTACT_ADDRESS": "", 
  "CONTACT_ADDRESS_CODE": "", 
  "CONTACT_ADDRESS_PROVINCE": "", 
  "CONTACT_ADDRESS_PROVINCE_CODE": "", 
  "CONTACT_ADDRESS_CITY": "", 
  "CONTACT_ADDRESS_CITY_CODE": "", 
  "CONTACT_ADDRESS_COUNTY": "", 
  "CONTACT_ADDRESS_COUNTY_CODE": "", 
  "CONTACT_ADDRESS_TOWN": "", 
  "CONTACT_ADDRESS_TOWN_CODE": "", 
  "NATIVE_PLACE": "", 
  "NATIVE_PLACE_PROVINCE": "", 
  "NATIVE_PLACE_PROVINCE_CODE": "", 
  "NATIVE_PLACE_CITY": "",
  "NATIVE_PLACE_CITY_CODE": "", 
  "NATIVE_PLACE_COUNTY": "", 
  "NATIVE_PLACE_COUNTY_CODE": "" 
  }
}
#end
#http_post_rs("http://192.168.10.51:8088/ld/common/dict/PatientInfo?method=createPatientInfo")
###把参数转发出去
#(str(json_param))
#end
```





## L

### #@list_record(~)

用于从给定的列表 `p_list` 中获取第一条记录,并将其赋值给变量 `record`

- 示例

```enjoy
#@list_record(list)
#set(pat_record=record)
```

- 源码

```enjoy
#define list_record(p_list)
#set(record=kitutil.getRecord())
#if(!isEmpty(p_list))
#set(record=p_list[0])
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



### #@setReqNoAllLog()

不记录日志

```enjoy
#define setReqNoAllLog()
#@setAttribute("setNoLog","1")
#@setAttribute("setNoSqlLog","1")
#@setAttribute("setNoPlumeSqlLog","1")
#end
```



### #@setReqLog()

记录日志

```enjoy
#define setReqLog()
#@setAttribute("setNoLog",null)
#@setAttribute("setNoSqlLog",null)
#@setAttribute("setNoPlumeSqlLog",null)
#end
```

- `#@setAttribute("setNoLog",null)`将请求的"setNoLog"属性设置为null，表示记录日志。
- `#@setAttribute("setNoSqlLog",null)`将请求的"setNoSqlLog"属性设置为null，表示记录SQL日志。
- `#@setAttribute("setNoPlumeSqlLog",null)`将请求的"setNoPlumeSqlLog"属性设置为null，表示记录Plume SQL日志



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



### #@toJsonStr(~)

等同于kitutil.toJSONString(~)

- 源码

```enjoy
#define toJsonStr(str)
	#(kitutil.toJSONString(str))
#end
```



### #@toSqlStr(~)

等同于org.apache.commons.lang.StringEscapeUtils::escapeSql(~)

用于对 SQL 查询中的特殊字符进行转义，以防止 SQL 注入攻击

- 源码

```enjoy
#define toSqlStr(str)
	#(org.apache.commons.lang.StringEscapeUtils::escapeSql(str))
#end
```



### #@toUrlStr(~)

如果str不为空，执行java.net.URLEncoder::encode(str, "utf-8")

将字符串 `str` 按照 UTF-8 字符集进行 URL 编码

- 源码

```enjoy
#define toUrlStr(str)
	#if(str!=null && str!="")
		#(java.net.URLEncoder::encode(str, "utf-8"))
	#end
#end
```









## V

### \#@validate_blank_param(~)

对传入的参数数组进行验证,确保每个参数都不为空。如果有任何一个参数为空,就会抛出相应的业务异常,提示该参数不能为空

- 示例

```enjoy
#@validate_blank_param(["sfz_hm","pat_name"])
```

- 源码

```enjoy
#define validate_blank_param(arr)
#for(item : arr)
#if(param[item]==null || isBlank(param[item].toString()))
#@throwBusi(item+"不为空")
#end
#end
#end
```



### #@validate_in_param(~)

函数的作用是检查param里的 `arr` 中的每个参数是否在 `in_arr` 指定的范围内,如果不在,则抛出一个业务异常

```enjoy
validate_in_param(arr,in_arr)
```

1. `arr`: 需要验证的参数名称,可以是一个字符串或者一个字符串数组。
2. `in_arr`: 指定的合法值范围,是一个数组。



- 示例

```enjoy
#@validate_in_param(["type"],["today","all"])
### 判断param.type的值是否为 "today"或者"all"
```



# 自定义视图

## 调用自定义视图

- 示例

```enjoy
#sql_query_list("his")
select * from #@his_v_mzpb()
#end
```



## his_v_mzpb()

- 示例

```enjoy
#sql_query_list("his")
select distinct substr(his.f_pinyin(zs_mc),0,1) pym from (
select distinct zs_id,zs_mc,ks_hz,hz_mc
from #@his_v_mzpb()
where
#if(rq!="")
to_char(checkdt,'yyyy-MM-dd')=#@para("rq") and
#end
(
(
sj_id='0'
and
to_char(sysdate,'hh24:mi') <= '12:00'
)
or
sj_id = '1'
or  checkdt >=trunc(sysdate+1)
)
and
checkdt >= trunc(sysdate)
and sj_id in ('0','1')
#if(ks_id!="" && ks_id!='jzflag')
and ks_hz = #@para("ks_id")
#end
and checkdt <= trunc(sysdate + #(ts))
) order by pym
#end
```











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

### dbutil.getNowStr(~) 

获取日期

- 示例

```enjoy
#set(dqsj=dbutil.getNowStr("yyyy-MM-dd"))   ### 获取当前日期，并格式化为指定格式的字符串，赋值给变量 "dqsj"。
```

```enjoy
#set(dqsj=dbutil.getNowStr("HH:mm"))
```



### dbutil.toDate(~)

转换为日期类型

-  示例

```enjoy
#set(cs_rq=cn.hutool.core.util.IdcardUtil::getBirthByIdCard(sfz_hm)) ### 根据身份证号获取出生日期
#set(cs_rq=dbutil.toDate(cs_rq))  ### 转换为日期类型
```

### dbutil.getNow()

获取当前时间

- 示例

```enjoy
#set(now=dbutil.getNow())  
```



## kitutil

### kitutil.getStr(~)

```enjoy
#set(cardInfo_sex=kitutil.getStr(param.sex))
```





### kitutil.toJSONObject(rs)

 

- 示例

```enjoy
#set(json=kitil.toJSONObject(rs))  
```



### kitutil.toList(obj)



### 



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



## IdcardUtil

### isValidCard(~)

验证身份证号是否合法

- 示例

```enjoy
#if(cn.hutool.core.util.IdcardUtil::isValidCard(sfz_hm)) 
```

### getBirthByIdCard(~)

根据身份证号获取出生日期

- 示例

```enjoy
#set(cs_rq=cn.hutool.core.util.IdcardUtil::getBirthByIdCard(sfz_hm))
```



# enjoy语法



# 初始化变量

```enjoy
query_param:  查询参数,url的?后面的部分
```



## Param

```enjoy
#set(param.put("PYM",pym.toUpperCase()))  ### 将参数存入param
```



# other

## 开始流程

```enjoy
#@setCross()  ### 允许跨域等

#set(method=query_param.method??"") ### 设置method变量
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



# temp

_def   D:\work\api-view-base\jarboot\services\api-view-base\webapp\WEB-INF\common\_def.html
