# 项目结构

以下为一般项目结构，根据不同的 Web 框架习惯，可使用括号内的文字替换；根据不同的项目类型和需求，可自由增删某些结构：

```
- templates (views)          # 模板文件
- public (static)            # 静态文件
	- css                    
	- fonts                  
	- img                    
	- js                     
- routes                     # 路由逻辑处理
- models					 # 数据逻辑层
- pkg                        # 子模块
	- setting                # 应用配置存取
- cmd                        # 命令行程序命令
- conf                       # 默认配置
	- locale                 # i18n 本地化文件
- custom                     # 自定义配置
- data                       # 应用生成数据文件
- log                        # 应用生成日志文件
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
