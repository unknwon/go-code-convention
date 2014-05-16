# 版权声明

作为开源项目，必须有相应的开源许可证才能算是真正的开源。在选择了一个开源许可证之后，需要在源文件中进行相应的版权声明才能生效。以下分别以 [Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0) 和 MIT 授权许可为例。

### Apache License, Version 2.0

该许可证要求在所有的源文件中的头部放置以下内容才能算协议对该文件有效：

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

其中，`[yyyy]` 表示该源文件创建的年份。紧随其后的是 `[name of copyright owner]`，即版权所有者。如果为个人项目，就写个人名称；若为团队项目，则宜写团队名称。

### MIT License

一般使用 MIT 授权的项目，需在源文件头部增加以下内容：

```
// Copyright [yyyy] [name of copyright owner]. All rights reserved.
// Use of this source code is governed by a MIT-style
// license that can be found in the LICENSE file.
```

其中，年份和版权所有者的名称填写规则与 Apache License, Version 2.0 的一样。

### 其它说明

- 其它类型的开源许可证基本上都可参照以上两种方案。
- 如果存在不同作者或组织对同个源文件的修改，在协议兼容的情况下，可将首行变为多行，按照先后次序排放：

	```
// Copyright 2011 Gary Burd
// Copyright 2013 Unknown
	```