# Taro

## 使用方法

## taro convert 

* taro 版本 v1.3.9-beta.2

### 工人小程序转taro踩坑

* 转换流程： 
* > 1 app.* (js & json & wxss) 转换 
* > 2 app.js生成 
* > 3 app.js依赖项拷贝 (api、env、ulogConfig) 
* > 4 app.json配置页面组件文件转换 
* > 4.1 index/index.* (js & json & wxml & wxss) 转换
* > 4.1.1 index/index.js 生成 
* > 4.1.2 index.json 解析 组件依赖项加载
* > 4.1.2.1 按钮组件 components/button/index.* (js & json & wxml & wxss) 转换
* > 4.1.2.2 按钮组件 components/button/index.js 生成
* > 4.2 login/index.* (js & json & wxml & wxss) 转换
* > 4.2.1 login/index.js 生成 
* > 4.2.2 index.json 解析 组件依赖项加载
* > 4.2.2.1 登录组件 components/login/index.* (js & json & wxml & wxss) 转换
* > 4.2.2.2 登录组件 components/login/index.js 生成
* > 4.2.2.3 components/login/index.js依赖项拷贝 (lf-weapp/index.js)
* > 5 wxss文件按解析顺序处理 scss生成
* > 5.1 app.scss 生成
* > 5.2 index/index.scss 生成
* > 5.3 components/button/index.scss 生成
* > 5.4 login/index.scss 生成
* > 5.5 components/login/index.scss 生成

* 问题：

* 4.1 index.js依赖项，'模块名'的加载方式没做处理
* 解决方法： @tarojs/cli/src/convert/helper.ts 添加对'模块名'的加载处理
```
//  第三步：判断是否存在模块文件 (miniprogram_npm查找)
  let pathArr = sourceFilePath.split('/')
  let dirPath = ''
  pathArr.forEach((ele, ind) => {
    dirPath += ele + '/'
    let vpath = resolveScriptPath(path.resolve(dirPath, 'miniprogram_npm', value))
    if (vpath) {
      if (fs.existsSync(vpath)) {
        if (fs.lstatSync(vpath).isDirectory()) {
          if (fs.existsSync(path.join(vpath, 'index.js'))) {
            vpath = path.join(vpath, 'index.js')
          } else {
            printLog(processTypeEnum.ERROR, '引用目录', `文件 ${sourceFilePath} 中引用了目录 ${value}！`)
            return
          }
        }
        scriptFiles.add(vpath)
      }
    }
  })
```

* 4.1问题修改后的转换过程

* > 开始代码转换...
* > 转换  入口文件  app.js
* > 转换  入口配置  app.json
* > 转换  入口样式  app.wxss
* > 生成  入口文件  taroConvert/src/app.js
* > 拷贝  JS 文件   taroConvert/src/miniprogram_npm/@ke/lf-weapp/index.js
* > 拷贝  JS 文件   taroConvert/src/config/api.js
* > 拷贝  JS 文件   taroConvert/src/env.js
* > 拷贝  JS 文件   taroConvert/src/miniprogram_npm/@lianjia/fizz/index.js
* > 拷贝  JS 文件   taroConvert/src/miniprogram_npm/@lianjia/fizz/ulog.js
* > 拷贝  JS 文件   taroConvert/src/miniprogram_npm/@lianjia/fizz/api/request.js
* > 拷贝  JS 文件   taroConvert/src/miniprogram_npm/@lianjia/fizz/utils/index.js
* > 拷贝  JS 文件   taroConvert/src/miniprogram_npm/@lianjia/fizz/ulogConfig.js
* > 拷贝  JS 文件   taroConvert/src/ulogConfig.js
* > 转换  页面文件  pages/index/index.js
* > 转换  页面配置  pages/index/index.json
* > 转换  页面模板  pages/index/index.wxml
* > 转换  页面样式  pages/index/index.wxss
* > 错误  图片不存在  index/user.png
* > 生成  页面文件  taroConvert/src/pages/index/index.js
* > 转换  组件文件  miniprogram_npm/@ke/lf-weapp/components/button/index.js
* > 转换  组件配置  miniprogram_npm/@ke/lf-weapp/components/button/index.json
* > 转换  组件模板  miniprogram_npm/@ke/lf-weapp/components/button/index.wxml
* > 转换  组件样式  miniprogram_npm/@ke/lf-weapp/components/button/index.wxss
* > 生成  组件文件  taroConvert/src/miniprogram_npm/@ke/lf-weapp/components/button/index.js
* > 转换  页面文件  pages/login/index.js
* > 转换  页面配置  pages/login/index.json
* > 转换  页面模板  pages/login/index.wxml
* > 转换  页面样式  pages/login/index.wxss
* > 错误  图片不存在  login/wechat.png
* > 生成  页面文件  taroConvert/src/pages/login/index.js
* > 转换  组件文件  miniprogram_npm/@ke/lf-weapp/components/login/index.js
* > 转换  组件配置  miniprogram_npm/@ke/lf-weapp/components/login/index.json
* > 转换  组件模板  miniprogram_npm/@ke/lf-weapp/components/login/index.wxml
* > 转换  组件样式  miniprogram_npm/@ke/lf-weapp/components/login/index.wxss
* > 生成  组件文件  taroConvert/src/miniprogram_npm/@ke/lf-weapp/components/login/index.js
* > 拷贝  JS 文件   taroConvert/src/miniprogram_npm/@ke/lf-weapp/components/login/index.js
* > 生成  样式文件  taroConvert/src/app.scss
* > 生成  样式文件  taroConvert/src/pages/index/index.scss
* > 生成  样式文件  taroConvert/src/miniprogram_npm/@ke/lf-weapp/components/button/index.scss
* > 生成  样式文件  taroConvert/src/pages/login/index.scss
* > 生成  样式文件  taroConvert/src/miniprogram_npm/@ke/lf-weapp/components/login/index.scss
* > 生成  文件      taroConvert/config/index.js
* > 生成  文件      taroConvert/config/dev.js
* > 生成  文件      taroConvert/config/prod.js
* > 生成  文件      taroConvert/package.json
* > 生成  文件      taroConvert/project.config.json
* > 生成  文件      taroConvert/.gitignore
* > 生成  文件      taroConvert/.editorconfig
* > 生成  文件      taroConvert/.eslintrc
* > 生成  文件      taroConvert/src/index.html

* > 生成  组件文件  taroConvert/src/miniprogram_npm/@ke/lf-weapp/components/login/index.js
* > 拷贝  JS 文件   taroConvert/src/miniprogram_npm/@ke/lf-weapp/components/login/index.js

* 问题：

* 拷贝  JS 文件   taroConvert/src/miniprogram_npm/@ke/lf-weapp/index.js
* es6 模块化加载的组件没有处理 (export * from './lib/utils/request')
* 解决方法： @tarojs/cli/src/convert/index.ts 添加babel-type的'ExportAllDeclaration'的加载处理
```
ExportAllDeclaration (astPath) {
  const node = astPath.node
  const source = node.source
  const value = source.value
  analyzeImportUrl(sourceFilePath, scriptFiles, source, value)
}
```

### 2019-07-18

* 由于taro convert的坑太多，修改了两个加载的问题后依然还有大量问题
* 如：ulog封装的页面配置没有按小程序页面处理
* 如：当前页面的getApp获取的全局对象和原生的不一样
* 如：项目package.json的依赖包在运行时加载依赖，且出错
* 暂时放弃taro convert迁移方案，转手动代码迁移
* 手动迁移的好处：不需要解决taro convert的坑，只需要解决taro build的坑

### 2019-07-24

* 回归taro convert，继续走原生转taro的坑=。=

### 工人小程序src部分转taro踩坑

* 问题1、支持styl的样式转换 - -
* 问题2、ulog包的一层配置无法解析
* 2.1 taro迁移的埋点问题：

解决方案：1、修改埋点工具

2、解决taro convert编译过程对App、Page的识别问题

## taro weapp

## taro h5