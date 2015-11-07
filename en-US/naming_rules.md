# Naming Rules

### File name

- The main entry point file of the application should be named as `main.go` or same as the application. For example, the entry point file of `Gogs` is named `gogs.go`.

### Functions and Methods

- If the main purpose of functions or methods is returning a `bool` type value, the name of function or method should starts with `Has`, `Is`, `Can` or `Allow`, etc.

	```go
	func HasPrefix(name string, prefixes []string) bool { ... }
	func IsEntry(name string, entries []string) bool { ... }
	func CanManage(name string) bool { ... }
	func AllowGitHook() bool { ... }
	```

### Constants

- Constant should use all capital letters and use underscore `_` to separate words.

	```go
	const APP_VER = "0.7.0.1110 Beta"
	```

- If you need enumerated type, you should define the corresponding type first:

	```go
	type Scheme string

	const (
		HTTP  Scheme = "http"
		HTTPS Scheme = "https"
	)
	```

- If functionality of the module is relatively complicated and easy to mixed up with constant name, you can add prefix to every constant:

	```go
	type PullRequestStatus int

	const (
		PULL_REQUEST_STATUS_CONFLICT PullRequestStatus = iota
		PULL_REQUEST_STATUS_CHECKING
		PULL_REQUEST_STATUS_MERGEABLE
	)
	```

### Variables

- A variable name should follow general English expression or shorthand.
- In relatively simple (less objects and more specific) context, variable name can use simplified form as follows:
    - `user` to `u`
    - `userID` to `uid`
- If variable type is `bool`, its name should start with `Has`, `Is`, `Can` or `Allow`, etc.

	```go
	var isExist bool
	var hasConflict bool
	var canManage bool
	var allowGitHook bool
	```

- The last rule also applies for defining structs:

	```go
	// Webhook represents a web hook object.
	type Webhook struct {
		ID           int64 `xorm:"pk autoincr"`
		RepoID       int64
		OrgID        int64
		URL          string `xorm:"url TEXT"`
		ContentType  HookContentType
		Secret       string `xorm:"TEXT"`
		Events       string `xorm:"TEXT"`
		*HookEvent   `xorm:"-"`
		IsSSL        bool `xorm:"is_ssl"`
		IsActive     bool
		HookTaskType HookTaskType
		Meta         string     `xorm:"TEXT"` // store hook-specific attributes
		LastStatus   HookStatus // Last delivery status
		Created      time.Time  `xorm:"CREATED"`
		Updated      time.Time  `xorm:"UPDATED"`
	}
	```

#### Variable Naming Convention

Variable name is generally using Camel Case style, but when you have unique nouns, should apply following rules:

- If the variable is private, and the unique noun is the first word, then use lower cases, e.g. `apiClient`.
- Otherwise, use the original cases for the unique noun, e.g. `APIClient`, `repoID`, `UserID`.

Here is a list of words which are commonly identified as unique nouns:

```go
// A GonicMapper that contains a list of common initialisms taken from golang/lint
var LintGonicMapper = GonicMapper{
	"API":   true,
	"ASCII": true,
	"CPU":   true,
	"CSS":   true,
	"DNS":   true,
	"EOF":   true,
	"GUID":  true,
	"HTML":  true,
	"HTTP":  true,
	"HTTPS": true,
	"ID":    true,
	"IP":    true,
	"JSON":  true,
	"LHS":   true,
	"QPS":   true,
	"RAM":   true,
	"RHS":   true,
	"RPC":   true,
	"SLA":   true,
	"SMTP":  true,
	"SSH":   true,
	"TLS":   true,
	"TTL":   true,
	"UI":    true,
	"UID":   true,
	"UUID":  true,
	"URI":   true,
	"URL":   true,
	"UTF8":  true,
	"VM":    true,
	"XML":   true,
	"XSRF":  true,
	"XSS":   true,
}
```
