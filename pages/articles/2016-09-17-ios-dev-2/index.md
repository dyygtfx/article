---
layout: post
title: 写给前端的 iOS 开发教程(2)：熟悉 CocoaPods
datePublished: '2016-09-16 11:21:04'
category: iOS
---

CocoaPods 我的理解就是和 Node 的 npm 作用差不多，用来管理一些配置依赖文件，不过它是从 Ruby 的 RubyGems,Bundler 受启发来的。如果你要用 OC 开发，肯定要学会使用 pod 的，就像你用 Node，肯定要用 npm 一样的。

网上也有很多 pod 的使用教程，这里也把之前的笔记简单记录一下

## 使用CocoaPods

制作和使用CocoaPods库都十分简单，往往几分钟就能配置完毕。

想获取最新的官方教程，[请前往此处](http://guides.cocoapods.org/)。

#### 安装CocoaPods

CocoaPods可以方便地通过RubyGems安装，打开Terminal，然后键入以下命令：

```
$ sudo gem install cocoapods
```

就这么简单，现在你应该可以开始使用pod命令了。

>如果你使用Ruby版本管理器，如rbenv，你可能需要运行以下指令来重新链接shim的二进制文件（例如：$ rbenv rehash）。

#### 管理相关性

一个相关性管理器可以将一系列的软件需求转化为具体的标签，然后下载并且整合进入相关的项目。

申明需求可以自动化整个项目配置，这也是软件开发的最佳实践之一，无论是在任何语言中。甚至你不使用第三方库，CocoaPods仍然是一个管理代码相关性的绝佳工具。

#### Podfile

Podfile这个文件是用来用来申明项目代码相关性的，正如Bundler的Gemfile，或者npm的package.json

cd进入.xcodeproj文件所在的目录，通过以下命令来创建一个Podfile

```
$ pod init
```

#### Podfile

```
platform :ios, '7.0'

target "AppName" do

end
```

你可以申明需要不同版本的库，大部分情况下，申明到minor或者patch版本就足够了

```
pod 'X', '~> 1.1'
```

#### CocoaPods遵循语意化版本规范。

对于那些不在CocoaPods公共Git仓库中的库，你可以用任何一个Git, Mercurial或者SVN仓库取代，并且还可以指定具体的commit, branch或者tag。

```
pod 'Y', :git => 'https://github.com/NSHipster/Y.git', :commit => 'b4dc0ffee'
```
一旦所有的相关性都申明完毕，你可以使用以下指令来安装所需要的库：

```
$ pod install
```
安装过程中，CocoPods会使用递归来分析所有的需求，并且建立一个代码相关性的图，最后将Podfile序列化为Podfile.lock

比如，如果两个库都需要使用AFNetworking，CocoaPods会确定一个同时能被这两库使用的版本，然后将同一个安装版本链接到两个不同的库中。

CocoaPods会创建一个新的包含之前安装好的静态库Xcode项目，然后将它们链接成一个新的libPods.a target。你原有的项目将会依赖这个新的静态库。一个xcworkspace文件会被创建，从此之后，你应该只打开这个xcworkspace文件来进行开发。

反复使用pod install命令，只会让CocoaPods重复以上步骤，重新安装这些库。所以，当你需要升级它们时，请使用以下命令：

Bash
$ pod update
试着使用CocoaPod

try是一个及其实用但又鲜为人知的CocoaPods命令，通过它你能够在安装一个库之前，先试用一下。

你只需要在try后面加上任意一个CocoaPods公共库的名称，就能试用它了！

Bash
$ pod try Ono
建立自己的CocoaPod

作为Objective-C软件分发实际上的标准，CocoaPods几乎是所有开源项目的标配，如果你想让你的项目被大家很方便地使用。

诚然，这会提高一点点你分享项目的门槛，但是，好处是显然易见的。你花几分钟创建一个.podspec文件可以节省下其他开发者无数的时间。

规范

.podspec文件作为CocoaPods的一个独立单元，包含了名称，版本，许可证，和源码文件等所有信息。

官方指南中有许多信息和范例

以下是NSHipsterKit.podspec

```
Pod::Spec.new do |s|
  s.name     = 'NSHipsterKit'
  s.version  = '1.0.0'
  s.license  = 'MIT'
  s.summary  = "A pretty obscure library.
                You've probably never heard of it."
  s.homepage = 'http://nshipster.com'
  s.authors  = { 'Mattt Thompson' =>
                 'mattt@nshipster.com' }
  s.social_media_url = "https://twitter.com/mattt"
  s.source   = { :git => 'https://github.com/nshipster/NSHipsterKit.git', :tag => '1.0.0' }
  s.source_files = 'NSHipsterKit'
end
```
一旦把这个.podspec发布到公共数据库中，任何想使用它的开发者，只需要在Podfile中加入如下声明即可：

#### Podfile

```
pod 'NSHipsterKit', '~> 1.0'
.podspec文件也可以作为管理内部代码的利器：
```

```
pod 'Z', :path => 'path/to/directory/with/podspec'
```
#### 发布CocoaPod

CocoaPods 0.33中加入了Trunk服务。

虽然一开始使用GitHub Pull Requests来整理所有公共pods效果很好。但是，随着Pod数量的增加，这个工作对于spec维护人员Keith Smiley来说变得十分繁杂。甚至一些没有通过$ pod lint的spec也被提交上来，造成repo无法build。

CocoaPods Trunk服务的引入，解决了很多类似的问题。CocoaPods作为一个集中式的服务，使得分析和统计平台数据变得十分方便。

要想使用Trunk服务，首先你需要注册自己的电脑。这很简单，只要你指明你的邮箱地址（spec文件中的）和名称即可。

```
$ pod trunk register mattt@nshipster.com "Mattt Thompson"
```

至此，你就可以通过以下命令来方便地发布和升级你的Pod！

```
$ pod trunk push NAME.podspec
```