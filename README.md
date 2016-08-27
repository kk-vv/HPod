---
layout: post
title: 构建自己的Cocoapods私有库 
categories: [Cocoapods]
comments: true
---
  
[版权所有,转载请注明出处!](https://ifallen.github.io)


### `创建Cocoapods 私有库`

---

#### `1、创建组件项目`

```
pod lib create HAutoScrollView
```

- `如图(该命令会Pull pod-template项目，完了之后填写下面四个问题)`


>
![image](https://github.com/iFallen/ifallen.github.io/raw/master/img/2016/08/podspec/init.jpeg)

```
1. 支持的语言
2. 是否包含Demo应用
3. 测试框架
4. UI测试
5. class 前缀
```
>
根据自己需要选择，具体可[查看官方描述](https://guides.cocoapods.org/making/using-pod-lib-create.html)

- `信息填完之后，会自动打开工程如下：` 


>
![image](https://github.com/iFallen/ifallen.github.io/raw/master/img/2016/08/podspec/project.jpeg)


```
1. HAutoScrollView.podspec 是我们要编辑的并最终提交到Cocoapods的配置文件
2. Example 是我们写Demo的地方
3. Pod/HAutoScrollView/Classes 下面自己需要实现的类(自行添加或替换！)
```

#### `2、提交自己的项目到Git`

>
在编辑`HAutoScrollView.podspec`之前，我们需要想把自己的项目提交到Git管理服务上，可以做[GitHub](https://github.com)上创建一个同名的未初始化的仓库。

- `提交代码：（终端：cd 到刚刚创建的工程目录下）`

```
$ git add . 
$ git commit -am 'Initial Commit'
$ git remote add origin https://github.com/iFallen/HAutoScrollView.git(自己的远程仓库地址)
```

- `因为podspec文件中获取Git版本控制的项目还需要tag号，所以我们要打上一个tag`

```
$ git tag -m 'First Release' '0.1.0'
$ git push --tags
```


#### `3、编辑podspec`

>
在完成前面两步之后就可以开始编辑`podspec`文件了
>
`注：`确保在 Pods/HAutoScrolLView/Classes已经有了自己的项目文件！

- `原有的podspec文件,简单英文 大家要看的懂`

```
#
# Be sure to run `pod lib lint HAutoScrollView.podspec' to ensure this is a
# valid spec before submitting.
#
# Any lines starting with a # are optional, but their use is encouraged
# To learn more about a Podspec see http://guides.cocoapods.org/syntax/podspec.html
#

Pod::Spec.new do |s|
  s.name             = 'HAutoScrollView'//自己库文件名称
  s.version          = '0.1.0' //版本号
  s.summary          = 'A short description of HAutoScrollView.' //简短描述

  s.description      = <<-DESC
TODO: Add long description of the pod here. //详细描写，别把DESC删了！！！
                       DESC

  s.homepage         = 'https://github.com/<GITHUB_USERNAME>/HAutoScrollView'//Git 主页地址
  # s.screenshots     = 'www.example.com/screenshots_1', 'www.example.com/screenshots_2' //截图预览地址 没有就删掉
  s.license          = { :type => 'MIT', :file => 'LICENSE' } //开源协议
  s.author           = { 'LiangJun.Hu' => 'hulj1204@yahoo.com' } //作者信息
  s.source           = { :git => 'https://github.com/<GITHUB_USERNAME>/HAutoScrollView.git', :tag => s.version.to_s } // 版本库地址
  # s.social_media_url = 'https://twitter.com/<TWITTER_USERNAME>' //社交地址，没有可删掉

  s.ios.deployment_target = '8.0' //支持的版本号

  s.source_files = 'HAutoScrollView/Classes/**/*' //自己项目的源文件
  
  //png资源 没有就删掉，有就删掉 ‘#’ 号
  # s.resource_bundles = {
  #   'HAutoScrollView' => ['HAutoScrollView/Assets/*.png']
  # }
  
  # s.public_header_files = 'Pod/Classes/**/*.h' //暴露的头文件
  # s.frameworks = 'UIKit', 'MapKit' //自己需要依赖的系统库文件 逗号分隔
  # s.dependency 'AFNetworking', '~> 2.3' //自己需要依赖的三方库文件 逗号分隔
end

```

- `修改后的的podspec文件`

```
Pod::Spec.new do |s|
  s.name             = 'HAutoScrollView'
  s.version          = '0.1.0'
  s.summary          = 'HAutoScrollView'
  s.description      = <<-DESC
  循环ScrollView,支持自动滚动、支持点击事件代理回调，已处理NSTimer 销毁，处理AutoLayout 适配。
                       DESC

  s.homepage         = 'https://github.com/iFallen/HAutoScrollView'
  s.license          = { :type => 'MIT', :file => 'LICENSE' }
  s.author           = { 'LiangJun.Hu' => 'hulj1204@yahoo.com' }
  s.source           = { :git => 'https://github.com/iFallen/HAutoScrollView.git', :tag => s.version.to_s }
  s.social_media_url = 'https://ifallen.github.io'

  s.ios.deployment_target = '7.0'
  s.requires_arc = true

  s.source_files = 'HAutoScrollView/Classes/**/*'
  s.public_header_files = 'HAutoScrollView/Classes/**/*.h'
end


```



#### `4、验证podspec文件`
>
编辑完podspec文件后我们需要验证他有没有错(终端下操作)
>
podspec如果有没有用到的配置，请全部删掉！

```
$ pod lib lint HAutoScrollView.podspec
```

- `正常输出如下:`

>
![image](https://github.com/iFallen/ifallen.github.io/raw/master/img/2016/08/podspec/lint.jpeg)

>
1、警告可以有，但是不能有错。
>
2、如果提示你 某个模拟器不支持，可能是你终端选择的Xcode有问题，用`sudo xcode-select -s /Applications/Xcode.app/` 修改


#### `5、提交podspec添加到本地`

>
`向Spec Repo提交podspec需要完成两点一个是podspec必须通过验证无误，另外删掉无用的注释`


- `本地初始化组件仓库`

```
$ pod repo add HAutoScrollView https://github.com/iFallen/HAutoScrollView.git
```

- `向仓库中添加组件(添加在自己的 ~/.cocoapods/ 目录下面,可以自己打开查看)`


```
$ pod repo push HAutoScrollView ~/Desktop/HAutoScrollView.podspec
```

#### `6、测试自己私有的spec`

>
上面几步都没问题，那么自己的私有库就构建好了，可以找个项目测试一下。

- `测试私有库`

>
![image](https://github.com/iFallen/ifallen.github.io/raw/master/img/2016/08/podspec/test.jpeg)


>
以上步骤，完成了自己的私有库构建。如果你想共享给别人用，即其他人能通过`pod search`能够检索到你的库文件，需要通过`pod trunk push` 提交。

---

#### `7、Trunk 方式提交开源组件`

>
push 到 cocoapods 之前，最好删掉我们刚刚添加到本地的spec!

```
$ pod repo remove  HAutoScrollView
```

#### `Trunk Push`
- `1.注册pod账号：`

- `pod trunk register [E-mail] [User Name]`

```
$ pod trunk register hulj1204@yahoo.com "JuanFelix"
```

上述邮箱会收到一封验证邮件，按照邮件说明打开制定的链接，注册流程就完成了。

- `2.注册流程完成后，可以使用命令查看自己信息：`

>
$ pod trunk me

>
![image](https://github.com/iFallen/ifallen.github.io/raw/master/img/2016/08/podspec/trunkme1.jpeg)

- `3.提交自己的spec`

```
$ pod trunk push HAutoScrollView.podspec
```

- `4.检索自己的spec(不要在自己的spec目录下检索！)`

```
$ pod search HAutoScrollView
```

- `如图:`

>
![image](https://github.com/iFallen/ifallen.github.io/raw/master/img/2016/08/podspec/search.jpeg)

>
这个时候，你的开源组件就可以供其他人通过Cocoapods下载使用了。
鉴于墙的问题，最好挂个VPN操作.

- `5.删除spec`

>
如果你想删除 执行delete 命令就OK

```
$ pod trunk delete HPod 0.1.0
```

>
![image](https://github.com/iFallen/ifallen.github.io/raw/master/img/2016/08/podspec/delete.jpeg)


---

#### `8、更新spec`

- `有新的版本提交时`

```
$ git tag '1.0.0 Release' '1.0.0'  //记得修改.podspec 里面的版本号
$ git push --tags
$ pod trunk push HAutoScrollView.podspec
```

- `成功后再次检索就有新版本了`

>
![image](https://github.com/iFallen/ifallen.github.io/raw/master/img/2016/08/podspec/search2.jpeg)

#### `9、添加开发人员`

- `1.查看spec信息`

```
$ pod trunk info HAutoScrollView
```

>
![image](https://github.com/iFallen/ifallen.github.io/raw/master/img/2016/08/podspec/trunkinfo.jpeg)

- `2.添加/移除owner`

```
$ pod trunk add-owner   HAutoScrollView email@email.com
$ pod trunk remove-owner  email@email.com
```

---

	
