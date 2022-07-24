# IPWarden

在开始使用之前，请务必阅读并同意[免责声明](Disclaimer.md)中的条款，否则请勿下载使用本工具。

下载地址:https://github.com/EnnioX/IPWarden/releases/tag/IPWarden

## 简介

IPWarden是一个面向IP的攻击面监测工具，可循环扫描IP资产，更新暴露项数据。在主机、端口&协议发现与风险端口管理的基础上，探测Web资产并对Web资产进行组件指纹扫描、CMS识别、管理后台识别、ssl证书收集、xray漏洞扫描。本工具特点为所有扫描结果均可通过API返回json数据和导出为xlsx表格，方便二次开发与数据加工。适合甲方安全人员管理公网/内网IP资产风险暴露面。集成的工具有:nmap、masscan、TideFinger、xray

PS:Warden是War3中的英雄守望者的英文名，纪念一下沉迷魔兽的小学年代

## 功能

1. 主机、端口&协议发现
2. Web站点探测
3. Web组件指纹信息收集
4. Web CMS识别
5. Web管理后台识别
6. Xray漏洞扫描
7. SSL证书信息扫描
8. 首页汇总数据生成统计图概览
9. 数据汇总xlsx文件导出
10. 风险端口常见漏洞POC测试（开发中）

## 首页截图

1 .端口与协议发现
   ![端口发现](./img/port.png)
   ![协议发现](./img/protocol.png)
2 .风险端口与协议发现
   ![风险端口发现](./img/riskport.png)
   ![风险协议发现](./img/riskprotocol.png)
3 .Web指纹收集
   ![Web指纹收集](./img/webfinger.png)
4 .Web后台站点占比与全部站点响应码占比
   ![Web信息](./img/bing.png)
5 .开放Web服务端口统计
   ![Web信息](./img/webport.png)
6 .xray扫描规则统计
   ![xay扫描风险](./img/xray.png)
7 .Web ssl证书扫描
   ![SSL证书](./img/ssl.png)

## API

| 序号 | Api用途                  | 方法 | url                           | 请求参数     | 返回字段                                                  | 返回格式 |
| ---- | ------------------------ | ---- | ----------------------------- | ------------ | --------------------------------------------------------- | -------- |
| 1    | 查询全部IP开放端口数据   | GET  | http://127.0.0.1/portsdata    | 无           | ip, port, protocol, updatetime                            | json     |
| 2    | 查询指定ip开放的端口     | GET  | http://127.0.0.1/ip=?         | ip(string)   | port, protocol, updatetime                                | json     |
| 3    | 查询开放指定端口的ip     | GET  | http://127.0.0.1/port=?       | port(string) | ip, updatetime                                            | json     |
| 4    | 查询全部风险端口数据     | GET  | http://127.0.0.1/riskports    | 无           | 同序号1                                                   | json     |
| 5    | 查询白名单外风险端口数据 | GET  | http://127.0.0.1/newriskports | 无           | 同序号1                                                   | json     |
| 6    | 查询SSL证书数据          | GET  | http://127.0.0.1/ssl          | 无           | ip, url, common_name, start_date, expire_date, updatetime | json     |
| 7    | Web站点探测              | GET  | http://127.0.0.1/web          | 无           | ip, url, title, backstage, updatetime                     | json     |
| 8    | Web Finger信息           | GET  | http://127.0.0.1/webfinger    | 无           | url, title, webfinger, updatetime                         | json     |
| 9    | Web管理后台站点探测      | GET  | http://127.0.0.1/backstage    | 无           | 同序号7                                                   | json     |
| 10   | Xray扫描                 | GET  | http://127.0.0.1/xray         | 无           | url, payload, plugin, request, updatetime                 | json     |
| 11   | Web cms信息              | GET  | http://127.0.0.1/cms          | 无           | url, cms, title, updatetime                               | json     |
| 12   | 下载xlsx     | GET  | http://127.0.0.1/xlsx         | 无           |                                                           | xlsx     |

### API返回参数说明
```
   ip : ip地址(str)
   port : 端口(str)
   protocol : 端口协议(str)
   url : 访问地址(str)
   common_name : ssl证书名称(str)
   start_date : ssl证书开始日期(str)
   expire_date : ssl证书结束日期(str)
   title : 网站标题(str)
   backstage : 如果值为yes识别为web管理后台，否则为no(str)
   webfinger : web指纹资产,如"nginx"(str)
   cms : web cms识别,如ThinkPHP(str)
   payload : xray扫描poc(str)
   plugin : xray扫描规则(str)
   request : xray扫描http请求(str)
   updatetime : 扫描更新时间(str)
```

### Web站点探测API返回示例（http://127.0.0.1/web）

```
[
   {
      "ip": "192.168.1.1"
      "url": "http://192.168.0.1:7070/"
      "title": "巧克力真好吃"
      "backstage": "no"
      "updatetime": "2022-07-13 13:13:58"
   }
   {
      "ip": "192.168.1.2"
      "url": "http://example.com/"
      "title": "XXX管理后台"
      "backstage": "yes"  # 值为yes代表识别为管理后台
      "updatetime": "2022-07-13 13:13:58"
   }
]
```

### xray扫描API返回示例（http://127.0.0.1/xray）

```
[
   {
      "url": "https://1x3.x1.xx0.2x8:443/"
      "payload": "/admin.do"
      "plugin": "dirscan/admin/default"
      "request": "GET /admin.do HTTP/1.1\r\nHost: 1x3.x1.xx0.2x8:443\r\nUser-..."
      "updatetime": "2022-07-13 19:11:03"
   }
   {
      "url": "http://x4.x3.1x2.x99/admin/login.jsp"
      "payload": ""
      "plugin": "baseline/sensitive/server-error"
      "request": "GET /admin/mailsms/s?func=ADMIN:appState&dumpConfig=/ HTTP/1.1\r\nHo..."
      "updatetime": "2022-07-13 19:18:00"
   }
]
```

## 部署方式

### 部署前环境准备
1 .Linux

2 .python3

3 .mysql或mariadb数据库(utf-8编码)

注意；如果扫描公网IP，建议使用有独立公网IP的云服务器或者调低masscan扫描线程小于1000，否则可能会影响局域网出口网络，内网扫描可忽略这点

### 部署过程
1 .解压拷贝到服务器后，对项目文件夹赋权（测试环境直接梭哈777）
```
chmod -R 777 IPWarden
```

2 .配置文件修改:进入IPWarden目录，工具有2个配置文件，分别为设置系统服务端口和数据库连接的serverConfig.py和设置扫描参数的scanConfig.py

serverConfig.py

```
# 系统基础参数
API_PORT = 80  # 设置为希望开放的服务端口

# mysql配置
MYSQL_HOST = '127.0.0.1'  # 此处修改为数据库IP地址
MYSQL_PORT = 3306  # 此处修改为数据库端口
MYSQL_USER = 'root'  # 数据库连接用户名
MYSQL_PASSWORD = 'password'  # 数据库连接密码
MYSQL_DATABASE = ''  # 库名
```

scanConfig.py

```
# masscan参数
SCAN_IP = '192.168.1.1,10.0.8.0/24,10.0.1.110-10.0.1.150'  # 选择扫描的目标IP，同masscan参数格式
SCAN_PORT = '1-65535'  # 设置扫描的端口范围，同masscan参数格式
RATE = '10000'  # 扫描线程
SCAN_WHITE_LIST = ''  # 不扫描的ip白名单,格式同SCAN_IP

# 要进行Web探测的端口
WEB_PORT = ['80-10000', '50000']  # 按需设置，暴力设置1-65535也可

# Web管理后台关键词
WEB_BACKSTAGE = ['login', 'admin', '登录', '管理后台', '系统后台', '管理系统']

# 风险端口白名单,使用序号5api请求返回数据不包含以下数据
RISK_PORT_WHITE_LIST = [['192.168.86.14', '3306'],['192.168.86.13', '22']]

# 自定义风险端口
RISK_PORT_LIST = ['21','22','3389'...]  # 可采用配置文件中默认数据
```

3 .配置文件修改好后，建议先更新pip源，如已更新可跳过

```
pip3 install --upgrade pip -i http://pypi.douban.com/simple --trusted-host pypi.douban.com
```

4 .在IPWarden目录下使用如下命令后台执行runIPWarden.py开始循环监控，更改scanConfig.py配置文件无需重启服务，下一扫描周期自动重载。如更改serverConfig.py配置文件后需要先用kill.sh停止服务再重新执行命令重启

```
nohup python3 runIPWarden.py &
```

停止服务：在IPWarden目录下使用如下命令执行kill.sh文件

```
./kill.sh
```

服务启动后，默认循环启动所有扫描（可在runIPWarden.py文件自行修改选择模块，web_scan.py是后续web相关扫描的基础模块），坐等以后通过api收集数据和看首页统计图就好。如果目标ip较多，每轮扫描的时间会比较长，第一次扫描建议一天后再回来看结果

## 写在最后

欢迎添加开发者微信交流：Ennio404

