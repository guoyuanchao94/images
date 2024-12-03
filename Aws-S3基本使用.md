# <center>Aws S3服务的基本使用


## 1、安装并配置凭证
### 安装AWS CLI
1. 想要了解AWS CLI是什么,请参阅[什么是AWS CLI](https://docs.aws.amazon.com/zh_cn/cli/latest/userguide/cli-chap-welcome.html)
2. 直接安装AWS CLI,Windows平台,[AWS CLI下载链接](https://awscli.amazonaws.com/AWSCLIV2.msi)
3. 具体安装请参阅[AWS CLI安装步骤](https://docs.aws.amazon.com/zh_cn/cli/latest/userguide/getting-started-install.html)

### 配置凭证
1. 打开Windows PowerShell,输入aws configure,开始输入凭证
2. 按照提示依次输入AWS Access Key ID、AWS Secret Access Key、Default region name即可
3. 详细配置过程请参阅,[为配置设置AWS CLI](https://docs.aws.amazon.com/zh_cn/cli/latest/userguide/cli-chap-configure.html)
### 配置结果
1. 凭证配置完成后,可以在~/.aws目录下找到config和credentials文件
2. 使用文本编辑器打开config文件可以看到配置的region
3. 使用文本编辑器打开credentials文件可以看到配置的aws_access_key_id和aws_secret_access_key
### 检验凭证
1. 使用文本编辑器打开~/.aws目录下的config文件和credentials文件看到凭证信息,通过比较凭证信息可以检验凭证是否有误
2. 打开Windows PowerShell,输入aws configure list命令,可以看到我们进行的一些配置,如![配置图片](C:\Users\20271\Desktop\Snipaste_2024-12-03_14-44-33.png "配置信息")
3. 打开Windows PowerShell,输入aws s3 ls可以检验凭证是否有误,如果凭证无误,可以显示我们的存储桶的桶名信息
4. 打开Windows PowerShell,输入aws sts-get-caller-identity,如果凭证无误,可以看到以Json格式显示的身份信息
## 2、编译源码

1. 源码下载

## 3、静态库和动态库的使用

## 4、注意事项

## 5、示例代码

