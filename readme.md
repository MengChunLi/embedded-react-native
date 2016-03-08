## 需求

- [CocoaPods](http://cocoapods.org/) – `gem install cocoapods`
- [Node.js](http://nodejs.org)
    - 安装 **nvm**（安装向导在[这里](https://github.com/creationix/nvm#installation)）。然后运行`nvm install node && nvm alias default node`，这将会默认安装最新版本的Node.js并且设置好命令行的环境变量，这样你可以输入`node`命令来启动Node.js环境。nvm使你可以可以同时安装多个版本的Node.js，并且在这些版本之间轻松切换。
- 建立package.json： `npm init`
- 在你JS代码文件所在目录下，安装React Native依赖：`npm install react-native --save`

## 通过CocoaPods安装React Native

[CocoaPods](http://cocoapods.org/)是iOS/Mac开发最常用的包管理工具。我们需要用它来引入React Native。如果你还没安装过CocoaPods，参考[这篇文章](http://guides.cocoapods.org/using/getting-started.html)。

当你准备好开始使用CocoaPods之后，往`Podfile`中增加以下的内容。如果你还没有这个文件，在你工程的根目录下创建一个。

```ruby
# Uncomment this line to define a global platform for your project
platform :ios, '7.0'
# Uncomment this line if you're using Swift
use_frameworks!

# Depending on how your project is organized, your node_modules directory may be
# somewhere else; tell CocoaPods where you've installed react-native from npm
pod 'React', :path => '../node_modules/react-native', :subspecs => [
  'Core',
  'RCTImage',
  'RCTNetwork',
  'RCTText',
  'RCTWebSocket',
  # Add any other subspecs you want to use in your project
]

target 'embedded-react-native' do

end
```

记得要添加所有你需要的依赖。举例来说，`<Text>`元素如果不添加`RCTText`依赖就不能运行。
