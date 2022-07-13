# IPWarden

在开始使用之前，请务必阅读并同意[免责声明](Disclaimer.md)中的条款，否则请勿下载使用本工具。

## 简介

IPWarden是一个网络风险发现扫描工具，确定目标IP/网段后即可循环扫描监控或单次扫描，扫描产生的数据结果均可通过API获取，方便调用与加工。适合甲方安全人员管理公网/内网资产安全风险暴露面，渗透测试人员用于信息收集和攻击面探测

PS:工作与学习中接触过一些不错的开源安全工具，使用过程中想如果数据结果可以方便脚本调用做二次处理就更好了，所以开发了这个通过调用api返回json格式数据结果的风险发现扫描工具，方便甲方安全人员在扫描结果基础上灵活开发新功能，例如：高危端口发现告警，自动化端口安全测试，ssl证书配置监控等

## 功能

1. 主机、端口发现与风险端口整理
2. 端口协议识别
3. Web站点发现
4. Web指纹信息收集(集成TideFinger)
5. Web管理后台识别
6. Xray漏洞扫描
7. SSL证书信息扫描
8. 主页汇总数据生成柱状统计图

## API


| 序号 | Api用途                  | 方法 | url                               | 请求参数     | 返回字段                                                  | 返回格式 |
| ---- | ------------------------ | ---- | --------------------------------- | ------------ | --------------------------------------------------------- | -------- |
| 1    | 查询全部IP开放端口数据   | GET  | http://172.16.98.138/portsdata    | 无           | ip, port, protocol, updatetime                            | json     |
| 2    | 查询指定ip开放的端口     | GET  | http://172.16.98.138/ip=?         | ip(string)   | port, protocol, updatetime                                | json     |
| 3    | 查询开放指定端口的ip     | GET  | http://172.16.98.138/port=?       | port(string) | ip, updatetime                                            | json     |
| 4    | 查询全部风险端口数据     | GET  | http://172.16.98.138/riskports    | 无           | 同序号1                                                   | json     |
| 5    | 查询白名单外风险端口数据 | GET  | http://172.16.98.138/newriskports | 无           | 同序号1                                                   | json     |
| 6    | 查询SSL证书数据          | GET  | http://172.16.98.138/ssl          | 无           | ip, url, common_name, start_date, expire_date, updatetime | json     |
| 7    | Web站点探测              | GET  | http://172.16.98.138/web          | 无           | ip, url, title, backstage, updatetime                     | json     |
| 8    | Web Finger信息           | GET  | http://172.16.98.138/webfinger    | 无           | url, title, webfinger, updatetime                         | json     |
| 9    | Web管理后台站点探测      | GET  | http://172.16.98.138/backstage    | 无           | 同序号7                                                   | json     |
| 10   | Xray扫描                 | GET  | http://172.16.98.138/xray         | 无           | url, payload, plugin, request, updatetime                 | json     |

## 主页截图

1. 端口与协议发现
   ![端口发现TOP10](./img/port.png)
   ![协议发现TOP10](./img/protocol.png)
   ![风险端口发现TOP10](./img/riskport.png)
   ![风险协议发现TOP10](./img/riskprotocol.png)
2. web发现
   ![Web指纹收集TOP10](./img/webfinger.png)
   ![xay扫描风险TOP10](./img/xary.png)
   ![SSL证书TOP10](./img/ssl.png)
