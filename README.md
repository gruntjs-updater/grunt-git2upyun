# grunt-git2upyun

> grunt plugin for uploading files managed by git to UPYUN.

> 用于将 git 仓库中管理的文件同步到又拍云。

## Getting Started
This plugin requires Grunt `~0.4.4`

If you haven't used [Grunt](http://gruntjs.com/) before, be sure to check out the [Getting Started](http://gruntjs.com/getting-started) guide, as it explains how to create a [Gruntfile](http://gruntjs.com/sample-gruntfile) as well as install and use Grunt plugins. Once you're familiar with that process, you may install this plugin with this command:

```shell
npm install grunt-git2upyun --save-dev
```

Once the plugin has been installed, it may be enabled inside your Gruntfile with this line of JavaScript:

```js
grunt.loadNpmTasks('grunt-git2upyun');
```

## The "git2upyun" task

### Overview
In your project's Gruntfile, add a section named `git2upyun` to the data object passed into `grunt.initConfig()`.

```js
grunt.initConfig({
  git2upyun: {
    your_target: {
	    options: {
	      // Target-specific file lists and/or options go here.
	    }
      
    },
  },
});
```

### Options

#### options.bucketname
Type: `String`
Default value: `''`

空间名称。请参考又拍云对此术语的定义。

#### options.username
Type: `String`
Default value: `''`

操作员名称。请参考又拍云对此术语的定义。

#### options.password
Type: `String`
Default value: `''`

操作员密码。请参考又拍云对此术语的定义。

#### options.root
Type: `String`
Default value: `''`

文件在云存储服务器上存储的根目录。可以是`/`，表示空间的根目录。所有上传的文件最终路径是：
```javascript
path.join(options.root, '文件在仓库中的相对路径')
```

#### options.repository
Type: `String`
Default value: `./`

本地仓库路径，此仓库中所管理的文件都将上传至又拍云。

#### options.ignore
Type: `String` || `array`
Default value: `[]`

忽略哪些文件。采用 `grunt.file.expand` 函数对传入的参数进行解析。请参考：http://gruntjs.com/api/grunt.file#grunt.file.expand

#### options.cleanpath
Type: `function`
Default value: `false`
Function paramter: `fullpath`

在文件将要上传到又拍云或需要在又拍云上删除之前，又拍云上所存储的文件的路径都会经过此函数进行处理，也就是说可以通过此函数修改文件最终在又拍云上存储的路径。


### Usage Examples

我们假设本地git仓库路径为 `/data/git/repo` ，配置信息如下：

```js
grunt.initConfig({
  git2upyun: {
    options: {
      bucketname: 'my-first-bucket', //空间名称
      username: 'somebody', //操作员名称
      password: 'secret', //密码
      root: '/images/', //文件存储的目录，可以是 '/'

      repository: '/data/git/repo', //本地仓库路径
      ignore:['.*', 'README.md'], //忽略哪些文件
      cleanpath: false //上传文件之前会调用此函数处理目标存储路径
    }
  },
});
```

#### cleanpath 的用法

举例来说，本地仓库中的文件存放路径为（相对于仓库目录的路径） `ajax/lig/bootstrap/bootstrap.js`，`root` 配置为 `/3rd` ，如果按照默认的生成路径的方式，最终在服务器上的文件路径为 `/3rd/ajax/lib/bootstrap/bootstrap.js` ，但是我们希望存储的路径是 `/3rd/bootstrap/bootstrap.js` ，那么，就需要将 `ajax/lib/` 删除掉，这是，cleanpath就能派上用场了：

```js
cleanpath:function(fullpath) {return fullpath.replace('ajax/lib/', '');}
```

这样就能将默认路径修改为我们需要的结果了！

## 实际使用案例

[Bootstrap中文网](http://www.bootcss.com)所维护的[开放CDN](http://open.bootcss.com)服务就是采用此Grunt插件将 cdnjs.com 的github仓库镜像到又拍云上，从而为国内用户提供更好的加速服务。


## 版权和协议

本插件所有代码版权归 [Bootstrap中文网](http://www.bootcss.com) 所有，遵循 MIT 开源协议。
其中，`lib/upyun.js` 文件中的代码归[又拍云](http://www.upyun.com)所有。