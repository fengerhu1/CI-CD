## CI/CD 环境搭建 ##

### CI 是什么 ###
CI (Continous Integration)，持续集成。  
Martin FLower 这样定义持续集成：持续集成是一种软件开发实践，即团队开发成员经常集成他们的工作，通常每个成员每天至少集成一次，这就意味着每天可能会发生多次集成。  
在现在的软件开发项目中，几乎没有项目是只有一个人在开发的。超过一个人就形成了团队，每个人同时并行开发不同模块的功能，这就涉及到代码的集成，所以代码集成是几乎所有开发团队都要面临的问题。  
每次集成都通过自动化的构建（编译，自动化测试，部署)来验证正确性，从而尽快地发现集成错误。大多数团队发现这个过程可以大大减少集成的问题，让团队能够更快的开发内聚的软件。

![](https://upload-images.jianshu.io/upload_images/1445879-da8a8efae7d2c7df.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

从图中可以看出，同时会存在多个开发者（两个以上），他们会向同一个版本控制代码库中提交代码，存在一个CI Master去监测代码库是否存在更新，一旦更新，就会在Build Server中触发构建，一次构建通常包含一下几个步骤：

- Check out
- Run build
- Compile
- Test (Unit, Integration, E2E)
- Deploy

构建运行完毕，Build Server会输出构建结果，CI Master会根据结果失败与否设置状态（失败：红，成功：绿），最后通知开发人员、QA以及Team Leader。

### 为什么要构建 CI ###
CI能够给我们带来的益处有：

- 减少重复的过程
- 降低风险
- 可视化
- 增强团队的信心
- 随时随地可以生成可部署的软件（CD）

### 搭建 CI 环境 ###

1. 准备工作
	- 拥有 GitHub 帐号
	- 该帐号下面有一个项目
	- 该项目里面有可运行的代码
	- 该项目还包含构建或测试脚本

	使用 Github 账户登入 Travis CI，Travis 会列出 Github 上面你的所有仓库，以及你所属于的组织。此时，选择你需要 Travis 帮你构建的仓库，打开仓库旁边的开关。一旦激活了一个仓库，Travis 会监听这个仓库的所有变化。

	![](https://raw.githubusercontent.com/fengerhu1/CI-CD/master/pic/1.jpg)

2. .travis.yml

	Travis 要求项目的根目录下面，必须有一个.travis.yml文件。这是配置文件，指定了 Travis 的行为。该文件必须保存在 Github 仓库里面，一旦代码仓库有新的 Commit，Travis 就会去找这个文件，执行里面的命令。

	![](https://raw.githubusercontent.com/fengerhu1/CI-CD/master/pic/5.png)

	在项目根目录中新建 .travis.yml 配置文件，内容如下:

	![](https://raw.githubusercontent.com/fengerhu1/CI-CD/master/pic/travis.png)


	script中的脚本是你希望travis-ci帮你跑的测试脚本，可以根据需要自行更改。

	![](https://raw.githubusercontent.com/fengerhu1/CI-CD/master/pic/2.png)

3. 运行

	
	![](https://raw.githubusercontent.com/fengerhu1/CI-CD/master/pic/3.png)

	commit之后，在travis-ci中可以看到ci结果。若测试不通过，在job log中也可以看到完整的错误信息。

	![](https://raw.githubusercontent.com/fengerhu1/CI-CD/master/pic/4.png)

4. 简单部署

	如果要部署至github page上，首先前端package.json中需要修改

	添加github page url

	添加scripts

	![](https://raw.githubusercontent.com/fengerhu1/CI-CD/master/pic/6.jpg)


	另外为了防止他人向你的repo提交恶意pull request 所以需要添加github_token认证

	进入github，右上角头像 > settings > developer settings > personal access tokens > generate new token，勾选repo即可。

	然后进入travis-ci repo，settings > environmental variables 中刚刚将生成的github_token添加。

	![](https://raw.githubusercontent.com/fengerhu1/CI-CD/master/pic/github_token.png)

	Name: github_token  ，Value: 刚刚生成的一长串token

	注意不要勾选display ! github_ token是不希望别人看到的，一旦勾选上，别人能在你的build log中看到你的github_token。

	之后每次commit，travis-ci会将项目测试，测试通过后，打包放在github repo的gh-pages分支。

	最后进入github repo， settings > options > github pages 将 source 设置为 gh-pages branch即可。	



### CD 是什么 ###

持续交付在持续集成的基础上，将集成后的代码部署到更贴近真实运行环境的「类生产环境」(production-like environments)中。持续交付优先于整个产品生命周期的软件部署，建立在高水平自动化持续集成之上。 

### 为什么要构建 CD ###

快速发布。能够应对业务需求，并更快地实现软件价值。编码->测试->上线->交付的频繁迭代周期缩短，同时获得迅速反馈;高质量的软件发布标准。整个交付过程标准化、可重复、可靠，整个交付过程进度可视化，方便团队人员了解项目成熟度;更先进的团队协作方式。从需求分析、产品的用户体验到交互 设计、开发、测试、运维等角色密切协作，相比于传统的瀑布式软件团队，更少浪费。

### 搭建 CD 环境 ###

1. 准备工作

	官网注册heroku账号并下载 heroku-cli

2. 命令行中进入项目目录

	`$ heroku login`    
	`Enter your Heroku credentials.`  
	`Email: user@example.com`  
	`Password:  `  
3. 创建heroku repo

	`$ heroku create `

4. 上传至heroku repo

	`$ git push heroku master`

5. 项目部署完毕，可通过下面这条命令打开部署的网站  

	`$ heroku open`
	
	展示地址：<http://murmuring-crag-84334.herokuapp.com/>

### 参考资料 ###

<https://blog.csdn.net/eugenelee2096/article/details/73332615>  
<https://www.jianshu.com/p/75401d84e2da>  
[travis-ci官方文档](https://docs.travis-ci.com/)  
[heroku官方文档](https://devcenter.heroku.com/articles/getting-started-with-nodejs#introduction)


	

