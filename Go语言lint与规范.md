# Go语言lint与规范

关于lint的描述

1. 如果使用golangci-lint的话，目前只能使用<v1.48.0的版本，从v1.48.0以上，都需要go1.19
2. golangci-lint集成了 go-vet, staticcheck, revive 这3种meta-linter，这3种可以单独使用，其中 go-vet 为官方自带，staticcheck在使用vscode时强制使用。
3. 当前内网使用版本为v1.44.2

# Linter

| 名称 | 用途 | 地址 | 类型 | 其它说明 | 预设值 |
| --- | --- | --- | --- | --- | --- |
| typecheck | 检测类型定义与使用 | 默认自带 | bug检测 |  |  |
| asciicheck | 代码中除注释外是否包含非ASCII字符。 | https://github.com/tdakkota/asciicheck | bug检测，style检测 |  |  |
| bodyclose | 检测http调用的response body是否正常关闭。 | https://github.com/timakin/bodyclose | bug检测，style检测 | 暂未支持泛型，在.golangci-lint.yml配置中暂时去掉 |  |
| cyclop | 检测代码中是否存在循环引用 | https://github.com/bkielbasa/cyclop | 复杂度检测 |  |  |
| deadcode | 检测是否有未使用的声明 | https://github.com/remyoudompheng/go-misc/tree/master/deadcode | v1.49.0里会被弃用 | 代码长期未更新，可用unused替代 |  |
| depguard | 检测被引用的package是否被允许 | https://github.com/OpenPeeDeeP/depguard |  |  |  |
| dogsled | 检测函数/方法的输出值是否有过多的被丢弃 | https://github.com/alexkohler/dogsled | style检测 |  |  |
| dupl | 检测代码中是否有拷贝 | https://github.com/mibk/dupl | style检测 |  |  |
| durationcheck | 防止时间类型进行进行乘法 | https://github.com/charithe/durationcheck | bug检测 |  |  |
| errcheck | 检测代码中是否有错误没有被检查 | https://github.com/kisielk/errcheck | bug检测，error检测 |  |  |
| errorlint | 防止Go1.13引入的error Wrapping策略被误用。 | https://github.com/polyfloyd/go-errorlint | bug检测，error检测 |  |  |
| exhaustive | 1. 检测switch语句中的enum是否被完全处理。检测map结构中当key为enum时，是否被完全处理。 | https://github.com/nishanths/exhaustive | bug检测 |  |  |
| exportloopref | 检测for循环产生的局部变量是否被以指针的形式对外暴露。 | https://github.com/kyoh86/exportloopref | bug检测 | 存在false negative的情况，如果出现，可酌情判断。 |  |
| forbidigo | 防止代码中使用某些标识符，如fmt.Println | https://github.com/ashanbrown/forbidigo | style检测 |  |  |
| funlen | 检测代码中函数是否过于冗长和复杂 | https://github.com/ultraware/funlen | 复杂度检测 |  | 默认为60行，400个声明。 |
| gochecknoinits | 防止代码中存在init函数 | https://github.com/leighmcculloch/gochecknoinits | style检测 |  |  |
| gocognit | 计算并检查函数的cognitive complexity | https://github.com/uudashr/gocognit | 复杂度检测 | CognitiveComplexity.pdf (sonarsource.com) | CognitiveComplexity.pdf (sonarsource.com) |
| goconst | 检查代码中是否有重复使用的字符串没有用const定义。 | https://github.com/jgautheron/goconst | style检测 |  |  |
| gocyclo | 计算并检查函数的cyclomatic complexity | https://github.com/fzipp/gocyclo | 复杂度检测 |  |  |
| godot | 检查注释是否以.结尾 | https://github.com/tetafro/godot | style检测，注释检测 |  |  |
| godox |  |  |  |  |  |
| gofmt | 格式化代码 | https://pkg.go.dev/cmd/gofmt | format工具 |  |  |
| gofumpt | 增强版gofmt | https://github.com/mvdan/gofumpt | format工具 |  |  |
| goheader | 检测文件的copyright |  |  |  |  |
| goimports | 格式化import的顺序 | https://pkg.go.dev/golang.org/x/tools/cmd/goimports | format工具 |  |  |
| gomoddirectives | go.mod文件检测 | https://github.com/ldez/gomoddirectives | style检测 |  |  |
| gomodguard | 类似depguard，表明哪些gomod被允许或者block。 | https://github.com/ryancurrah/gomodguard | style检测 |  |  |
| goprintffuncname | 检测printf-like函数命名是否以f结尾 | https://github.com/jirfag/go-printf-func-name | style检测 |  |  |
| gosec | 对代码进行安全性检测 | https://github.com/securego/gosec | bug检测 |  |  |
| gosimple | 检测代码是否可以被简化 | https://github.com/dominikh/go-tools/tree/master/simple | style检测 |  |  |
| govet | 检测代码是否规范、是否有常见的bug | https://pkg.go.dev/cmd/vet | bug检测 |  |  |
| ifshort | 检测if语句是否可以使用较短的声明 | https://github.com/esimonov/ifshort | style检测 |  |  |
| importas | 保证alias的一致性 | https://github.com/julz/importas | style检测 |  |  |
| ineffassign | 检测无效的赋值 | https://github.com/gordonklaus/ineffassign | unused检测 |  |  |
| lll | 检测过长的行数 |  |  |  |  |
| makezero | 检测被append的slice初始化长度不为0 | https://github.com/ashanbrown/makezero | style检测，bug检测 |  |  |
| misspell | 检测错误的英文拼写 | https://github.com/client9/misspell | style检测，comment检测 |  |  |
| nakedret | 当函数体长度达到阈值（默认为5）时，防止裸return | https://github.com/alexkohler/nakedret | style检测 |  |  |
| nestif | 防止if嵌套程度过深 | https://github.com/nakabonne/nestif | complexity检测 |  |  |
| nilerr | 防止err不为空，但返回却为空 | https://github.com/gostaticanalysis/nilerr | bug检测 | 暂未支持泛型，在.golangci-lint.yml配置中暂时去掉 |  |
| nlreturn | 强制非简单语句的return之前加空行 | https://github.com/ssgreg/nlreturn | style检测 |  |  |
| noctx | 强制发送http请求时增加ctx参数 | https://github.com/sonatard/noctx | 性能检测，bug检测 | 暂未支持泛型，在.golangci-lint.yml配置中暂时去掉 |  |
| nolintlint |  强制golangci-lint的nolint提示明确表达。 | https://github.com/golangci/golangci-lint/blob/master/pkg/golinters/nolintlint/README.md | style检测 |  |  |
| paralleltest | 防止t.Parallel()方法被错误使用 | https://github.com/kunwardeep/paralleltest | style检测，test检测 |  |  |
| prealloc | 提示可以被提前分配空间的slice | https://github.com/alexkohler/prealloc | 性能检测 |  |  |
| predeclared | 防止预定义的关键字被覆盖 | https://github.com/nishanths/predeclared | 性能检测 |  |  |
| promlinter | 检查prometheus的metric name是否正确 | https://github.com/yeya24/promlinter | style检测 |  |  |
| revive | golint的替代 | https://github.com/mgechev/revive | style检测，metalinter |  |  |
| rowserrcheck | 强制检测sql.Rows.Error | https://github.com/jingyugao/rowserrcheck | bug检测，sql检测 | 暂未支持泛型，在.golangci-lint.yml配置中暂时去掉 |  |
| sqlclosecheck | preparedStatement和rows使用后被正确关闭 | https://github.com/ryanrolds/sqlclosecheck | bug检测，sql检测 | 暂未支持泛型，在.golangci-lint.yml配置中暂时去掉 |  |
| staticcheck | 通用的linter，确保false postive的情况最少出现。 | https://staticcheck.io | bug检测，metalinter | 使用vscode编辑时会被强制使用。 |  |
| structcheck | 已经不被维护，将在v1.49.0中被移除，将会被 unused 替代。 |  |  |  |  |
| stylecheck | 功能与golint, revive类似 | https://github.com/dominikh/go-tools/blob/master/stylecheck/doc.go | style检测 |  |  |
| thelper | 确保test代码用到的helper函数中都有t.Helper()，确保*testing.T的receiver名字都为t. | https://github.com/kulti/thelper | style检测 |  |  |
| tparallel | 确保t.Parallel()被正确使用. | https://github.com/moricho/tparallel | style检测，test检测 | 暂未支持泛型，在.golangci-lint.yml配置中暂时去掉 |  |
| unconvert | 确保无用类型转换被移除 | https://github.com/mdempsky/unconvert | style检测 |  |  |
| unparam | 检测未使用的函数参数 | https://github.com/mvdan/unparam | unused检测 | 暂未支持泛型，在.golangci-lint.yml配置中暂时去掉 |  |
| unused | 检测未使用的常量、变量、函数和类型 | https://github.com/dominikh/go-tools/tree/master/unused | unused检测 |  |  |
| varcheck | 作者不维护，可以被unused替代 |  |  |  |  |
| wastedassign | 报告无用赋值 | https://github.com/sanposhiho/wastedassign | style检测 | 暂未支持泛型，在.golangci-lint.yml配置中暂时去掉 |  |
| whitespace | 检查无用的空行 | https://github.com/ultraware/whitespace |  |  |  |
| bidichk | 防止某些危险的unicode字符在源代码中出现 | https://github.com/breml/bidichk | bug检测 | research!rsc: On “Trojan Source” Attacks (swtch.com) |  |
| golint | v1.41.0之后会被弃用 |  |  |  |  |
| execinquery | 检测db.Query()的错误用法 | https://github.com/1uf3/execinquery | sql检测 |  |  |
| nosprintfhostport | 检测在构造网络请求地址的使用，强制使用net.JoinHostPort | https://github.com/stbenjam/no-sprintf-host-port | style检测 |  |  |
| grouper | 检测const,var,type,import等是否以group的方式进行处理 | https://github.com/leonklingele/grouper | style检测 |  |  |
| decorder | 强制const,var,type,func,init函数的声明顺序和函数 | https://gitlab.com/bosi/decorder | format检测，style检测 |  |  |
| errchkjson | 检测json的error是否需要被处理 | https://github.com/breml/errchkjson | bug检测 | 建议不用，首先容易与其它linter冲突，此外如果此linter有bug，可能会造成比较严重的影响 |  |
| maintidx | 衡量函数的可维护性，数值越大表明可维护性越好。 | https://github.com/yagipy/maintidx | 复杂度检测 |  |  |

## go-vet详细检测

可通过 `go tool vet help [command]` 来查看说明和示例。 

| 检测项 | 原文 | 译文 | 说明 |
| --- | --- | --- | --- |
| asmdecl | report mismatches between assembly files and Go declarations | 检测汇编代码和Go声明的不一致性 |  |
| assign | check for useless assignments | 检测没有用到的赋值 |  |
| atomic | check for common mistakes using the sync/atomic package | 检测使用sync/atomic包的常见错误。 |  |
| bools | check for common mistakes involving boolean operators | 检测涉及布尔判断的常见错误。 |  |
| buildtag | check that +build tags are well-formed and correctly located | 检测 +build 标签被正确使用。 |  |
| cgocall | detect some violations of the cgo pointer passing rules | 检测使用cgo指针传值的常见错误。 |  |
| composites | check for unkeyed composite literals | 检测组合字面量没有指定key的问题。 |  |
| copylocks | check for locks erroneously passed by value | 检测锁相关的结构被pass by value的问题 |  |
| httpresponse | check for mistakes using HTTP responses | 检测使用http response的错误。| `resp, err := http.Head(url) defer resp.Body.Close() if err != nil { log.Fatal(err) } // (defer statement belongs here)` | https://github.com/dominikh/go-tools/issues/149 |
| loopclosure | check references to loop variables from within nested functions | 检测for循环中内嵌的函数是否引用了循环变量。 |  |
| lostcancel | check cancel func returned by context.WithCancel is called | 检测context.WithCancel返回的cancel函数是否被调用。 |  |
| nilfunc | check for useless comparisons between functions and nil | 检测函数和nil之间无效的比较判断。 |  |
| printf | check consistency of Printf format strings and arguments | 检测Printf函数字符串模板和参数的一致性。 |  |
| shift | check for shifts that equal or exceed the width of the integer | 检测移位后是否超出了整形变量所能表示的范围。 |  |
| stdmethods | check signature of methods of well-known interfaces | 检测常用interface的方法签名 | 包括如下这些方法 `Format GobEncode GobDecode MarshalJSON MarshalXML Peek ReadByte ReadFrom ReadRune Scan Seek UnmarshalJSON UnreadByte UnreadRune WriteByte WriteTo` |
| structtag | check that struct field tags conform to reflect.StructTag.Get | 检测结构体字段的tag符合reflect.StructTag.Get的标准。 |  |
| tests | check for common mistaken usages of tests and examples | 检测Test、Benchmark和Example函数中的常见问题。 |  |
| unmarshal | report passing non-pointer or non-interface values to unmarshal | 检测传递非指针或非接口类型的值到unmarshal方法中。 |  |
| unreachable | check for unreachable code | 检测无法达到的代码。 |  |
| unsafeptr | check for invalid conversions of uintptr to unsafe.Pointer | 检测uintptr到unsafe.Pointer的无效转换。 |  |
| unusedresult | check for unused results of calls to some functions | 检测未被使用的函数返回值。 |  |

## staticcheck详细检测

使用vscode写go相关代码时，会被强制引用

来源：[Checks | Staticcheck](https://staticcheck.io/docs/checks/)

- [ ]  补充表格