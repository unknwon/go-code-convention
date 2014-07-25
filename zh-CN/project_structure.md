# 项目结构

以下为一般项目结构，根据不同的 Web 框架习惯，可使用括号内的文字替换；根据不同的项目类型和需求，可自由增删某些结构：

```
- templates(views)：用于存放模板文件
- public(static)：用于存放静态文件
	- css：用于存放 CSS 文件
	- fonts：用于存放字体文件
	- img：用于存放图像文件
	- js：用于存放 JavaScript 文件
- routers(controllers)：用于存放路由文件
- models：用于存放业务逻辑层文件
- modules：用于存放子模块文件
	- setting：用于存放应用配置存取文件
- cmd：用于存放命令行程序命令文件
- conf：用于存放配置文件
	- locale: 用于存放 i18n 本地化文件
- data：用于存放应用生成数据文件
- log：用于存放应用生成日志文件
```

## 命令行应用

当应用类型为命令行应用时，需要将命令相关文件存放于 `/cmd` 目录下，并为每个命令创建一个单独的源文件：

```
/cmd
	dump.go
	fix.go
	serve.go
	update.go
	web.go
```