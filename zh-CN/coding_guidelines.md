# 代码指导

### 基本约束

- 所有应用的 `main` 包需要有 `APP_VER` 常量表示版本，格式为 `X.Y.Z.Date Status`，例如：`0.6.5.0524 Alpha`。
- 单独的库需要有函数 `Version` 返回库版本号的字符串，格式为 `X.Y.Z`。
- 当单行代码超过 80 个字符时，就要考虑分行。分行的规则是以参数为单位将从较长的参数开始换行，以此类推直到每行长度合适：

	```go
	So(z.ExtractTo(
		path.Join(os.TempDir(), "testdata/test2"),
		"dir/", "dir/bar", "readonly"), ShouldBeNil)
	```
	
- 当单行声明语句超过 80 个字符时，就要考虑分行。分行的规则是将参数按类型分组，紧接着的声明语句的是一个空行，以便和函数体区别：

	```go
	// NewNode initializes and returns a new Node representation.
	func NewNode(
		importPath, downloadUrl string,
		tp RevisionType, val string,
		isGetDeps bool) *Node {
	
		n := &Node{
			Pkg: Pkg{
				ImportPath: importPath,
				RootPath:   GetRootPath(importPath),
				Type:       tp,
				Value:      val,
			},
			DownloadURL: downloadUrl,
			IsGetDeps:   isGetDeps,
		}
		n.InstallPath = path.Join(setting.InstallRepoPath, n.RootPath) + n.ValSuffix()
		return n
	}
	```
	
- 分组声明一般需要按照功能来区分，而不是将所有类型都分在一组：

	```go
	const (
		// Default section name.
		DEFAULT_SECTION = "DEFAULT"
		// Maximum allowed depth when recursively substituing variable names.
		_DEPTH_VALUES = 200
	)
	
	type ParseError int
	
	const (
		ERR_SECTION_NOT_FOUND ParseError = iota + 1
		ERR_KEY_NOT_FOUND
		ERR_BLANK_SECTION_NAME
		ERR_COULD_NOT_PARSE
	)
	```
	
- 当一个源文件中存在多个相对独立的部分时，为方便区分，需使用由 [ASCII Generator](http://www.network-science.de/ascii/) 提供的句型字符标注（示例：`Comment`）：

	```go
	// _________                                       __
	// \_   ___ \  ____   _____   _____   ____   _____/  |_
	// /    \  \/ /  _ \ /     \ /     \_/ __ \ /    \   __\
	// \     \___(  <_> )  Y Y  \  Y Y  \  ___/|   |  \  |
	//  \______  /\____/|__|_|  /__|_|  /\___  >___|  /__|
	//         \/             \/      \/     \/     \/
	```

- 函数或方法的顺序一般需要按照依赖关系由浅入深由上至下排序，即最底层的函数出现在最前面。例如，下方的代码，函数 `ExecCmdDirBytes` 属于最底层的函数，它被 `ExecCmdDir` 函数调用，而 `ExecCmdDir` 又被 `ExecCmd` 调用：

	```go
	// ExecCmdDirBytes executes system command in given directory
	// and return stdout, stderr in bytes type, along with possible error.
	func ExecCmdDirBytes(dir, cmdName string, args ...string) ([]byte, []byte, error) {
		...
	}
	
	// ExecCmdDir executes system command in given directory
	// and return stdout, stderr in string type, along with possible error.
	func ExecCmdDir(dir, cmdName string, args ...string) (string, string, error) {
		bufOut, bufErr, err := ExecCmdDirBytes(dir, cmdName, args...)
		return string(bufOut), string(bufErr), err
	}
	
	// ExecCmd executes system command
	// and return stdout, stderr in string type, along with possible error.
	func ExecCmd(cmdName string, args ...string) (string, string, error) {
		return ExecCmdDir("", cmdName, args...)
	}
	```
- 结构附带的方法应置于结构定义之后，按照所对应操作的字段顺序摆放方法：

	```go
	type Webhook struct { ...
	func (w *Webhook) GetEvent() { ...
	func (w *Webhook) SaveEvent() error { ...
	func (w *Webhook) HasPushEvent() bool { ...
	```

- 如果一个结构拥有对应操作函数，大体上按照 `CRUD` 的顺序放置结构定义之后：

	```go
	func CreateWebhook(w *Webhook) error { ...
	func GetWebhookById(hookId int64) (*Webhook, error) { ...
	func UpdateWebhook(w *Webhook) error { ...
	func DeleteWebhook(hookId int64) error { ...
	```
- 如果一个结构拥有以 `Has`、`Is`、`Can` 或 `Allow` 开头的函数或方法，则应将它们至于所有其它函数及方法之前；这些函数或方法以 `Has`、`Is`、`Can`、`Allow` 的顺序排序。
- 结构所属的方法需要置于结构所属的函数之前。
- 如果某个类型的变量的某个字段为函数类型，则习惯性将该变量置于最前，相关函数紧随其后：

	```go
	var CmdDump = cli.Command{
		Name:  "dump",
		...
		Action: runDump,
		Flags:  []cli.Flag{},
	}
	
	func runDump(*cli.Context) { ...
	```
- 在初始化结构时，不能使用匿名初始化，必须用一一对应方式：

	```go
	AddHookTask(&HookTask{
		Type:        HTT_WEBHOOK,
		Url:         w.Url,
		Payload:     p,
		ContentType: w.ContentType,
		IsSsl:       w.IsSsl,
	})
	```
- 如果需要初始化的字段的字母总数量较少，可以合并到一行：

	```go
	// ...
	return x.Get(&User{OrgEmail: email})
	```


