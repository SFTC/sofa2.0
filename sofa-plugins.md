# sofa-new-plugins

首先为什么叫new-plugins，是由于插件模式的方案讨论了比较多轮，最终使用的方案与前期讨论的差距较大，所以最终叫了new-plugins的名字。  

sofa的插件是用以进行除git操作、文件复制以外的文件内容处理的，主要被用在create命令中。sofa内置了一些同步、异步插件，帮助模板开发者快速达成自定义create模式的目的。  

需要注意的是，多个插件会按序同步执行。  

## 内置插件

### ReplaceKeywordPlugin

关键字替换插件

```javascript
const { Sofa } = require("sofa-new-plugins");

module.exports = {
  name: "sofa-module-cv",
  type: "module",
  frame: "react",
  isTs: 1,
  isBlank: false,
  description: "Connect-View 页面模板；",
  plugins: [
    new Sofa.Plugins.ReplaceKeywordPlugin([
      {
        // 源字符串
        originStr: "EventManage",
        // 目标字符串
        targetStr: "$$moduleName$$",
      }
    ]),
    ……
```

传参：替换对儿数组，可以替换多个关键字。

### CollectParamsPlugin

参数收集插件

与用户交互，得到其他参数，插件的参数与inquirer一致即可。

```javascript
  new Sofa.Plugins.CollectParamsPlugin([
    {
      type: "input",
      message: "请输入module的中文名称",
      name: "moduleChineseName",
      default: "$$moduleName$$"
    }
  ]),
```

### InsertCodePlugin

代码生成插件

寻找指定的变量（导出变量，export default均可），在变量之上插入新的数组元素或者Object的键值对。

```javascript
  new Sofa.Plugins.InsertCodePlugin({
    filePath: "$$projectRoot$$/src/config/menu.conf.js",
    findType: "identify",
    findName: "menu",
    insertValue: {
      key: "$$moduleName$$",
      icon: "tool"
    }
  }),
  new Sofa.Plugins.InsertCodePlugin({
    filePath: "$$projectRoot$$/src/config/menu.conf.js",
    findType: "identify",
    findName: "messagesMap",
    insertKey: "$$moduleName$$",
    insertValue: {
      id: "sofa.config.$$moduleName$$",
      defaultMessage: "$$moduleChineseName$$"
    }
  })
```

## 替换字符串

sofa-new-plugins预置了一种特殊的替换字符串语法，在插件的配置参数中将$$包裹的变量进行替换，例如在项目'test'中，$$projectName$$会被替换为test。  

下面列出了可被替换的变量名称：

number | name | description
-|-|-
1 | operator | 操作者
2 | email | 操作者Email
3 | operator | 操作者
4 | projectName | 项目名称
5 | moduleName | 模块名称
6 | projectRoot | 项目路径

此外任何其他需要收集的变量信息都可以通过CollectParamsPlugin插件与用户交互进行收集。

## 用户自定义插件

插件的开发非常简单，在类中实现如下apply方法即可：

```javascript
class BasePlugin {
  /**
   * 构造函数
   * @param {*} params 初始化参数
   */
  constructor(params) {
    this.params = params;
  }

  /**
   * 核心执行函数
   * @param {object} files file列表
   * @param {Sofa} sofaEntity Sofa实体，用以传递Sofa执行参数
   * @param {function} callback 回调函数，需在执行末尾调用
   */
  apply(files, sofaEntity, callback) {
    // core code
    callback(error, files);
  }
}
```
