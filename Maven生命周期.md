Maven生命周期模型
=============

Maven拥有一套十分完善的生命周期模型，它涵盖了几乎所有软件项目的构建的生命周期。
  
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
  - validate
  - generate-sources
  - process-sources
  - generate-resources
  - process-resources     复制并处理资源文件，至目标目录，准备打包。
  - compile     编译项目的源代码。
  - process-classes
  - generate-test-sources 
  - process-test-sources 
  - generate-test-resources
  - process-test-resources     复制并处理资源文件，至目标测试目录。
  - test-compile     编译测试源代码。
  - process-test-classes
  - test     使用合适的单元测试框架运行测试。这些测试代码不会被打包或部署。
  - prepare-package
  - package     接受编译好的代码，打包成可发布的格式，如 JAR 。
  - pre-integration-test
  - integration-test
  - post-integration-test
  - verify
  - install     将包安装至本地仓库，以让其它项目依赖。
  - deploy     将最终的包复制到远程的仓库，以让其它开发人员与项目共享。

* Site Lifecycle  
  - pre-site          执行一些需要在生成站点文档之前完成的工作
  - site              生成项目的站点文档
  - post-site         执行一些需要在生成站点文档之后完成的工作，并且为部署做准备
  - site-deploy       将生成的站点文档部署到特定的服务器上


最重要的是Default Liftcycle这个生命周期，几乎所有的重要的构建工作都在这个生命完成。通过各个阶段的名字，我们就可以清楚知道每个阶段都完成的工作。

值得注意的是，每个生命周期是相互独立的，而每个生命周期每个阶段都是前后依赖的，也就是说每个阶段的完成都依赖于该生命周期的前面阶段的完成。
例如，执行mvn install，当test阶段发现测试用例不通过，那么test后面的阶段都不会执行。
再如，mvn deplop阶段的完成，需要intall、virfy等该生命周期里前面阶段的完成。
所以，这就是为什么我们在执行mvn install时候看到控制台打印了前面所有阶段的执行信息了。

命令行执行生命周期
-----------------
一定要谨记， **每个生命周期的相互独立、互不干扰的，而每个生命周期内的阶段是前后依赖的** 。

我们可以用过<code>mvn 阶段名</code>来执行执行各生命周期的阶段。例如：

```maven
> mvn install  # 执行Default生命周期的install阶段
> mvn site     # 执行Site生命周期的site阶段
```

我们也可以按顺序执行各个生命周期的各个阶段，例如：

```
# 执行Clean生命周期的clean阶段，然后执行Default生命周期的install阶段，最后执行Site生命周期的site阶段
> mvn clean install site
```

声明周期阶段插件
---------------

Maven只是抽象地定义了软件项目的生命周期以及生命周期的各个阶段。具体的实现交由插件完成，而插件以独立的构建的形式存在。
插件是与生命周期的阶段绑定的，在默认情况下，并不是每个阶段都有与之对应的插件，下面的表格罗列个各个阶段默认对应的插件，以及插件需要完成的任务：

####Clean生命周期:

生命周期阶段 | 目标插件
-------------|--------------------------
pre-clean    | 
clean        | maven-clean-plugin:clean
post-clean   | 


####Default生命周期：

生命周期阶段             | 目标插件                                 | 插件任务
-------------------------|------------------------------------------|---------------------------------
process-resources        | maven-resources-plugin:resources         | 复制资源文件到目标目录
compile                  | maven-compiler-plugin:compile            | 编译代码到目标目录
process-test-resources   | maven-resources-plugin:testResources     | 复制测试资源到目标目录
test-compile             | maven-compiler-plugin:testCompile        | 编译测试代码到目标目录
test                     | maven-surefire-plugin:test               | 执行测试用例
package                  | maven-jar-plugin:jar                     | 创建项目jar包
install                  | maven-install-plugin:install             | 将项目输出构建到本地仓库
deploy                   | maven-deploy-plugin:deploy               | 将项目输出部署到远程仓库

####Site生命周期：

生命周期阶段 | 目标插件
-------------|--------------------------
pre-site     | 
site         | maven-site-plugin:site
post-site    | 
site-deploy  | maven-site-plugin:deploy

参考资料
-------------
《Maven实战》作者: <a href="http://www.juvenxu.com/" target="_blank">http://www.juvenxu.com/</a>
