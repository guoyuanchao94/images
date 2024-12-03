# <center>Aws S3服务的基本使用


## 1、安装并配置凭证
### 安装AWS CLI
1. 想要了解AWS CLI是什么,请参阅[什么是AWS CLI](https://docs.aws.amazon.com/zh_cn/cli/latest/userguide/cli-chap-welcome.html)
2. 直接安装AWS CLI,Windows平台,[AWS CLI下载链接](https://awscli.amazonaws.com/AWSCLIV2.msi)
3. 具体安装请参阅[AWS CLI安装步骤](https://docs.aws.amazon.com/zh_cn/cli/latest/userguide/getting-started-install.html)

### 配置凭证
1. 打开Windows PowerShell,输入`aws configure`,开始输入凭证信息
2. 按照提示依次输入`AWS Access Key ID`、`AWS Secret Access Key`、`Default region name`对应的值即可完成配置过程
3. 详细配置过程请参阅,[为配置设置AWS CLI](https://docs.aws.amazon.com/zh_cn/cli/latest/userguide/cli-chap-configure.html)
### 配置结果
1. 凭证配置完成后,可以在`~/.aws`目录下找到`config`和`credentials`文件
2. 使用文本编辑器打开`config`文件可以看到配置的`region`信息
3. 使用文本编辑器打开`credentials`文件可以看到配置的`aws_access_key_id`和`aws_secret_access_key`信息
### 检验凭证
1. 使用文本编辑器或浏览器打开`~/.aws`目录下的`config`文件和`credentials`文件可以看到凭证信息,通过比较凭证信息可以检验凭证是否有误
2. 打开Windows PowerShell,输入`aws configure list`命令,可以看到我们进行的一些配置,如![配置图片](https://github.com/guoyuanchao94/images/blob/main/config.png?raw=true)
3. 打开Windows PowerShell,输入`aws s3 ls`可以检验凭证是否有误,如果凭证无误,可以显示我们的存储桶的桶名信息,如![存储桶列表](https://github.com/guoyuanchao94/images/blob/main/bucket.png?raw=true)
### 凭证问题
1. Windows PowerShell,输入上述命令如果出现`An error occurred (InvalidAccessKeyId) when calling the ListBuckets operation: The AWS Access Key Id you provided does not exist in our records.`等问题,请参阅[为什么我会收到错误信息 "The AWS Access Key Id you provided does not exist in our records?"](https://aws.amazon.com/cn/getting-started/faq/access-key-id-doesnt-exist-in-records/),观看视频寻找解决方法
## 2、下载并编译源码
### 下载源码
1. 新建下载源码文件夹,在当前文件夹下打开Git Bash,输入或粘贴以下命令 `git clone --recurse-submodules https://github.com/aws/aws-sdk-cpp`,之后回车,等待仓库克隆完成即可
   
### 编译源码
1. 新建源码构建文件夹,在当前文件夹下打开Windows PowerShell或打开终端切换到当前源码构建目录,,输入以下命令,`cmake <path-to-root-of-this-source-code> 
-DCMAKE_BUILD_TYPE=Debug 
-DCMAKE_INSTALL_PREFIX=<path-to-install> -DBUILD_ONLY="s3",`其中`<path-to-root-of-this-source-code>`需要替换为源码下载目录,`<path-to-install>`需要替换为库文件安装目录,同样需要自己创建
1. 等待源码编译完成,同样在当前目录下,执行命令`cmake --build . --config=Debug`并回车
2. 等待源码构建完成,以管理员方式打开命令提示符,切换到源码构建目录,执行以下命令`cmake --install . --config=Debug`,等待命令执行完成,库文件及头文件会被下载到`path-to-install`对应的目录中
3. 注意以上源码只构建与Amazon S3相关的部分,如果需要构建整个SDK,删除`-DBUILD_ONLY="s3"`即可
4. 注意以上生成库为Debug模式,想要生成Release模式,将1.和2.中的命令的Debug切换成Release即可
5. 有关编译源码详细问题请参阅[aws/aws-sdk-cpp](https://github.com/aws/aws-sdk-cpp),有关CMake参数列表,请参阅[CMake参数](https://github.com/aws/aws-sdk-cpp/blob/main/docs/CMake_Parameters.md)

### 直接安装库
1. 考虑到网络问题,下载aws-sdk-cpp的源码时间可能较长并且可能会失败,考虑使用vcpkg下载和安装aws-sdk-cpp
2. 下载vcpkg,新建文件夹,在其中打开Git Bash,执行`git clone https://github.com/Microsoft/vcpkg.git`命令,等待仓库克隆完成,Windows平台下依次执行以下两条命令`./vcpkg integrate install`和`./vcpkg install aws-sdk-cpp`,等待命令执行完成,库文件vcpkg安装目录下的installed目录

## 3、静态库和动态库的使用
1. Windows平台,打开Visual Studio,创建空项目,点击菜单栏`项目`,选择最下面属性,点击进入属性页
2. 选择配置为Debug,平台为x64
3. 配置属性->调试,在环境一值,输入`PATH=<.dll文件所在目录>`,如图![动态库目录](https://github.com/guoyuanchao94/images/blob/main/BinPath.png?raw=true)
4. C/C++常规->附加包含目录,点击附加包含目录的文本框,编辑选择库目录所在目录其中的include目录
5. 链接器->常规->附加库目录,同上操作,选择静态库文件,即.lib文件所在目录
6. 链接器->输入->附加依赖项,输入所需静态库名称,如aws-c-auth.lib
## 4、代码测试
1. 复制[Amazon S3 examples using SDK for C++](https://docs.aws.amazon.com/zh_cn/sdk-for-cpp/v1/developer-guide/cpp_s3_code_examples.html)Hello Amazon S3示例到工程中编译并运行
2. 如果库文件配置正确并且凭证配置正确,上述示例会打印存储桶名称
## 4、注意事项
1. 如果验证凭证无误,编译程序出现链接错误,比如`LNK2001 无法解析的外部符号 "char const * const Aws::Http::CONTENT_TYPE_HEADER" (?CONTENT_TYPE_HEADER@Http@Aws@@3QBDB)`以及`LNK2001	无法解析的外部符号 "char const * const Aws::Http::API_VERSION_HEADER" (?API_VERSION_HEADER@Http@Aws@@3QBDB)`
2. 出现上述问题,在头文件前添加`#define USE_IMPORT_EXPORT`,或者点击菜单栏项目,选择属性,C/C++->预处理器->预处理器定义,添加`USE_IMPORT_EXPORT`即可
3. 了解此问题,可以参阅[unresolved external symbol CONTENT_TYPE_HEADER and API_VERSION_HEADER](https://github.com/aws/aws-sdk-cpp/issues/1235)以及[(minIO) aws sdk for C++](https://www.jianshu.com/p/74f13cd08cc7)

## 5、示例代码
有关AWS S3更多示例代码,可以参阅官方文档[示例代码](https://docs.aws.amazon.com/zh_cn/sdk-for-cpp/v1/developer-guide/cpp_s3_code_examples.html)

## <center> AWS-SDK-CPP代码

