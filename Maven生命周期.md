Maven生命周期模型
=============

Maven拥有一套十分完善的生命周期模型，它涵盖了所有软件项目的构建的生命周期。
  
生命周期
------------
Maven有三套完全相互独立，互不干扰的生命周期，这三套生命周期分别为：

* Clean Lifecycle    在真正构建之前进行一些清理工作
* Default Lifecycle  构建项目，包括：编译、测试、打包、部署等等
* Site Lifecycle     生成项目报告、发布站点等

这三套生命周期是相互独立、互不干扰的，你可以单独调用 **mvn clean** 、 **mvn install** 、 **mvn site** 来分别进行目录清理，构建项目和发布站点。

阶段
-------------
每套生命周期由一组 **阶段** （Phase）组成。如下所示：

* Clean Lifecycle    
  - pre-clean          执行一些需要在clean之前完成的工作
  - clean              移除所有上一次构建生成的文件
  - post-clean         执行一些需要在clean之后立刻完成的工作
* Default Lifecycle  
  - 地方
  - 地方
  - 地方
* Site Lifecycle  
  - pre-site          执行一些需要在生成站点文档之前完成的工作
  - site              生成项目的站点文档
  - post-site         执行一些需要在生成站点文档之后完成的工作，并且为部署做准备
  - site-deploy       将生成的站点文档部署到特定的服务器上
