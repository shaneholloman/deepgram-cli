# deepgram-cli

This markdown document provides a comprehensive overview of the directory structure and file contents. It aims to give viewers (human or AI) a complete view of the codebase in a single file for easy analysis.

## Document Table of Contents

The table of contents below is for navigational convenience and reflects this document's structure, not the actual file structure of the repository.

- [deepgram-cli](#deepgram-cli)
  - [Document Table of Contents](#document-table-of-contents)
  - [Repo File Tree](#repo-file-tree)
  - [Repo File Contents](#repo-file-contents)
    - [.gitignore](#gitignore)
    - [.golangci.yaml](#golangciyaml)
    - [go.mod](#gomod)
    - [go.sum](#gosum)
    - [LICENSE](#license)
    - [main.go](#maingo)
    - [assets/config/template.deepgram.yaml](#assetsconfigtemplatedeepgramyaml)
    - [cmd/login.go](#cmdlogingo)
    - [cmd/root.go](#cmdrootgo)
    - [cmd/test.go](#cmdtestgo)
    - [internal/auth/device\_flow.go](#internalauthdevice_flowgo)
    - [internal/auth/guard.go](#internalauthguardgo)
    - [internal/auth/session.go](#internalauthsessiongo)
    - [internal/config/config\_init.go](#internalconfigconfig_initgo)
    - [internal/config/config\_write.go](#internalconfigconfig_writego)
    - [pkg/common/constants.go](#pkgcommonconstantsgo)
    - [pkg/common/prompts.go](#pkgcommonpromptsgo)

## Repo File Tree

This file tree represents the actual structure of the repository. It's crucial for understanding the organization of the codebase.

```tree
.
├── assets/
│   └── config/
│       └── template.deepgram.yaml
├── cmd/
│   ├── login.go
│   ├── root.go
│   └── test.go
├── internal/
│   ├── auth/
│   │   ├── device_flow.go
│   │   ├── guard.go
│   │   └── session.go
│   └── config/
│       ├── config_init.go
│       └── config_write.go
├── pkg/
│   └── common/
│       ├── constants.go
│       └── prompts.go
├── .gitignore
├── .golangci.yaml
├── LICENSE
├── go.mod
├── go.sum
└── main.go

8 directories, 17 files
```

## Repo File Contents

The following sections present the content of each file in the repository.

### .gitignore

```ini
# project specific
my-plugins/

# Binaries for programs and plugins
.DS_Store
*.exe
*.exe~
*.dll
*.o
*.a
*.so
*.dylib

# Folders
_obj
_test
*.test

# Output of the go coverage tool, specifically when used with LiteIDE
*.out

# Dependency directories (remove the comment below to include it)
# vendor/

# cgo stuff
*.cgo1.go
*.cgo2.c
_cgo_defun.c
_cgo_gotypes.go
_cgo_export.*
_testmain.go
.idea/
*.iml
```

### .golangci.yaml

```yml
issues:
  # List of regexps of issue texts to exclude, empty list by default.
  # But independently from this option we use default exclude patterns,
  # it can be disabled by `exclude-use-default: false`. To list all
  # excluded by default patterns execute `golangci-lint run --help`

  exclude-rules:
    # Exclude gosimple bool check
    - linters:
        - gosimple
      text: "S(1002|1008|1021)"
    # Exclude failing staticchecks for now
    - linters:
        - staticcheck
      text: "SA(1006|1019|4006|4010|4017|5007|6005|9004):"
    # Exclude lll issues for long lines with go:generate
    - linters:
        - lll
      source: "^//go:generate "

  # Maximum issues count per one linter. Set to 0 to disable. Default is 50.
  max-issues-per-linter: 0

  # Maximum count of issues with the same text. Set to 0 to disable. Default is 3.
  max-same-issues: 0

linters:
  disable-all: true
  enable:
    - gofmt
    - gosimple
    - govet
    - ineffassign
    - staticcheck
    - unconvert
    - unused
  fast: true

# options for analysis running
run:
  go: "1.22"

  # default concurrency is a available CPU number
  concurrency: 4

  # timeout for analysis, e.g. 30s, 5m, default is 1m
  timeout: 10m

  # exit code when at least one issue was found, default is 1
  issues-exit-code: 1

  # include test files or not, default is true
  tests: true

# output configuration options
output:
  # print lines of code with issue, default is true
  print-issued-lines: true

  # print linter name in the end of issue text, default is true
  print-linter-name: true

  # make issues output unique by line, default is true
  uniq-by-line: true

# all available settings of specific linters
linters-settings:
  gofumpt:
    module-path: github.com/deepgram/deepgram-cli
  errcheck:
    # report about not checking of errors in type assetions: `a := b.(MyStruct)`;
    # default is false: such cases aren't reported by default.
    check-type-assertions: false

    # report about assignment of errors to blank identifier: `num, _ := strconv.Atoi(numStr)`;
    # default is false: such cases aren't reported by default.
    check-blank: false

    # path to a file containing a list of functions to exclude from checking
    # see https://github.com/kisielk/errcheck#excluding-functions for details
    #exclude: /path/to/file.txt
```

### go.mod

```txt
module deepgram-cli

go 1.22.5

require (
 github.com/spf13/cobra v1.8.1
 github.com/spf13/viper v1.19.0
)

require (
 github.com/fsnotify/fsnotify v1.7.0 // indirect
 github.com/hashicorp/hcl v1.0.0 // indirect
 github.com/inconshreveable/mousetrap v1.1.0 // indirect
 github.com/magiconair/properties v1.8.7 // indirect
 github.com/mitchellh/mapstructure v1.5.0 // indirect
 github.com/pelletier/go-toml/v2 v2.2.2 // indirect
 github.com/pkg/errors v0.9.1
 github.com/sagikazarmark/locafero v0.4.0 // indirect
 github.com/sagikazarmark/slog-shim v0.1.0 // indirect
 github.com/sourcegraph/conc v0.3.0 // indirect
 github.com/spf13/afero v1.11.0 // indirect
 github.com/spf13/cast v1.6.0 // indirect
 github.com/spf13/pflag v1.0.5 // indirect
 github.com/subosito/gotenv v1.6.0 // indirect
 go.uber.org/atomic v1.9.0 // indirect
 go.uber.org/multierr v1.9.0 // indirect
 golang.org/x/exp v0.0.0-20230905200255-921286631fa9 // indirect
 golang.org/x/sys v0.18.0 // indirect
 golang.org/x/text v0.14.0 // indirect
 gopkg.in/ini.v1 v1.67.0 // indirect
 gopkg.in/yaml.v3 v3.0.1 // indirect
)
```

### go.sum

```txt
github.com/cpuguy83/go-md2man/v2 v2.0.4/go.mod h1:tgQtvFlXSQOSOSIRvRPT7W67SCa46tRHOmNcaadrF8o=
github.com/davecgh/go-spew v1.1.0/go.mod h1:J7Y8YcW2NihsgmVo/mv3lAwl/skON4iLHjSsI+c5H38=
github.com/davecgh/go-spew v1.1.1/go.mod h1:J7Y8YcW2NihsgmVo/mv3lAwl/skON4iLHjSsI+c5H38=
github.com/davecgh/go-spew v1.1.2-0.20180830191138-d8f796af33cc h1:U9qPSI2PIWSS1VwoXQT9A3Wy9MM3WgvqSxFWenqJduM=
github.com/davecgh/go-spew v1.1.2-0.20180830191138-d8f796af33cc/go.mod h1:J7Y8YcW2NihsgmVo/mv3lAwl/skON4iLHjSsI+c5H38=
github.com/frankban/quicktest v1.14.6 h1:7Xjx+VpznH+oBnejlPUj8oUpdxnVs4f8XU8WnHkI4W8=
github.com/frankban/quicktest v1.14.6/go.mod h1:4ptaffx2x8+WTWXmUCuVU6aPUX1/Mz7zb5vbUoiM6w0=
github.com/fsnotify/fsnotify v1.7.0 h1:8JEhPFa5W2WU7YfeZzPNqzMP6Lwt7L2715Ggo0nosvA=
github.com/fsnotify/fsnotify v1.7.0/go.mod h1:40Bi/Hjc2AVfZrqy+aj+yEI+/bRxZnMJyTJwOpGvigM=
github.com/google/go-cmp v0.5.9 h1:O2Tfq5qg4qc4AmwVlvv0oLiVAGB7enBSJ2x2DqQFi38=
github.com/google/go-cmp v0.5.9/go.mod h1:17dUlkBOakJ0+DkrSSNjCkIjxS6bF9zb3elmeNGIjoY=
github.com/hashicorp/hcl v1.0.0 h1:0Anlzjpi4vEasTeNFn2mLJgTSwt0+6sfsiTG8qcWGx4=
github.com/hashicorp/hcl v1.0.0/go.mod h1:E5yfLk+7swimpb2L/Alb/PJmXilQ/rhwaUYs4T20WEQ=
github.com/inconshreveable/mousetrap v1.1.0 h1:wN+x4NVGpMsO7ErUn/mUI3vEoE6Jt13X2s0bqwp9tc8=
github.com/inconshreveable/mousetrap v1.1.0/go.mod h1:vpF70FUmC8bwa3OWnCshd2FqLfsEA9PFc4w1p2J65bw=
github.com/kr/pretty v0.3.1 h1:flRD4NNwYAUpkphVc1HcthR4KEIFJ65n8Mw5qdRn3LE=
github.com/kr/pretty v0.3.1/go.mod h1:hoEshYVHaxMs3cyo3Yncou5ZscifuDolrwPKZanG3xk=
github.com/kr/text v0.2.0 h1:5Nx0Ya0ZqY2ygV366QzturHI13Jq95ApcVaJBhpS+AY=
github.com/kr/text v0.2.0/go.mod h1:eLer722TekiGuMkidMxC/pM04lWEeraHUUmBw8l2grE=
github.com/magiconair/properties v1.8.7 h1:IeQXZAiQcpL9mgcAe1Nu6cX9LLw6ExEHKjN0VQdvPDY=
github.com/magiconair/properties v1.8.7/go.mod h1:Dhd985XPs7jluiymwWYZ0G4Z61jb3vdS329zhj2hYo0=
github.com/mitchellh/mapstructure v1.5.0 h1:jeMsZIYE/09sWLaz43PL7Gy6RuMjD2eJVyuac5Z2hdY=
github.com/mitchellh/mapstructure v1.5.0/go.mod h1:bFUtVrKA4DC2yAKiSyO/QUcy7e+RRV2QTWOzhPopBRo=
github.com/pelletier/go-toml/v2 v2.2.2 h1:aYUidT7k73Pcl9nb2gScu7NSrKCSHIDE89b3+6Wq+LM=
github.com/pelletier/go-toml/v2 v2.2.2/go.mod h1:1t835xjRzz80PqgE6HHgN2JOsmgYu/h4qDAS4n929Rs=
github.com/pkg/errors v0.9.1 h1:FEBLx1zS214owpjy7qsBeixbURkuhQAwrK5UwLGTwt4=
github.com/pkg/errors v0.9.1/go.mod h1:bwawxfHBFNV+L2hUp1rHADufV3IMtnDRdf1r5NINEl0=
github.com/pmezard/go-difflib v1.0.0/go.mod h1:iKH77koFhYxTK1pcRnkKkqfTogsbg7gZNVY4sRDYZ/4=
github.com/pmezard/go-difflib v1.0.1-0.20181226105442-5d4384ee4fb2 h1:Jamvg5psRIccs7FGNTlIRMkT8wgtp5eCXdBlqhYGL6U=
github.com/pmezard/go-difflib v1.0.1-0.20181226105442-5d4384ee4fb2/go.mod h1:iKH77koFhYxTK1pcRnkKkqfTogsbg7gZNVY4sRDYZ/4=
github.com/rogpeppe/go-internal v1.9.0 h1:73kH8U+JUqXU8lRuOHeVHaa/SZPifC7BkcraZVejAe8=
github.com/rogpeppe/go-internal v1.9.0/go.mod h1:WtVeX8xhTBvf0smdhujwtBcq4Qrzq/fJaraNFVN+nFs=
github.com/russross/blackfriday/v2 v2.1.0/go.mod h1:+Rmxgy9KzJVeS9/2gXHxylqXiyQDYRxCVz55jmeOWTM=
github.com/sagikazarmark/locafero v0.4.0 h1:HApY1R9zGo4DBgr7dqsTH/JJxLTTsOt7u6keLGt6kNQ=
github.com/sagikazarmark/locafero v0.4.0/go.mod h1:Pe1W6UlPYUk/+wc/6KFhbORCfqzgYEpgQ3O5fPuL3H4=
github.com/sagikazarmark/slog-shim v0.1.0 h1:diDBnUNK9N/354PgrxMywXnAwEr1QZcOr6gto+ugjYE=
github.com/sagikazarmark/slog-shim v0.1.0/go.mod h1:SrcSrq8aKtyuqEI1uvTDTK1arOWRIczQRv+GVI1AkeQ=
github.com/sourcegraph/conc v0.3.0 h1:OQTbbt6P72L20UqAkXXuLOj79LfEanQ+YQFNpLA9ySo=
github.com/sourcegraph/conc v0.3.0/go.mod h1:Sdozi7LEKbFPqYX2/J+iBAM6HpqSLTASQIKqDmF7Mt0=
github.com/spf13/afero v1.11.0 h1:WJQKhtpdm3v2IzqG8VMqrr6Rf3UYpEF239Jy9wNepM8=
github.com/spf13/afero v1.11.0/go.mod h1:GH9Y3pIexgf1MTIWtNGyogA5MwRIDXGUr+hbWNoBjkY=
github.com/spf13/cast v1.6.0 h1:GEiTHELF+vaR5dhz3VqZfFSzZjYbgeKDpBxQVS4GYJ0=
github.com/spf13/cast v1.6.0/go.mod h1:ancEpBxwJDODSW/UG4rDrAqiKolqNNh2DX3mk86cAdo=
github.com/spf13/cobra v1.8.1 h1:e5/vxKd/rZsfSJMUX1agtjeTDf+qv1/JdBF8gg5k9ZM=
github.com/spf13/cobra v1.8.1/go.mod h1:wHxEcudfqmLYa8iTfL+OuZPbBZkmvliBWKIezN3kD9Y=
github.com/spf13/pflag v1.0.5 h1:iy+VFUOCP1a+8yFto/drg2CJ5u0yRoB7fZw3DKv/JXA=
github.com/spf13/pflag v1.0.5/go.mod h1:McXfInJRrz4CZXVZOBLb0bTZqETkiAhM9Iw0y3An2Bg=
github.com/spf13/viper v1.19.0 h1:RWq5SEjt8o25SROyN3z2OrDB9l7RPd3lwTWU8EcEdcI=
github.com/spf13/viper v1.19.0/go.mod h1:GQUN9bilAbhU/jgc1bKs99f/suXKeUMct8Adx5+Ntkg=
github.com/stretchr/objx v0.1.0/go.mod h1:HFkY916IF+rwdDfMAkV7OtwuqBVzrE8GR6GFx+wExME=
github.com/stretchr/objx v0.4.0/go.mod h1:YvHI0jy2hoMjB+UWwv71VJQ9isScKT/TqJzVSSt89Yw=
github.com/stretchr/objx v0.5.0/go.mod h1:Yh+to48EsGEfYuaHDzXPcE3xhTkx73EhmCGUpEOglKo=
github.com/stretchr/objx v0.5.2/go.mod h1:FRsXN1f5AsAjCGJKqEizvkpNtU+EGNCLh3NxZ/8L+MA=
github.com/stretchr/testify v1.3.0/go.mod h1:M5WIy9Dh21IEIfnGCwXGc5bZfKNJtfHm1UVUgZn+9EI=
github.com/stretchr/testify v1.7.1/go.mod h1:6Fq8oRcR53rry900zMqJjRRixrwX3KX962/h/Wwjteg=
github.com/stretchr/testify v1.8.0/go.mod h1:yNjHg4UonilssWZ8iaSj1OCr/vHnekPRkoO+kdMU+MU=
github.com/stretchr/testify v1.8.4/go.mod h1:sz/lmYIOXD/1dqDmKjjqLyZ2RngseejIcXlSw2iwfAo=
github.com/stretchr/testify v1.9.0 h1:HtqpIVDClZ4nwg75+f6Lvsy/wHu+3BoSGCbBAcpTsTg=
github.com/stretchr/testify v1.9.0/go.mod h1:r2ic/lqez/lEtzL7wO/rwa5dbSLXVDPFyf8C91i36aY=
github.com/subosito/gotenv v1.6.0 h1:9NlTDc1FTs4qu0DDq7AEtTPNw6SVm7uBMsUCUjABIf8=
github.com/subosito/gotenv v1.6.0/go.mod h1:Dk4QP5c2W3ibzajGcXpNraDfq2IrhjMIvMSWPKKo0FU=
go.uber.org/atomic v1.9.0 h1:ECmE8Bn/WFTYwEW/bpKD3M8VtR/zQVbavAoalC1PYyE=
go.uber.org/atomic v1.9.0/go.mod h1:fEN4uk6kAWBTFdckzkM89CLk9XfWZrxpCo0nPH17wJc=
go.uber.org/multierr v1.9.0 h1:7fIwc/ZtS0q++VgcfqFDxSBZVv/Xo49/SYnDFupUwlI=
go.uber.org/multierr v1.9.0/go.mod h1:X2jQV1h+kxSjClGpnseKVIxpmcjrj7MNnI0bnlfKTVQ=
golang.org/x/exp v0.0.0-20230905200255-921286631fa9 h1:GoHiUyI/Tp2nVkLI2mCxVkOjsbSXD66ic0XW0js0R9g=
golang.org/x/exp v0.0.0-20230905200255-921286631fa9/go.mod h1:S2oDrQGGwySpoQPVqRShND87VCbxmc6bL1Yd2oYrm6k=
golang.org/x/sys v0.18.0 h1:DBdB3niSjOA/O0blCZBqDefyWNYveAYMNF1Wum0DYQ4=
golang.org/x/sys v0.18.0/go.mod h1:/VUhepiaJMQUp4+oa/7Zr1D23ma6VTLIYjOOTFZPUcA=
golang.org/x/text v0.14.0 h1:ScX5w1eTa3QqT8oi6+ziP7dTV1S2+ALU0bI+0zXKWiQ=
golang.org/x/text v0.14.0/go.mod h1:18ZOQIKpY8NJVqYksKHtTdi31H5itFRjB5/qKTNYzSU=
gopkg.in/check.v1 v0.0.0-20161208181325-20d25e280405/go.mod h1:Co6ibVJAznAaIkqp8huTwlJQCZ016jof/cbN4VW5Yz0=
gopkg.in/check.v1 v1.0.0-20190902080502-41f04d3bba15 h1:YR8cESwS4TdDjEe65xsg0ogRM/Nc3DYOhEAlW+xobZo=
gopkg.in/check.v1 v1.0.0-20190902080502-41f04d3bba15/go.mod h1:Co6ibVJAznAaIkqp8huTwlJQCZ016jof/cbN4VW5Yz0=
gopkg.in/ini.v1 v1.67.0 h1:Dgnx+6+nfE+IfzjUEISNeydPJh9AXNNsWbGP9KzCsOA=
gopkg.in/ini.v1 v1.67.0/go.mod h1:pNLf8WUiyNEtQjuu5G5vTm06TEv9tsIgeAvK8hOrP4k=
gopkg.in/yaml.v3 v3.0.0-20200313102051-9f266ea9e77c/go.mod h1:K4uyk7z7BCEPqu6E+C64Yfv1cQ7kz7rIZviUmN+EgEM=
gopkg.in/yaml.v3 v3.0.1 h1:fxVm/GzAzEWqLHuvctI91KS9hhNmmWOoWu0XTYJS7CA=
gopkg.in/yaml.v3 v3.0.1/go.mod h1:K4uyk7z7BCEPqu6E+C64Yfv1cQ7kz7rIZviUmN+EgEM=
```

### LICENSE

```txt
The MIT License (MIT)

Copyright Â© 2024 Deepgram

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
```

### main.go

```go
/*
Copyright Â© 2024 Deepgram

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
*/
package main

import "deepgram-cli/cmd"

func main() {
 cmd.Execute()
}
```

### assets/config/template.deepgram.yaml

```yml
api_key: "%DEEPGRAM_API_KEY%"
```

### cmd/login.go

```go
/*
Copyright Â© 2024 Deepgram

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
*/
package cmd

import (
 "fmt"
 "os"

 "github.com/spf13/cobra"
 "github.com/spf13/viper"

 "deepgram-cli/internal/auth"
 "deepgram-cli/internal/config"
 "deepgram-cli/pkg/common"
)

// loginCmd represents the login command
var loginCmd = &cobra.Command{
 Use:   "login",
 Short: "Log in a user",
 Long:  `Logs a user into Deepgram with browser-based authentication.`,
 Run:   runLogin,
}

func init() {
 rootCmd.AddCommand(loginCmd)
 // Allow users to provide an API key directly during login.
 // This also removes the global scope api_key flag from the --help menu
 // to reduce confusion and provide a different description in this context.
 // This isn't bound to the loaded config "api_key" key, so we can tell if a
 // new key is being provided.
 loginCmd.Flags().StringVarP(&ApiKey, "api_key", "k", "", "Configure the CLI with your Deepgram API key")

 // allow users to force write and skip the prompt
 loginCmd.Flags().BoolP("force-write", "f", false, "Don't prompt for confirmation when providing an API key")
}

func runLogin(cmd *cobra.Command, args []string) {
 var (
  // Get the API key from the config context
  key string = cmd.Flags().Lookup("api_key").Value.String()

  str string
  err error
 )

 switch {
 case key != "":
  err = cliAuth()
 default:
  err = webAuth()
 }

 fmt.Print(str, err)
}

func cliAuth() error {
 if !viper.GetBool("force-write") {
  cobra.CheckErr(common.PromptBool("Do you want to write this key to config?"))

  if config.ConfigFileExists() {
   cobra.CheckErr(common.PromptBool("Configuration file already exists. Overwrite?"))
  }
 }

 return config.WriteConfigFile()
}

func webAuth() error {
 var (
  key string = viper.GetString("api_key")

  newKey string
  err    error
 )

 if key != "" {
  cobra.CheckErr(common.PromptBool("You're already logged in. Do you want to login again?"))
 }

 hostname, err := os.Hostname()

 cobra.CheckErr(err)

 fmt.Printf("Logging in to %s/n", hostname)

 session, err := auth.StartSession(hostname)
 // start session
 // open browser
 // wait for response

 fmt.Println(session)

 if err != nil {
  return err
 }

 viper.Set("api_key", newKey)
 newKey = "123456"

 return config.WriteConfigFile()
}
```

### cmd/root.go

```go
/*
Copyright Â© 2024 Deepgram

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
*/
package cmd

import (
 "os"

 "github.com/spf13/cobra"
 "github.com/spf13/viper"

 "deepgram-cli/internal/config"
)

var ApiKey string

var rootCmd = &cobra.Command{
 Use:               "deepgram",
 Short:             "The Deepgram command line interface.",
 Long:              `This is the Deepgram command line interface.`,
 PersistentPreRunE: config.ConfigInit,
}

func Execute() {
 err := rootCmd.Execute()
 if err != nil {
  os.Exit(1)
 }
}

func init() {
 rootCmd.PersistentFlags().StringVarP(&ApiKey, "api_key", "k", "", "Run the CLI with your Deepgram API key")
 viper.BindPFlag("api_key", rootCmd.PersistentFlags().Lookup("api_key"))
}
```

### cmd/test.go

```go
/*
Copyright Â© 2024 Deepgram

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
*/
package cmd

import (
 "fmt"

 "github.com/spf13/cobra"

 "deepgram-cli/internal/auth"
)

// testCmd represents the test command
var testCmd = &cobra.Command{
 Use:    "test",
 Short:  "test",
 Long:   `test.`,
 Run:    runTest,
 PreRun: auth.Guard,
}

func init() {
 rootCmd.AddCommand(testCmd)
}

func runTest(cmd *cobra.Command, args []string) {
 fmt.Print("test", cmd, args)
}
```

### internal/auth/device_flow.go

```go
package auth

import (
 "github.com/spf13/cobra"
)

func DeviceFlow(cmd *cobra.Command, args []string) {
 //  if apiKey == "" {
 //   fmt.Fprintln(os.Stderr, `DEEPGRAM_API_KEY is not set in the configuration file `+constants.DefaultConfigText+` or environment variable.

 // Run "deepgram login" to configure your API key.
 //   `)

 //   log.Fatal(`DEEPGRsAM_API_KEY is not set in the configuration file or environment variable.`)
 //  }
}
```

### internal/auth/guard.go

```go
package auth

import (
 "fmt"
 "log"
 "os"

 "github.com/spf13/cobra"
 "github.com/spf13/viper"

 "deepgram-cli/pkg/common"
)

func Guard(cmd *cobra.Command, args []string) {
 apiKey := viper.GetString("api_key")

 if apiKey == "" {
  fmt.Fprintln(os.Stderr, `DEEPGRAM_API_KEY is not set in the configuration file `+common.DefaultConfigText+` or environment variable.

Run "deepgram login" to configure your API key.
   `)

  log.Fatal(`DEEPGRsAM_API_KEY is not set in the configuration file or environment variable.`)
 }
}
```

### internal/auth/session.go

```go
package auth

import "fmt"

type Session struct {
 Hostname string
}

func StartSession(hostname string) (Session, error) {
 var (
  result Session
  args   []string
 )

 args = append(args, hostname)

 fmt.Println(args, hostname)

 return result, nil
}
```

### internal/config/config_init.go

```go
package config

import (
 "os"
 "path/filepath"

 "github.com/spf13/cobra"
 "github.com/spf13/viper"

 "deepgram-cli/pkg/common"
)

func ConfigInit(cmd *cobra.Command, args []string) error {
 viper.SetConfigName(common.ConfigFileName)
 viper.SetConfigType(common.ConfigFileType)

 if dirname, err := os.UserHomeDir(); err == nil {
  viper.AddConfigPath(filepath.Join(dirname, common.ConfigHomeSubdir))
  viper.AddConfigPath(filepath.Join(dirname, common.ConfigHomeSubdir, common.ConfigDeepgramSubdir))
 }

 viper.SetEnvPrefix("deepgram")
 viper.AutomaticEnv()

 if err := viper.ReadInConfig(); err != nil {
  if _, ok := err.(viper.ConfigFileNotFoundError); !ok {
   return err
  }
 }

 return nil
}
```

### internal/config/config_write.go

```go
package config

import (
 "fmt"
 "os"
 "path/filepath"

 "github.com/pkg/errors"
 "github.com/spf13/viper"

 "deepgram-cli/pkg/common"
)

func ConfigFileExists() bool {
 return viper.ConfigFileUsed() != ""
}

func WriteConfigFile() error {
 configFile := viper.ConfigFileUsed()

 if configFile == "" {
  fullname := common.ConfigFileName + "." + common.ConfigFileType

  if dirname, err := os.UserHomeDir(); err == nil {
   configFile = filepath.Join(dirname, common.ConfigHomeSubdir, common.ConfigDeepgramSubdir, fullname)
  }

  if configFile == "" {
   return errors.New("Failed to acquire config directory name")
  }

  configDirPath := filepath.Dir(configFile)

  if err := os.MkdirAll(configDirPath, os.ModePerm); err != nil {
   return err
  }

  fmt.Printf("Deepgram configuration file created at %s/n", configFile)
 }

 if err := viper.WriteConfigAs(configFile); err != nil {
  return err
 }

 return nil
}
```

### pkg/common/constants.go

```go
package common

// text consts
const (
 DefaultConfigText = "(default is $HOME/.config/Deepgram/config.yaml)"
)

// text consts
const (
 DeviceFlowUrl = "https://community.deepgram.com/api/auth/cli/device"
)

// config consts
const (
 ConfigFileName       = "config"
 ConfigFileType       = "yaml"
 ConfigHomeSubdir     = ".config"
 ConfigDeepgramSubdir = "deepgram"
)
```

### pkg/common/prompts.go

```go
package common

import (
 "bufio"
 "errors"
 "fmt"
 "os"
 "strings"
)

func PromptBool(message string) error {
 reader := bufio.NewReader(os.Stdin)
 fmt.Print(message + " (y/N): ")

 var err error

 input, err := reader.ReadString('/n')
 if err != nil {
  return errors.New("Error reading file - " + err.Error())
 }

 // Remove the newline character and convert to lower case
 input = strings.TrimSpace(input)
 input = strings.ToLower(input)

 if input != "y" && input != "yes" {
  os.Exit(0)
 }

 return nil
}
```

> This concludes the repository's file contents. Please review thoroughly for a comprehensive understanding of the codebase.
