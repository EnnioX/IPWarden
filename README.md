# IPWarden

### 简介

IPWarden是一个资产探测安全工具，确定目标IP/网段后即可循环扫描监控或单次扫描，扫描产生的数据结果均可通过API获取，方便调用与加工。适合甲方安全人员管理公网/内网资产安全风险暴露面，渗透测试人员用于信息收集和攻击面探测

在开始使用之前，请务必阅读并同意[免责声明](Disclaimer.md)中的条款，否则请勿下载使用本工具。

### 功能

1. 主机、端口发现与风险端口整理
2. 端口协议识别
3. Web站点发现
4. Web指纹信息收集(集成TideFinger)
5. Web管理后台识别
6. Xray漏洞扫描
7. SSL证书信息扫描
8. 主页汇总数据生成统计图

### API


| 序号 | Api用途                  | 方法 | url                               | 请求参数     | 返回字段                                                                                                                                      | 返回格式 |
| ---- | ------------------------ | ---- | --------------------------------- | ------------ | --------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| 1    | 查询全部IP开放端口数据   | GET  | http://172.16.98.138/portsdata    | 无           | ip:ip地址<br />port:端口<br />protocol:协议<br />updatetime:扫描更新时间                                                                      | json     |
| 2    | 查询指定ip开放的端口     | GET  | http://172.16.98.138/ip=?         | ip(string)   | port:端口<br />protocol:协议<br />updatetime:扫描更新时间                                                                                     | json     |
| 3    | 查询开放指定端口的ip     | GET  | http://172.16.98.138/port=?       | port(string) | ip:ip地址<br />updatetime:扫描更新时间                                                                                                        | json     |
| 4    | 查询全部风险端口数据     | GET  | http://172.16.98.138/riskports    | 无           | 同序号1                                                                                                                                       | json     |
| 5    | 查询白名单外风险端口数据 | GET  | http://172.16.98.138/newriskports | 无           | 同序号1                                                                                                                                       | json     |
| 6    | 查询SSL证书数据          | GET  | http://172.16.98.138/ssl          | 无           | ip:ip地址<br />url:访问地址<br />common_name:证书名称<br />start_date:证书开始日期<br />expire_date:证书结束日期<br />updatetime:扫描更新时间 | json     |
| 7    | Web站点探测              | GET  | http://172.16.98.138/web          | 无           | ip:ip地址<br />url:访问地址<br />title:网站标题<br />backstage:是否为管理后台(值为1表示为管理后台)<br />updatetime:扫描更新时间               | json     |
| 8    | Web Finger信息           | GET  | http://172.16.98.138/webfinger    | 无           | url:访问地址<br />title:网站标题<br />webfinger:资产指纹名称(如nginx)<br />updatetime:扫描更新时间                                            | json     |
| 9    | Web管理后台站点探测      | GET  | http://172.16.98.138/backstage    | 无           | 同序号7                                                                                                                                       | json     |
| 10   | Xray扫描                 | GET  | http://172.16.98.138/xray         | 无           | url:访问地址<br />payload:漏洞测试样例<br />plugin:漏洞匹配到的扫描规则<br />request:请求头<br />updatetime扫描更新时间                       | json     |

### 主页截图

1. 端口与协议发现
   ![端口发现TOP10](./img/port.png)
   ![协议发现TOP10](./img/protocol.png)
   ![风险端口发现TOP10](./img/riskport.png)
   ![风险协议发现TOP10](./img/riskprotocol.png)
2. web发现
   ![Web指纹收集TOP10](./img/webfinger.png)
   ![xay扫描风险TOP10](./img/xary.png)
   ![SSL证书TOP10](./img/ssl.png)
