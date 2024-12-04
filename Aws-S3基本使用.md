# <center>Aws S3服务的基本使用

## 1、安装软件并配置凭证

### 安装AWS CLI
1. 想要了解什么是 AWS CLI,请参阅[什么是AWS CLI](https://docs.aws.amazon.com/zh_cn/cli/latest/userguide/cli-chap-welcome.html)
2. 直接安装 AWS CLI,Windows 平台,[AWS CLI下载链接](https://awscli.amazonaws.com/AWSCLIV2.msi)
3. 具体安装过程请参阅[AWS CLI安装步骤](https://docs.aws.amazon.com/zh_cn/cli/latest/userguide/getting-started-install.html)

### 配置凭证
1. 打开 Windows PowerShell,输入`aws configure`,开始输入凭证信息
2. 按照提示依次输入`AWS Access Key ID`、`AWS Secret Access Key`、`Default region name`对应的值,`Default output format`直接回车即可完成配置过程
3. 详细配置过程请参阅,[为配置设置AWS CLI](https://docs.aws.amazon.com/zh_cn/cli/latest/userguide/cli-chap-configure.html)
   
### 配置结果
1. 凭证配置完成后,可以在`~/.aws`目录下可以找到`config`和`credentials`文件
2. 使用文本编辑器打开`config`文件可以看到配置的`region`信息
3. 使用文本编辑器打开`credentials`文件可以看到配置的`aws_access_key_id`和`aws_secret_access_key`信息

### 检验凭证
1. 使用文本编辑器打开`~/.aws`目录下的`config`文件和`credentials`文件可以看到凭证信息,通过比较凭证信息可以检验配置的凭证是否有误
2. 打开 Windows PowerShell,输入`aws configure list`命令,可以看到我们进行的一些配置,如![配置图片](https://github.com/guoyuanchao94/images/blob/main/config.png?raw=true)
3. 打开 Windows PowerShell,输入`aws s3 ls`以检验凭证是否有误,如果凭证无误,会显示我们的存储桶信息,如![存储桶列表](https://github.com/guoyuanchao94/images/blob/main/bucket.png?raw=true)                           
否则会显示错误信息,提醒我们的配置不正确或账户存在问题,如![错误信息](https://github.com/guoyuanchao94/images/blob/main/error.png?raw=true)

### 凭证问题
1. 打开 Windows PowerShell,输入上述命令如果出现`An error occurred (InvalidAccessKeyId) when calling the ListBuckets operation: The AWS Access Key Id you provided does not exist in our records.`等问题,请参阅[为什么我会收到错误信息 "The AWS Access Key Id you provided does not exist in our records?"](https://aws.amazon.com/cn/getting-started/faq/access-key-id-doesnt-exist-in-records/),观看视频寻找解决方法

## 2、下载并编译源码

### 下载源码
1. 新建文件夹用于保存下载的源码,在保存源码文件夹下打开 Git Bash,输入或粘贴以下命令 `git clone --recurse-submodules https://github.com/aws/aws-sdk-cpp`,之后回车,等待仓库克隆完成即可
   
### 编译源码
1. 新建文件夹用于编译源码,在源码编译目录中打开 Windows PowerShell 或打开终端切换到当前源码编译目录,运行以下命令,`cmake <path-to-root-of-this-source-code> 
-DCMAKE_BUILD_TYPE=Debug -DCMAKE_INSTALL_PREFIX=<path-to-install> -DBUILD_ONLY="s3"`配置CMake项目,这里`<path-to-root-of-this-source-code>`是源码下载目录的路径,`<path-to-install>`是库文件安装目录的路径。这两个路径需要根据您的文件系统结构进行替换
1. 等待配置完成,在当前目录下,执行命令`cmake --build . --config=Debug`并回车来编译源码
2. 等待源码编译完成,以管理员身份运行命令提示符,切换到源码编译目录,执行以下命令`cmake --install . --config=Debug`来安装库文件和头文件,等待命令执行完成,库文件以及头文件会被安装到`path-to-install`对应的目录中
3. 请注意,上述步骤仅构建与 Amazon S3 相关的部分。如果需要构建整个 SDK,请在配置命令中删除`-DBUILD_ONLY="s3"`选项
4. 请注意,上述步骤默认生成的库是处于`Debug`模式,如果您希望生成`Release`模式的库,只需简单地将第 1 步和第 2 步中的命令中的`Debug`参数更改为`Release`即可
5. 有关编译源码详细问题请参阅[aws/aws-sdk-cpp](https://github.com/aws/aws-sdk-cpp),有关 CMake 参数列表,请参阅[CMake参数](https://github.com/aws/aws-sdk-cpp/blob/main/docs/CMake_Parameters.md)

## 3、直接安装库文件

### 安装库文件
1. 由于网络原因,从源码下载 AWS SDK for C++ 可能耗时较长且存在失败的风险,因此推荐考使用 vcpkg 来快速下载和安装 AWS SDK for C++
2. 新建文件夹用于保存vcpkg,在该文件夹内打开 Git Bash,执行`git clone https://github.com/Microsoft/vcpkg.git`命令以克隆 vcpkg 仓库
3. 等待仓库克隆完成,Windows 平台下依次执行以下命令`cd vcpkg`,`./bootstrap-vcpkg.bat`,`./vcpkg integrate install`和`./vcpkg install aws-sdk-cpp`来安装并配置 vcpkg ,等待命令执行完成,安装后的库文件将位于 vcpkg 目录下的 installed 文件夹中

### 注意事项
1. aws-sdk-cpp 在 vcpkg 中的端口由微软团队成员和社区贡献者保持更新,如果版本过时,请在 vcpkg 仓库中创建问题或拉取请求
2. 执行上述命令安装的库文件包括 Debug 模式和 Release 模式
3. 上述命令默认安装 x64 架构库文件,如需安装 x86 架构,请输入以下命令`./vcpkg install aws-sdk-cpp:x86-windows`

## 4、静态库和动态库的使用
1. Windows 平台,打开 Visual Studio,创建空项目,点击菜单栏`项目`,选择属性,点击进入属性页
2. 配置项目属性,在属性页中,选择配置为 Debug 或 Release,针对您的平台选择 x86 或 x64 
3. 设置环境变量,配置属性->调试->环境,输入`PATH=<dll-path>`,其中`<dll-path>`是动态库文件所在目录,如图![动态库目录](https://github.com/guoyuanchao94/images/blob/main/BinPath.png?raw=true)
4. 配置包含目录,C/C++常规->附加包含目录,点击附加包含目录的文本框,编辑选择库目录所在目录其中的include目录,如图![附加包含目录](https://github.com/guoyuanchao94/images/blob/main/includePath.png?raw=true)
5. 配置库目录,链接器->常规->附加库目录,同上操作,选择静态库文件所在目录,即.lib文件所在目录,如图![附加库目录](https://github.com/guoyuanchao94/images/blob/main/libPath.png?raw=true)
6. 添加依赖项,链接器->输入->附加依赖项,输入所需静态库名称,如aws-c-auth.lib

## 5、代码测试
1. 复制[Amazon S3 examples using SDK for C++](https://docs.aws.amazon.com/zh_cn/sdk-for-cpp/v1/developer-guide/cpp_s3_code_examples.html) Hello Amazon S3 示例到工程中编译并运行
2. 如果库文件配置正确并且凭证配置正确,上述示例会打印存储桶相关信息

## 6、注意事项
1. 如果验证凭证无误,编译程序出现链接错误,比如`LNK2001 无法解析的外部符号 "char const * const Aws::Http::CONTENT_TYPE_HEADER" (?CONTENT_TYPE_HEADER@Http@Aws@@3QBDB)`以及`LNK2001	无法解析的外部符号 "char const * const Aws::Http::API_VERSION_HEADER" (?API_VERSION_HEADER@Http@Aws@@3QBDB)`
2. 如果出现上述问题,请在头文件前添加`#define USE_IMPORT_EXPORT`,或者点击菜单栏项目,选择属性,C/C++->预处理器->预处理器定义,添加`USE_IMPORT_EXPORT`即可
3. 了解此问题,可以参阅[unresolved external symbol CONTENT_TYPE_HEADER and API_VERSION_HEADER](https://github.com/aws/aws-sdk-cpp/issues/1235)以及[(minIO) aws sdk for C++](https://www.jianshu.com/p/74f13cd08cc7)

## 7、示例代码
1. 有关AWS S3更多示例代码,可以参阅官方文档[示例代码](https://docs.aws.amazon.com/zh_cn/sdk-for-cpp/v1/developer-guide/cpp_s3_code_examples.html)

## <center> AWS-SDK-CPP代码

### 1. 项目依赖库文件
1. `aws-c-auth.lib`,处理 AWS 的认证机制.它提供 AWS API 调用所需的身份验证、授权和会话管理功能,包括凭证管理.
2. `aws-c-cal.lib`,负责日期和时间的处理,特别是与 AWS 服务的签名相关的时间戳.
3. `aws-c-common.lib`,AWS SDK 的基础库,包含了许多常用的通用功能,比如内存管理、日志、错误处理、字符串处理等.
4. `aws-c-compression.lib`,提供压缩和解压缩支持,用于在 AWS 请求和响应中处理压缩数据(例如 S3 存储中的压缩文件)
5. `aws-c-event-stream.lib`,提供事件流的处理,用于 AWS 服务的请求和响应处理,尤其是支持事件驱动的通信模型
6. `aws-c-http.lib`,用于处理 HTTP 请求和响应的库,底层支持 AWS SDK 与 AWS 服务之间的通信
7. `aws-c-io.lib`,提供底层的 I/O 操作支持,尤其是与 AWS 服务的通信中,网络连接、文件读取等
8. `aws-c-mqtt.lib`,实现 MQTT 协议的库,用于与 AWS IoT 服务进行交互
9. `aws-c-s3.lib`,专门用于与 Amazon S3(简单存储服务)交互的库,提供文件上传、下载、删除等操作
10. `aws-c-sdkutils.lib`,AWS SDK 的工具库,包含一些有用的工具和实用程序,比如配置文件解析、日志、错误处理等 
11. `aws-checksums.lib`,提供校验和计算功能,用于验证数据的完整性(例如计算文件的 MD5、SHA 等)
12. `aws-cpp-sdk-core.lib`,AWS SDK 的核心功能库,提供了连接 AWS 服务、请求处理、身份验证等基础功能
13. `aws-cpp-sdk-dynamodb.lib`,用于与 Amazon DynamoDB 服务进行交互的库,DynamoDB 是一个完全托管的 NoSQL 数据库 这个是否留下有待验证
14. `aws-cpp-sdk-kinesis.lib`,与 Amazon Kinesis(流数据服务)交互的库.Kinesis 用于收集、处理和分析实时数据流.
15. `aws-cpp-sdk-s3.lib`,与 Amazon S3 进行交互的库(类似于 aws-c-s3.lib,但它是 C++ 版本)
16. `aws-crt-cpp.lib`,AWS CRT(C Runtime) C++ 库,底层的依赖库,提供更高效的操作和与 AWS 服务的更高效通信 
17. `zlib.lib`,zlib 是一个广泛使用的压缩库,AWS SDK 中会用到它来处理数据的压缩和解压缩
### 1、创建存储桶

### 2、列出存储桶

### 3、删除存储桶

### 4、上传文件到存储桶

### 5、下载存储桶中文件到本地

### 6、删除存储桶中的文件

### 7、批量上传、批量下载、断点上传、断点下载


