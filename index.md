# Sofa 2.0

> 对于后台系统，各个页面的相似度高，怎样快速高质量的进行开发？
> 优质的页面、组件等怎样的积累方式是有效的、可持续的？
> 对于每个系统所共有的部分，怎样跨越框架的屏障，实现一次开发，处处使用？

## Sofa 是什么？

1. 一套快速低成本脚手架生成方案;

2. 一个优质代码片段积累仓库;

3. 多项目代码结构、代码质量管控方案;

4. 让所有的优质代码都有稳定的沉淀路径，让所有的代码都有迹可循;

## 1.开始

### 环境要求

Node Version 10.10 +，由于使用了fs一些新方法，之后会考虑替换方法降低版本要求；
Git 已安装且已登录；

### 安装

```bash
# 全局安装
sudo npm install --unsafe-perm -g sofa-scripts
```

在安装过程中会要求输入数据库连接秘钥，您可以直接使用我们提供的数据库秘钥【43D7E3D3C12AA550BDC2F7448C22F1EBFE92AF3188978D3F076B27F412A9E230C4830F93D28223C7B6E16B34FCBA7846E0ACB9CE0DB6602690448D88DD34BEA1
】或是，按照后续的教程，使用独立的数据库进行支持；

输入秘钥后，sofa会尝试连接数据库，提示连接成功即可使用。

## 2. Sofa命令全景

```bash
> sofa --help

# output
Usage: sofa [options] [command]

Options:
  -h, --help        output usage information

Commands:
  init              初始化Sofa
  set <assign>      修改/添加 全局设置，sofa set db=sth
  publish <type>    发布模板到模板库 type = template | micro
  update            更新模板（仅自己拥有权限的模板）
  remove            删除模板（仅自己拥有权限的模板）
  ls                显示所有可用模板
  search [keyword]  搜索模板
  create <type>     创建代码
  add <type>        以子模块方式添加微服务（模块）/组件 type = micro | component
```

## 3. 创建模板库

### 模板要求

【要求一】Sofa中的模板，要求提供标准格式的config文件，如下所示：

```javascript
// sofa.template.config.js

const path = require("path");
const { Sofa } = require("sofa-new-plugins");

module.exports = {
  // 【必填】模板名称
  name: "sofa-project-react-ts",
  // 【必填】模板类型，支持 project|module|component|micro
  type: "project",
  // 【必填】模板框架，支持 vue|react|react-native|mp|h5
  frame: "react",
  // 【必填】是否使用了typescript进行开发
  isTs: true,
  // 【必填】是否是空白模板，仅当空白component时为true
  isBlank: false,
  // 【必填】描述，对模板的具体描述
  description:
    "通用React项目模板，集成了系统管理、Pass登录等功能，配置后可直接使用，并提供了丰富的module备选模板；",
  // project 必填，module路径
  modulesPath: path.join(__dirname, "/src/containers"),
  // project 必填，组件路径
  componentsPath: path.join(__dirname, "/src/components"),
  // project 必填，微服务路径
  microsPath: path.join(__dirname, "/"),
  // 插件列表，在插件章节会详细介绍
  plugins: [
    // 按照顺序执行各个plugin
    new Sofa.Plugins.ReplaceKeywordPlugin([
      {
        originStr: "sofa-project-react-ts",
        targetStr: "$$projectName$$" // $$表示从运行实体中获得
      }
    ])
  ]
};
```

由于Sofa的运行是在依赖安装之前，因此配置文件中对外部模块的依赖仅限于node内置模块、Sofa模块；【Todo】

【要求二】模板需以独立的Git仓库形式存在；

### 发布上传模板

```bash
cd templateDir
sofa publish template
```

sofa会自动读取sofa.template.config.js中的设置，并与用户进行二次确认，信息确认完毕后，打Tag，存入数据库。

### 模板版本管理

sofa为每个模板提供了自动的版本管理，借助git tag的功能完成，因此不必担心拉取到的代码为未确认版本；

## 4.生成代码

sofa的生成代码是其核心应用命令；生成代码有两种模式，一种是Create模式，一种是Add模式；所谓Create模式，模板将以代码的形式存在，方便使用者在其上进行修改、二次开发；Add模式，模板将以Git子模块的形式存在，不允许修改，但是可以享受功能的自动升级，一般微服务类、组件类推荐使用Add模式；

### Create 模式

```bash
# 以create模式创建项目，会根据用户输入，为用户提供可选的模板
sofa create project

# 以create模式创建模块
sofa create module

# 以create模式创建组件
sofa create component

# 以create模式创建微服务【不推荐】
sofa create micro
```

### Add 模式

```bash
# 以add模式引入组件
sofa add component

# 以add模式引入微服务
sofa add micro
```

## 5.插件

插件部分参见sofa-new-plugins的ReadMe。
<a href="/sofa-plugins">sofa-new-plugins</a>

## 6.独立数据库

待完善

## 7.内置模板介绍

待完善
