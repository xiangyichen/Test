# 项目名称：article-video-matcher
## 项目语言
Java（JDK1.8）
## 项目管理工具
Maven（4.0.0）
## 主要实现功能
该项目针对文章推荐搜狐知道的视频内容，用于手机搜狐文章页的推广位
## 项目主要结构
* java
  * common
  * controller
  * data
  * service
* resources
## 各模块主要功能及介绍
### SohuLiveConfig配置类
配置数据源，事务管理器以及SqlSessionFactory等。

### resources
application.yml资源配置文件

### common
存放通用工具类，包括CacheNotLoadedException（用于处理Redis缓存没有加载成功的异常）；RedisTool（用于获取Redis分布式锁）。

### controller
* BaseController
基础控制类（其他控制类继承于它）。

* CacheStatusController
判断缓存状态的控制类，对应URL（"/cache/test"）,调用channelService判断缓存是否准备就绪。

* ExternalApiController
对外接口控制类，对应URL（"/external-api"）
  * 方法 recommendVideo，对应URL（"/external-api/recommend/ByArticle"），返回缓存中的推荐视频内容。<br>
  方法逻辑：优先找缓存中是否有推荐内容，若没有再调用recommendService->MpSearchClient寻找推荐内容，如果仍然找不到
  就调用channelService调取默认的推荐结果。
  * 方法 search，对应URL（"/external-api/recommend/ByArticle/search"），通过搜索返回推荐视频内容。

* InternalApiController 
对内接口控制类，对应URL（"/internal-api"）
  * 方法 getDefaultFeedByChannel，对应URL（"/internal-api/getByChannel"），调用channelService得到根据频道来的Feed。
  
### data
* annotation
定义了SohuLiveDataSource的标签。

* apiclient
文章搜索接口。

* entity
实体包，对应实体包括Feed、MpTag、RecommendVO、Video。

* mapper
Mybatis调用数据库的映射，完成了对数据库的一些基本操作。

### service
逻辑层
* BaseService(父类，其他逻辑类继承于它)
* ChannelService（负责跟频道相关的业务逻辑）
* ContentService（负责跟内容相关的业务逻辑）
* CounterService(负责推荐内容与缓存之间的一些逻辑操作)
* RecommendService(负责推荐业务逻辑)
* SearchService（负责调用文章搜索接口的业务逻辑）
* WhiteListService（负责与白名单相关的业务逻辑，不在白名单中的频道是无法获得推荐的）






