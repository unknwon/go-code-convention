# Go Code Convention

## Copyright

For any open source project, there must be a LICENSE file in the repository root to claim the rights.

Here are two examples of using [Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0) and MIT License.

### Apache License, Version 2.0

This license requires to put following content at the beginning of every file:

```
// Copyright [yyyy] [name of copyright owner]
//
// Licensed under the Apache License, Version 2.0 (the "License"): you may
// not use this file except in compliance with the License. You may obtain
// a copy of the License at
//
//     http://www.apache.org/licenses/LICENSE-2.0
//
// Unless required by applicable law or agreed to in writing, software
// distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
// WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the
// License for the specific language governing permissions and limitations
// under the License.
```

Replace `[yyyy]` with the creation year of the file. Then use personal name for personal projects, or organization name for team projects to replace `[name of copyright owner]`.

### MIT License

This license requires to put following content at the beginning of every file:

```
// Copyright [yyyy] [name of copyright owner]. All rights reserved.
// Use of this source code is governed by a MIT-style
// license that can be found in the LICENSE file.
```

Replace `[yyyy]` with the creation year of the file.

### Other notes

- Other types of license can follow the template of above two examples.

- If a file has been modified by different individuals and/or organizations, and multiple licenses are compatiable, then change the first line to multiple lines and sort them in the order of time:

  ```
  // Copyright 2011 Gary Burd
  // Copyright 2013 Unknwon
  ```

- Spefify which license is used for the project in bottom of the README file:

  ```
  ## License
  
  This project is under the MIT License. See the [LICENSE](LICENSE) file for the full license text.
  ```

## Project structure

For server-side services and CLI applications, they generally follow the same style of project structure as follows:

```
.bin/                # The directory to put all locally compiled binaries, should be ignored by .gitignore
.github/             # GitHub specific configuration
assets/              # Static resources, including those to be embeded into binaries
    migrations/      # Files for database schema migrations
    templates/       # Template files for rendering HTML pages
    embed.go         # The main file to tell Go toolchain to embed resources
cmd/                 # All "main" packages which produce binaries should be placed under this directory
    <logan smith>/
        main.go      # The entry point of a main package
data/                # Application generated data
dev/                 # Scripts for enhancing quality-of-life of development 
docs/                # Project specific documentation
internal/            # Common packages, "internal" is also a keyword to prevent other projects to import in Go
    conf/            # Read and/or write application configuration
    db/              # Database layer
    route/           # Routing layer
log/                 # Appication generated logs
.golangci.yml        # Custom configuration for golangci-lint
```

### Example `embed.go`

The following shows an example of embeding database schema migrations using Go 1.16 [embed](https://blog.carlmjohnson.net/post/2021/how-to-use-go-embed/):

```go
package assets

import (
	"embed"
)

//go:embed migrations/*.sql
var Migrations embed.FS
```

## Import packages

All package import paths must be valid Go Modules path except packages from standard library.

Generally, there are four types of packages a source file could import:

1. Packages from standard library
2. Third-party packages
3. Packages in the same organization but not in the same repository
4. Packages in the same repository

Basic rules:

- Use different groups to separate import paths by an blank line for two or more types of packages.
- Must not use `.` to simplify import path except in test files (`*_test.go`).
- Must not use relative path (`./subpackage`) as import path.

Here is a concrete example:

```go
import (
    "fmt"
    "html/template"
    "net/http"
    "os"

    "github.com/urfave/cli"
    "gopkg.in/macaron.v1"

    "github.com/gogs/git-module"

    "github.com/gogs/gogs/internal/route"
    "github.com/gogs/gogs/internal/route/repo"
    "github.com/gogs/gogs/internal/route/user"
)
```

## Commentary

- All exported objects must be well-commented; internal objects can be commented when needed.
- If the object is countable and does not know the number of it, use singular form and present tense; otherwise, use plural form.
- Comment of package, function, method and type must be a complete sentence (i.e. capitalize the first letter and end with a period).
- Maximum length of a comment line should be 80 characters.

### Package

- Only one file is needed for package-level comment.

- For `main` packages, a line of introduction is enough, and should start with the binary name:

  ```Go
  // Gogs is a painless self-hosted Git Service.
  package main
  ```

- For sub-packages of a complex project, no package level comment is required unless it's a functional module.

- For simple non-`main` packages, a line of introduction is enough as well.

- For more complex functional non-`main` packages, comment should have some basic usage examples and must start with `Package <name>`:

  ```Go
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

- When few paragraphs cannot explain the package well, you should create a [`doc.go`](https://github.com/robfig/cron/blob/master/doc.go) file for it.

### Struct and interface

- Type is often described in singular form, and the comment should state what it actuall does:

  ```Go
  // Request represents a request to run a command.
  type Request struct { ...
  ```

- Interface should be described as follows, and the comment should state what implementations should be doing:

  ```Go
  // FileInfo is the interface that describes a file and is returned by Stat and Lstat.
  type FileInfo interface { ...
  ```


### Functions and methods

- Comments of functions and methods must start with its name:

  ```Go
  // Post returns *BeegoHttpRequest with POST method.
  ```

- If one sentence is not enough, go to next line:

  ```Go
  // Copy copies file from source to target path.
  // It returns false and error when error occurs in underlying function calls.
  ```

- If the main purpose of a function or method is returning a `bool` value, its comment should start with `<name> returns true if`:

  ```Go
  // HasPrefix returns true if name has any string in given slice as prefix.
  func HasPrefix(name string, prefixes []string) bool { ...
  ```

### Other notes

- When something is waiting to be done, use comment starts with `TODO:` to remind maintainers.

- When a known problem/issue/bug needs to be fixed/improved, use comment starts with `FIXME:` to remind maintainers.

- When something is too magic and needs to be explained, use comment starts with `NOTE:`:

  ```Go
  // NOTE: os.Chmod and os.Chtimes don't recognize symbolic link,
  // which will lead "no such file or directory" error.
  return os.Symlink(target, dest)
  ```

- When dealing with security-related logic, use comment starts with `SECURITY:`
- When something important but is easy to overlook, use comment starts with `WARNING:`

- When the object is deprecated, use comment starts with `DEPRECATED:`:

  ```go
  // Email returns the user's oldest email, if one exists.
  // Deprecated: use Emails instead.
  func (r *UserResolver) Email(ctx context.Context) (string, error) {
  ```


## Naming rules

### Directories

- Do not use underscores (`_`) in directory names, use hyphens (`-`) instead.

### Files

- Do not use hyphens (`-`) in file names, use underscores (`_`) instead.
- The file contains the entry point of the application or package should be named as `main.go` or same as the package.

### Functions and methods

- If the main purpose of the function or method is returning a `bool` type value, the name of function or method should starts with `Has`, `Is`, `Can` or `Allow`, etc.

  ```go
  func HasPrefix(name string, prefixes []string) bool { ... }
  func IsEntry(name string, entries []string) bool { ... }
  func CanManage(name string) bool { ... }
  func AllowGitHook() bool { ... }
  ```

### Constants

- Constant should use camel cases except for coined terms and brand names (see later):

  ```go
  const appVersion = "0.13.0+dev"
  ```

- If you need enumerated type, you should define the corresponding type first:

  ```go
  type Scheme string
  
  const (
  	HTTP  Scheme = "http"
  	HTTPS Scheme = "https"
  )
  ```

- If functionality of the module is relatively complicated and easy to mixed up with constant name, you can add prefix to every constant for the enumerated type:

  ```go
  type PullRequestStatus int
  
  const (
  	PullRequestStatusConflict PullRequestStatus = iota
  	PullRequestStatusChecking
  	PullRequestStatusMergable
  )
  ```

### Variables

- A variable name should follow general English expression or shorthand.

- In relatively simple (less objects and more specific) context, variable name can use simplified form as follows:

  - `userID` to `uid`
  - `repository` to `repo`

- If variable type is `bool`, its name should start with `Has`, `Is`, `Can` or `Allow`, etc.

  ```go
  var isExist bool
  var hasConflict bool
  var canManage bool
  var allowGitHook bool
  ```

- Same rules also apply for defining structs:

  ```go
  // Webhook represents a web hook object.
  type Webhook struct {
  	ID           int64
  	RepoID       int64
  	OrgID        int64
  	URL          string
  	ContentType  HookContentType
  	Secret       string
  	Events       string
  	*HookEvent
  	IsSSL        bool
  	IsActive     bool
  	HookTaskType HookTaskType
  	Meta         string
  	LastStatus   HookStatus
  	CreatedAt    time.Time
  	UpdatedAt    time.Time
  }
  ```

### Coined terms and brand names

When you encounter coined terms and brand names, naming should respect the original or the most widely accepted form for the letter case. For example, use "GitHub" not "Github", use "API" not "Api", use "ID" not "Id".

When the coined term and brand name is the first word in a variable or constant name, keep them having the same case. For example, `apiClient`, `SSLCertificate`, `githubClient`.

Here is a list of words which are commonly identified as coined terms:

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

## Declarations

### Functions or methods

Order of arguments of functions or methods should generally apply following rules (from left to right):

1. More important to less important.
2. Simpler types to more complicated types.
3. Same types should be put together whenever possible.

#### Example

In the following declaration, type `User` is more complicated than type `string`, but `Repository` belongs to `User` (which makes the `user` argument more important), so `user` is more left than `repoName`:

```Go
func IsRepositoryExist(user *User, repoName string) (bool, error) { ...
```

## Coding guidelines

- Do not initialize structs with unnamed fields and do break fields into multiple lines:

  ```go
  func main() {
  	...
  	awsCloudwatchLogsLogGroup := os.Getenv("AWS_CLOUDWATCH_LOGS_LOG_GROUP")
  	if awsCloudwatchLogsLogGroup != "" {
  		client := awsutil.NewDefaultClient()
  		err := log.New(
  			"cloudwatchlogs",
  			awsutil.CloudWatchLogsIniter(),
  			1000,
  			awsutil.CloudWatchLogsConfig{ // <---- Focus here
  				Level:           log.LevelInfo,
  				Client:          client.NewCloudWatchLogsClient(),
  				LogGroupName:    awsCloudwatchLogsLogGroup,
  				LogStreamPrefix: awsCloudwatchLogsLogGroup + "-stream",
  				Retention:       30,
  				MaxRetries:      3,
  			},
  		)
  		if err != nil {
  			log.Fatal("Failed to init cloudwatchlogs logger: %v", err)
  		}
  	}
  	...
  }
  ```

- Group declaration should be organized by types, not all together:

  ```go
  const (
  	// Default section name.
  	DefaultSection = "DEFAULT"
  	// Maximum allowed depth when recursively substituing variable names.
  	depthValues = 200
  )
  
  type ParseError int
  
  const (
  	ErrSectionNotFound ParseError = iota + 1
  	ErrKeyNotFound
  	ErrBlankSectionName
  	ErrCouldNotParse
  )
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

## Testing

- Unit tests must use [github.com/stretchr/testify](https://github.com/stretchr/testify) and code coverage must above 80%.

### Examples

- The file of examples of helper modules should be named as `example_test.go`.
- Test cases of functions must start with `Test`, e.g. `TestLogger`.
- Test cases of methods must use format `Text<Struct>_<Method>`, e.g. `TestMacaron_Run`.

## Linting

**Warnings will be ignored in practice**, CI should either fail or pass on linting, anything that is not failing the CI should be removed.

Here is the best practice for configuring `.golangci.yml`:

```yml
linters-settings:
  nakedret:
    max-func-lines: 0 # Disallow any unnamed return statement

linters:
  enable:
    - deadcode
    - errcheck
    - gosimple
    - govet
    - ineffassign
    - staticcheck
    - structcheck
    - typecheck
    - unused
    - varcheck
    - nakedret
    - gofmt
    - rowserrcheck
    - unconvert
    - goimports
```

