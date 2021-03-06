# 七牛ufop服务


##简介
七牛云存储服务的一大亮点就是支持数据的处理，目前支持常规的图片，音视频及其他的一些文件处理方式。但是有的时候，在客户需要一些特定功能的数据处理时，就需要用到七牛提供的**用户自定义数据处理(ufop)**功能。

**ufop**的特点是客户自行提供对七牛空间中的文件的处理程序，然后通过七牛提供的ufop工具来进行管理。该程序只需要符合七牛ufop的规范即可。

具体可以参考官方文档：

1. [ufop简介](http://7rfgu2.com1.z0.glb.clouddn.com/ufop_introduction_v2.pdf)
2. [ufop快速入门](http://7rfgu2.com1.z0.glb.clouddn.com/ufop_step_by_step_v2.pdf)

该项目是根据客户需要的常见的数据处理功能，使用Go语言实现的自定义数据处理功能。该项目集成了多个ufop的功能，并且可以编译为统一的ufop处理程序。客户只需要根据七牛的ufop管理工具来创建ufop实例即可快速使用。

##结构

该项目可以直接编译为符合七牛ufop规范的可执行文件，然后配合`qufop.conf`配置文件来运行。该配置文件中除了所有的ufop功能所需要的共同的配置，还包括每一个ufop功能所需要的单独的配置项目。在创建不同的ufop实例的时候，客户只需要提供所有ufop功能所需要的共同配置信息和某ufop功能所需要的指定的配置信息即可。可以参考[示例配置](https://github.com/jemygraw/qiniu-ufop-service/blob/master/src/qufop.conf)

所有ufop所需要的共同的配置信息

|参数名|值|描述|
|----------|-----------|--------|
|listen_port|9100   	| 固定为9100，不可以改动|
|listen_host| 0.0.0.0 	| 固定为`0.0.0.0`，不可以改动|
|read_timeout| <自定义>		| http请求的读取超时时间，单位:秒，默认1800s|
|write_timeout| <自定义>		| http请求的回复超时时间，单位:秒，默认1800s|
|max_header_bytes| <自定义> | http请求的头部大小，单位:字节，默认65535字节|
|ufop_prefix| <自定义>	| ufop服务的前缀，因为该项目集成了很多ufop功能，而根据七牛的ufop规范，每一个ufop实例的名称必须不同，所以通过统一的前缀来避免ufop名称重复|
|access_key| <自定义> 	| 客户账户的AccessKey，可以从七牛后台获得 |
|secret_key| <自定义> 	| 客户账户的SecretKey，可以从七牛后台获得 |

**备注**：每个ufop实例所需要的单独的配置信息在每个ufop功能的文档中介绍。

**ufop功能**和**ufop实例**的联系和区别

1. ufop功能指的是该项目中实现的自定义数据处理功能，比如mkzip，unzip等。
2. ufop实例指的是通过七牛的ufop管理工具注册的自定义数据处理功能，该功能的名称是`前缀+ufop功能名称`，比如前缀是`qn-`，那么对于`mkzip`功能，它对应的实例名称就是`qn-mkzip`，在使用`pfop`接口等数据处理API时，使用的是`ufop实例名称`，即`qn-mkzip`。


##功能
目前该项目实现的ufop功能如下：

|名称|描述|文档|
|-----|--------------------------|---------|
|mkzip|实现了支持utf8和gbk两种编码方式的文件打包功能，可以解决Windows下使用系统自带解压工具解压zip出现的文件中文名称乱码问题。|[详细](https://github.com/jemygraw/qiniu-ufop-service/wiki/mkzip)|
|unzip|实现了文件上传七牛空间，再解压缩功能，可以用于小文件打包上传，提高上传速度。|[详细](https://github.com/jemygraw/qiniu-ufop-service/wiki/unzip)|


##反馈
1. 您可以通过创建issue的方式提交您的问题。
2. 如果您需要帮助，可以联系QQ：2037014430，加前注明来意，非技术问题勿扰。
