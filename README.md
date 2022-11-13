# <font color=Red>原作者：[@SAOJSM](https://github.com/SAOJSM/EU_CHICK_EXTEND_CHT)  Salute!</font>
  	1.本文修改自SAOJSM，作流程说明，简单修改，个人备份，Star请给原作者
  	2.仅支持TGBOT通知
  	3.仅支持Gmail邮箱

# 部署流程

## 一、注册[TrueCaptcha](https://truecaptcha.org/)
前往[页面](https://truecaptcha.org/profile.html)获取`userid`和`apikey`（每日免费100次）

## 二、TG机器人注册
按照[小白教程](https://www.haianet.cn/334.html)获取`Token`和`Id`

## 三、获取Gmail Api Token
### 步骤 1
#### 1-1 使用德鸡账户登陆[Google Cloud](https://console.cloud.google.com/)
#### 1-2 左上角三条横`导航菜单`-`API和服务`-`已启用的 API 和服务`-`创建项目`，项目名随意
#### 1-3 `已启用的 API 和服务`-`刚创建的项目`-`启用API和服务`-搜索`Gmail API`并启用
#### 1-4 `已启用的 API 和服务`-`OAuth 同意屏幕`-`创建`-用户名称输入：`GMAIL VERIFY`-填写用户支持电子邮件和开发者联系电子邮件地址为`登陆的邮箱`-`保存并继续`-`保存并继续`-测试用户+ADD USERS 输入`登陆的邮箱`添加-`保存并继续`
#### 1-5 `已启用的 API 和服务`-`凭据-创建凭据-OAuth 客户端 ID-Web 应用`-添加已获授权的重定向 URI`http://localhost:36666/`-创建
#### 1-6 下载JSON，改名为credentials.json

### 步骤 2
#### 2-1 打包仓库文件到win本地，或点击[蓝奏云](https://wwk.lanzoul.com/b01k3s80b)（密码5sq5）下载。
#### 2-2 win配置python环境，上面`蓝奏云`提供python 3.10.7安装包。([小白教程](https://blog.csdn.net/weixin_43831559/article/details/121911854))
<center>

![py.png](https://s2.loli.net/2022/11/12/PmqVFTIkibHUzhZ.png)<br>
配置需注意上图前后顺序，路径最好全英文
</center>

#### 2-3 解压下载的包文件，将`1-6`获取的credentials.json放入包文件目录。这里假设包文件目录为
`E:\Users\Administrator\Desktop\script\euserv`  //请务必在后面的命令中修改成你自己的
#### 2-4 cmd输入以下命令(需科学上网)
```
pip install google-api-python-client
pip install google_auth_oauthlib
pip install socks
pip install -U requests[socks]
```
```
py E:\Users\Administrator\Desktop\script\euserv\gmail_api.py 你的邮箱用户名
```
#### 2-5 输入完以上命令浏览器会自动跳转，登陆后一直点`继续`完成授权。
`The authentication flow has completed. You may close this window.`表示授权完成。
#### 2-6 包文件中会自动生成一个文件"token_你的email.json"

## 部署至vps
### 打开vps，root目录下新建euserv文件夹
#### 将`credentials.json`、`eu.py`、'gmail_api.py'、`token_你的email.json`放入vps euserv文件夹
```
wget https://raw.githubusercontent.com/Zpipishrimp/EU/main/gmail_api.py
wget https://raw.githubusercontent.com/Zpipishrimp/EU/main/eu.py
```

### 修改 eu.py(26-30行)
```
TG_BOT_TOKEN = '你的TG_BOT_TOKEN'
TG_USER_ID = '你的TG_USER_ID'

USERNAME = os.environ.get("EUSERV_USERNAME", "德鸡用户名")  
PASSWORD = os.environ.get("EUSERV_PASSWORD", "德鸡密码") 

TRUECAPTCHA_USERID = os.environ.get("TRUECAPTCHA_USERID", "TrueCaptcha网站获取的userid")
TRUECAPTCHA_APIKEY = os.environ.get("TRUECAPTCHA_APIKEY", "TrueCaptcha网站获取的apikey")
```
### 执行以下命令
```
apt install python3-pip //安装python3,已装跳过
pip3 install --upgrade google-api-python-client google-auth-httplib2 google-auth-oauthlib beautifulsoup4 requests pysocks
pip install beautifulsoup4 //报错No module named 'bs4'时输入，否则跳过
cd /root/euserv
python3 eu.py
```
###成功图
<center>

![tc.png](https://s2.loli.net/2022/11/13/7vZgEBMUAzbSJea.png)
![cc.png](https://s2.loli.net/2022/11/13/aEdLKjcS5C7bpZf.png)
</center>
### 设置定时任务

```
crontab -e 

0,10 17 10 * * /usr/bin/python3 /root/euserv/eu.py

crontab -l  //查看定时是否成功
```
ps:`0,10 17 10 * *`表示每月10号，17:00和17:10分执行脚本。请查看你的德鸡到期时间，修改定时
（!定时两次是为了确保续期成功，第一次运行脚本可能仅登陆成功而没有续期）。

# Token失效
失效TG推送：`Token has been expired or revoled.`

此时只需重复`py E:\Users\Administrator\Desktop\script\euserv\gmail_api.py 你的邮箱用户名`获取,替换`token_你的email.json`
