# 测试用例

- 单元测试都必须使用 [GoConvey](http://goconvey.co/) 编写，且辅助包覆盖率必须在 80% 以上。

### 使用示例

- 为辅助包书写使用示例的时，文件名均命名为 `example_test.go`。
- 测试用例的函数名称必须以 `Test_` 开头，例如：`Test_Logger`。
- 如果为方法书写测试用例，则需要以 `Text_<Struct>_<Method>` 的形式命名，例如：`Test_Macaron_Run`。