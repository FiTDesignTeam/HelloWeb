# 插件文档

### 相关配置
1. **快捷键绑定**：package.json的keymap字段中设置，可区分平台。
2. **样式文件设置**：package.json的style字段。
> 关于css作用域：
> 1. 如果使用`renderVue`来渲染视图，则会给css自动加上id前缀作为css作用域限制。如不使用vue视图，插件的css作用域默认是全局。
> 2. 如果使用vue视图，同时又想让css的作用域提升至全局，可以同时设置package.json的globalStyle字段，此时，globalStyle字段的css作用域为全局，style资源的css作用域为插件局部。

3. **设置配置**：index.js的config字段。
4. **Vue渲染视图**：新建视图文件view.vue，在里面编写Vue的template，然后在index.js里调用HelloWeb.renderVue(vueOption, this)即完成视图渲染。
> 如果不需要Vue视图，使用原生js生成视图即可。
5. **Vue组件使用**：插件系统默认引入开源组件库[Element](http://element-cn.eleme.io/#/zh-CN)，请尽量使用[Element](http://element-cn.eleme.io/#/zh-CN)完成视图开发，以保证视图风格一致性。
6. **图片引用**：
  1. css引用图片使用相对路径
  2. HTML引用图片 - coming soon



### 示例

package.json

``` json
{
  "name": "setting-view",
  "version": "1.0.0",
  "description": "Core plugin for HelloWeb to edit setting",
  "main": "index.js",
  "style": "index.css",
  "keymap": {
    "mac": {
      "Command+,": "setting-view:open",
      "Command+w": "setting-view:close"
    },
    "win": {
      "Control+,": "setting-view:open",
      "Control+w": "setting-view:close"
    }
  },
  "scripts": {},
  "author": "Sheldon Law",
  "license": "MIT",
  "dependencies": {
    "markdown-to-html": "0.0.13"
  }
}
```

index.js

``` js
const { Markdown } = require('markdown-to-html');
const path = require('path');
const fs = require('fs-extra');
const common = require('../../media/js/_tasks/common.js');

module.exports = {
	// 配置格式 - 以this.testString的形式来访问
	config: {
 	 	testString: {
	     	type: 'string',
	       	title: '字符串设置',
	       	description: 'order-1',
	       	default: '',
	       	order: 1
		}
	}
	/**
	 * 生命周期：activate 插件被调用时执行
	 */
	activate: function() {
	},
	/**
	 * 生命周期：deactivate 插件被关闭时执行
	 */
	deactivate: function() {
	}
};

```







### 接口说明

``` js

/**
 * isWindows 判断系统是否为window系统
 * @return {Boolean} true || false
 */
HelloWeb.isWindows


/**
 * isMac 判断系统是否为mac系统
 * @return {Boolean} true || false
 */
HelloWeb.isMac

/**
 * renderVue 利用Vue来渲染view
 * @param  {Object} option         Vue配置对象
 * 如:
 * {
 *   data: function () {return {a: 1} },
 *   method: {
 *     handleClick: function(e) { console.log(e) }
 *   }
 * }
 * @param  {Plugin} pluginInstance 插件实例 = this
 * @return {VDOM}                  Vue虚拟DOM对象
 */
HelloWeb.renderVue(option, pluginInstance)


/**
 * getCurrentProject 获取当前项目信息
 * @return {Object} 项目信息
 */
HelloWeb.getCurrentProject()

/**
 * execCommand 执行shell脚本
 * @param  {String} command 需要执行的脚本
 * @param  {Function} onEnd 进程结束回调
 * @return {Function} kill - 关闭当前进程的接口
 */
HelloWeb.execCommand(command, onEnd)

/**
 * addProjectChangeListener 添加对项目切换的事件监听
 * @param {Function} callback 回调函数
 */
HelloWeb.addProjectChangeListener(callback)
HelloWeb.removeProjectChangeListener(callback)

/**
 * chooseFolderPath 弹窗，选择文件夹路径
 * @param  {Function} callback 选择完成后的回调函数，返回路径
 * @return {null}            
 */
HelloWeb.chooseFolderPath(callback)

/**
 * log 输出日志到日志面板（在工具面板的下方）
 * @param  {String} msg 日志信息
 * @return {null}
 */
HelloWeb.log(msg)

/**
 * addContextMenu 给文件管理器添加右键项
 * @param  {String} label 右键项显示
 * @param  {String} selector 选择器，格式为"数量&类型", 例如"one&css"、"multiple&html"，"other"表示其他类型
 * @param  {Function} handler 点击响应函数，返回所选文件数组
 * @return {null}
 */
HelloWeb.addContextMenuItem(label, selector, handler)

```
