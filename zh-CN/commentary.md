# 注释规范

- 所有的注释说明都必须是一个完整的句子，而非短语。
- 任何导出与非导出对象都需要注释说明其用途。
- 如果对象可数且无明确指定数量的情况下，一律使用单数形式描述。

### 包级别

- 包级别的注释就是对包的介绍，只需在同个包的任一源文件中说明即可有效。
- 对于 `main` 包，一般只有一行简短的注释用以说明包的用途，且以项目名称开头：
		
	```
// Gogs(Go Git Service) is a Self Hosted Git Service in the Go Programming Language.
package main
	```
	
- 对于一个复杂项目的子包，一般情况下不需要包级别注释，除非是代表某个特定功能的模块。
- 对于简单的非 `main` 包，也可用一行注释概括。
- 对于相对功能复杂的非 `main` 包，一般都会增加一些使用示例或基本说明，且以 `Package <name>` 开头：

	```
	/*
	Package regexp implements a simple library for regular expressions.
	
	The syntax of the regular expressions accepted is:
	
	    regexp:
	        concatenation { '|' concatenation }
	    concatenation:
	        { closure }
	    closure:
	        term [ '*' | '+' | '?' ]
	    term:
	        '^'
	        '$'
	        '.'
	        character
	        '[' [ '^' ] character-ranges ']'
	        '(' regexp ')'
	*/
	package regexp
	```

- 特别复杂的包说明，可单独创建 `doc.go` 文件来加以说明。

### 结构、接口及其它类型

- 类型的定义一般都以单数形式描述：

	```
// A Request represents a request to run a command.
type Request struct { ...
	```

### 函数与方法

- 函数与方法的注释需以函数或方法的名称作为开头：

	```
// Post returns *BeegoHttpRequest with POST method.
	```
	
- 如果一句话不足以说明全部问题，则可换行继续进行更加细致的描述：

	```
// Copy copies file from source to target path.
// It returns false and error when error occurs in underlying function calls.
	```
	
- 若函数或方法为判断类型（返回值主要为 `bool` 类型），则以 `<name> returns true if` 开头：

	```
// HasPrefix returns true if name has any string in given slice as prefix.
func HasPrefix(name string, prefixes []string) bool { ...
	```