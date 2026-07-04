# 夸克自动签到

## 配置

抓包流程：
【手机端】

1. 打开[reqable](https://reqable.com/zh-CN/)抓包工具(苹果用stream)，手机端访问签到页。
2. 搜索找到域名 `https://drive-m.quark.cn/1/clouddrive/capacity/growth/info` 的请求信息
   ![抓包](docs/抓包.jpg)
3. 复制url后面的参数: kps sign vcode pr fr并设置到环境变量中
   ![参数](docs/复制.png)
4. pr指登录的app(夸克为ucpro，夸克网盘为qk_clouddrive，配置一次后不用改)；fr指登录的设备(android或iphone,不用更改)
5. kps sign vcode有时效，初步测试有效期在两个月左右

## 配置环境变量

### 必要配置

|    抓包的参数    |      说明      |
|:-----------:|:------------:|
|  QUARK_KPS  | 抓包参数的kps  |
| QUARK_SIGN  | 抓包参数的sign |
| QUARK_VCODE |  抓包参数的vcode  |
| QUARK_PR  | 抓包参数的pr(ucpro或qk_clouddrive，夸克/夸克网盘) |
| QUARK_FR |  抓包参数的fr(android或iphone)  |

注：**QUARK_PR和QUARK_FR配置在variables中**；另外variables还添加了QUARK_USER(随便填写名字，用以区分账号)。
其他都为secrets。
   
### 可选配置
#### 1.邮箱通知
> qq邮箱可访问网页的，然后点击头像，进入账号与安全，如图获取授权码即密码
> 服务器：smtp.qq.com 端口：587
> **其他邮箱自行搜索**
![邮箱](docs/邮箱配置.png)


|   邮件通知配置    |    说明     |
|:-----------:|:---------:|
| SMTP_SERVER | smtp服务器地址 |
|  SMTP_PORT  |   服务器端口   |
|    EMAIL    |  接收通知的邮箱  |
|  PASSWORD   |  邮箱的授权码   |

#### 2.server酱推送
打开 [https://sct\.ftqq\.com/](https://sct.ftqq.com/) 登录获取 `SCTxxxx` 密钥，备用
|   配置    |    说明     |
|:-----------:|:---------:|
| SERVER_KEY | sever酱的key |

## Github Actions自动签到配置
![img.png](docs/secrets.png)

## 参考开源项目

[Auto_Check_In](https://github.com/BNDou/Auto_Check_In/blob/main/checkIn_Quark.py)
