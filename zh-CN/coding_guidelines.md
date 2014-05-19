# 代码指导

### 基本约束

- 当单行代码超过 80 个字符时，就要考虑分行。分行的规则是以参数为单位将从较长的参数开始换行，以此类推直到每行长度合适：

	```
So(z.ExtractTo(
	path.Join(os.TempDir(), "testdata/test2"),
	"dir/", "dir/bar", "readonly"), ShouldBeNil)
	```
	
- 分组声明一般需要按照功能来区分，而不是将所有类型都分在一组：

	```
	const (
		// Default section name.
		DEFAULT_SECTION = "DEFAULT"
		// Maximum allowed depth when recursively substituing variable names.
		_DEPTH_VALUES = 200
	)
	
	// Parse error types.
	const (
		ErrSectionNotFound = iota + 1
		ErrKeyNotFound
		ErrBlankSectionName
		ErrCouldNotParse
	)
	```
	
- 当一个源文件中存在多个相对独立的部分时，为方便区分，需使用由 [ASCII Generator](http://www.network-science.de/ascii/) 提供的句型字符标注（示例：`Comment`）：

	```
// _________                                       __
// \_   ___ \  ____   _____   _____   ____   _____/  |_
// /    \  \/ /  _ \ /     \ /     \_/ __ \ /    \   __\
// \     \___(  <_> )  Y Y  \  Y Y  \  ___/|   |  \  |
//  \______  /\____/|__|_|  /__|_|  /\___  >___|  /__|
//         \/             \/      \/     \/     \/
	```

- 函数或方法的顺序一般需要按照依赖关系由浅入深由上至下排序，即最底层的函数出现在最前面。例如，下方的代码，函数 `ExecCmdDirBytes` 属于最底层的函数，它被 `ExecCmdDir` 函数调用，而 `ExecCmdDir` 又被 `ExecCmd` 调用：

	```
	// ExecCmdDirBytes executes system command in given directory
	// and return stdout, stderr in bytes type, along with possible error.
	func ExecCmdDirBytes(dir, cmdName string, args ...string) ([]byte, []byte, error) {
		bufOut := new(bytes.Buffer)
		bufErr := new(bytes.Buffer)
	
		cmd := exec.Command(cmdName, args...)
		cmd.Dir = dir
		cmd.Stdout = bufOut
		cmd.Stderr = bufErr
	
		err := cmd.Run()
		return bufOut.Bytes(), bufErr.Bytes(), err
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


