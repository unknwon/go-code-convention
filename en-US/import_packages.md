# Import Packages

All package import paths must be a URL from a code hosting site except packages from standard library.

Therefore, there are four types of packages a source file could import:

1. Packages from standard library
2. Third-party packages
3. Packages in same organization but not in same repository
4. Packages in same repository

Basic rules:

- Use different groups to separate import paths by an empty line for two or more types of packages.
- Must not use `.` to simplify import path except in test files (`*_test.go`).
- Must not use relative path (`./subpackage`) as import path, all import paths must be `go get`-able.

Here is a complete example:

```go
import (
    "fmt"
    "html/template"
    "net/http"
    "os"

    "github.com/codegangsta/cli"
    "gopkg.in/macaron.v1"

    "github.com/gogs/git"
    "github.com/gogs/gfm"

    "github.com/gogs/gogs/routes"
    "github.com/gogs/gogs/routes/repo"
    "github.com/gogs/gogs/routes/user"
)
```
