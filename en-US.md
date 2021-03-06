# Go Code Convention

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


### Function and method

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

  