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

TBD
