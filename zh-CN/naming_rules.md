# 命名规则

### 函数或方法

- 若函数或方法为判断类型（返回值主要为 `bool` 类型），则名称应以 `Has` 或 `Is` 开头：

	```
func HasPrefix(name string, prefixes []string) bool { ...
func IsEntry(name string, entries []string) bool { ...
	```
	
### 变量

- 变量命名基本上遵循相应的英文翻译。
- 在相对简单的环境（对象数量少、针对性强）中，可以将一些名称由完整单词简写为单个字母，例如：
	- `user` 可以简写为 `u`
	- `userId` 可以简写 `uid`