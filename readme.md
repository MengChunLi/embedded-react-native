## Embedded React Native In IOS APP

架構

```
embedded-react-native
 └ ios
 └ android
 └ node_modules
 └ ReactComponent

```

Download ios native project and place in embedded-react-native/ios/

`git clone https://github.com/MengChunLi/ios-embedded-react-native.git`

## 需求

- [CocoaPods](http://cocoapods.org/) – `gem install cocoapods`
- [Node.js](http://nodejs.org)
    - 安装 **nvm**（安装向导在[这里](https://github.com/creationix/nvm#installation)）。然后运行`nvm install node && nvm alias default node`，这将会默认安装最新版本的Node.js并且设置好命令行的环境变量，这样你可以输入`node`命令来启动Node.js环境。nvm使你可以可以同时安装多个版本的Node.js，并且在这些版本之间轻松切换。
- 建立package.json： `npm init`
- 在你JS代码文件所在目录下，安装React Native依赖：`npm install react-native --save`

## 通过CocoaPods安装React Native

[CocoaPods](http://cocoapods.org/)是iOS/Mac开发最常用的包管理工具。我们需要用它来引入React Native。如果你还没安装过CocoaPods，参考[这篇文章](http://guides.cocoapods.org/using/getting-started.html)。

当你准备好开始使用CocoaPods之后，往`Podfile`中增加以下的内容。如果你还没有这个文件，在你工程的根目录下创建一个。

`Podfile` 需與xcodeproj同一個目錄

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

接着安装你的pods：

```
$ pod install
```

## 启动开发服务器。

_译注_：这一部分的官方文档都有一些过时。翻译组在翻译&审校完其它部分的文档后，如果官方文档还没有更新，会帮助校正官方文档的同时翻译中文文档。

在工程的根目录下，我们可以开启React Native开发服务器：

```
(JS_DIR=`pwd`/ReactComponent; cd node_modules/react-native; npm run start -- --root $JS_DIR)
```

这条命令会启动一个React Native开发服务器，用于构建我们的bundle文件。`--root`选项用来标明你的React Native应用所在的根目录。在我们这里是`ReactComponents`目录，里面有一个`index.ios.js`文件。开发服务器启动后会打包出`index.ios.bundle`文件来，并可以通过`http://localhost:8081/index.ios.bundle`来访问。

## 關於XCode設定

* 找不到.h檔

'RCTRootView.h' file not found

如果是使用Pods確定 Targets -> Build Phase -> Link binary with Libraries 裡面是否有新增 `Pods_embedded_react_native.framework` 

* 出現IOS9安全性連線問題

In iOS 9.0+ Apple by default blocks non-secure (http) requests. I had to add this to my Info.plist

```
<key>NSAppTransportSecurity</key>
    <dict>
        <key>NSExceptionDomains</key>
        <dict>
            <key>localhost</key>
            <dict>
                <key>NSTemporaryExceptionAllowsInsecureHTTPLoads</key>
                <true/>
            </dict>
        </dict>
    </dict>
```

參考資料: https://github.com/facebook/react-native/issues/304
