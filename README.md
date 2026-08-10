# 夸克自动签到

## 抓包流程
【手机端】
1. 打开[reqable](https://reqable.com/zh-CN/)抓包工具(苹果用stream)，手机端访问签到页。
2. 搜索找到域名 `https://drive-m.quark.cn/1/clouddrive/capacity/growth/info` 的请求信息
   ![抓包](docs/抓包.jpg)
3. 复制url后面的参数: kps sign vcode pr fr
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
1. 打开 Fork 后的仓库，进入 **Settings -> Secrets and variables -> Actions**。
2. 点击 **Repository secrets** 分区下的 **New repository secret** 按钮。
![img.png](docs/secrets.png)


## 工作流 workflow 设置

### 启用 GitHub Actions 及设置权限
1. 打开 Fork 后的仓库，进入 **Actions** 选项卡。如果看到黄色的提示条 "Workflows aren't right ....... enable them"，点击 **"I understand my workflows, go ahead and enable them"** 按钮启用 Actions。
2. **重要：设置 Workflow 权限**
   * 进入仓库的 **Settings -> Actions -> General** 页面。
   * 在 "Workflow permissions" 部分，选择 **"Read and write permissions"**。
   * 点击 "Save" 保存。
   * **此步骤是必需的**，以便 Actions 能够执行“保持仓库活跃”（空提交）和“清理旧的工作流记录”等操作。
3. 启用后，你会看到名为 `Quark签到` (或 `Quark Sign-in`) 的工作流已配置完成。
4. 脚本将按预设时间（北京时间每日约 8点08 和 13:00）自动运行。
   * **运行时间说明**：默认设置在北京时间上午 8点08 点和下午 1 点左右运行。由于 GitHub Actions 的计划任务调度机制，实际运行时间可能会有几分钟到几十分钟的延迟，这是正常现象。随机延迟的加入也会影响确切的启动时间。
   * **执行逻辑**：脚本会先检查当天是否已成功签到。如果已签到，则跳过后续的签到操作。

### 手动测试运行
1. 进入 **Actions** 选项卡，点击左侧的 `Quark签到` (或 `Quark Sign-in`) 工作流。
2. 点击右侧的 **Run workflow** 按钮，然后再次点击绿色 **Run workflow** 按钮，手动触发任务以验证配置是否成功。
3. 你可以点击运行中的 workflow 查看其执行日志和状态。


## 签到说明
- **每日自动签到**：定时运行脚本完成每日签到，领取成长奖励。
  - **新增：每日两次签到尝试**：分别在北京时间早上 8点08和下午 1 点左右尝试签到，增加成功率。
  - **新增：防止重复签到**：脚本会记录当日成功签到状态，避免不必要的重复执行。
  - **新增：随机延迟执行**：每次签到前加入随机延迟，模拟人工操作，降低被检测风险。
- **GitHub Actions 托管**：一键配置后，脚本每天自动运行，实现真正的“一劳永逸”。
  - **新增：自动保持仓库活跃**：通过空提交防止 GitHub因仓库不活跃而禁用 Actions。
  - **新增：自动清理旧记录**：自动删除旧的 Workflow 运行记录，保持 Actions 页面整洁。

## 参考开源项目

[Auto_Check_In](https://github.com/BNDou/Auto_Check_In/blob/main/checkIn_Quark.py)
