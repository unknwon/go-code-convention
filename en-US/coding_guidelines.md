# Coding Guidelines

### Basic Rules

- All applications that have `main` package should have constant `APP_VER` to indicate its version with format `X.Y.Z.Date [Status]`. For example, `0.7.6.1112 Beta`.
- A library/package should have function `Version` to return its version in `string` type with format `X.Y.Z[.Date]`.
- You should consider split lines of code that have more than 80 characters. The rule is use to argument as the unit to split:

	```go
	So(z.ExtractTo(
		path.Join(os.TempDir(), "testdata/test2"),
		"dir/", "dir/bar", "readonly"), ShouldBeNil)
	```

- When the function or method's declaration has more than 80 characters, you should consider split it as well. The rule is arguments with same type in the same line. After declaration, you also need to put an empty line to distinguish itself with function or method's body:

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

- Group declaration should be organized by types, not all together:

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

- When there are more than one functional modules in single source file, you should use [ASCII Generator](http://www.network-science.de/ascii/) to make a highlight title:

	```go
	// _________                                       __
	// \_   ___ \  ____   _____   _____   ____   _____/  |_
	// /    \  \/ /  _ \ /     \ /     \_/ __ \ /    \   __\
	// \     \___(  <_> )  Y Y  \  Y Y  \  ___/|   |  \  |
	//  \______  /\____/|__|_|  /__|_|  /\___  >___|  /__|
	//         \/             \/      \/     \/     \/
	```

- Functions or methods are ordered by the dependency relationship, such that the most dependent function or method should be at the top. In the following example, `ExecCmdDirBytes` is the most fundamental function, it's called by `ExecCmdDir`, and `ExecCmdDir` is also called by `ExecCmd`:

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

- Methods of struct should be put after struct definition, and order them by the order of fields they mostly operate on:

	```go
	type Webhook struct { ... }
	func (w *Webhook) GetEvent() { ... }
	func (w *Webhook) SaveEvent() error { ... }
	func (w *Webhook) HasPushEvent() bool { ... }
	```

- If a struct has operational functions, should basically follow the `CRUD` order:

	```go
	func CreateWebhook(w *Webhook) error { ... }
	func GetWebhookById(hookId int64) (*Webhook, error) { ... }
	func UpdateWebhook(w *Webhook) error { ... }
	func DeleteWebhook(hookId int64) error { ... }
	```

- If a struct has functions or methods start with `Has`, `Is`, `Can` or `Allow`, they should be ordered by the same order of `Has`, `Is`, `Can` or `Allow`.
- Declaration of variables should be put before corresponding functions or methods:

	```go
	var CmdDump = cli.Command{
		Name:  "dump",
		...
		Action: runDump,
		Flags:  []cli.Flag{},
	}

	func runDump(*cli.Context) { ...
	```

- Use named field to initialize struct fields whenever possible:

	```go
	AddHookTask(&HookTask{
		Type:        HTT_WEBHOOK,
		Url:         w.Url,
		Payload:     p,
		ContentType: w.ContentType,
		IsSsl:       w.IsSsl,
	})
	```
