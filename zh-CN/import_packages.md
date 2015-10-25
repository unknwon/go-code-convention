# 导入标准库、第三方或其它包

除标准库外，Go 语言的导入路径基本上依赖代码托管平台上的 URL 路径，因此一个源文件需要导入的包有 4 种分类：标准库、第三方包、组织内其它包和当前包的子包。

基本规则：

- 如果同时存在 2 种及以上，则需要使用分区来导入。每个分类使用一个分区，采用空行作为分区之间的分割。
- 在非测试文件（`*_test.go`）中，禁止使用 `.` 来简化导入包的对象调用。
- 禁止使用相对路径导入（`./subpackage`），所有导入路径必须符合 `go get` 标准。

下面是一个完整的示例：

```Go
import (
	"fmt"
	"html/template"
	"net/http"
	"os"

	"github.com/codegangsta/cli"
	"gopkg.in/macaron.v1"

	"github.com/gogits/git"
	"github.com/gogits/gfm"

	"github.com/gogits/gogs/routers"
	"github.com/gogits/gogs/routers/repo"
	"github.com/gogits/gogs/routers/user"
)
```
