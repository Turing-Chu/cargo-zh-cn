## 包 ID 格式

> [reference/pkgid-spec.md][pkgid-spec]
>
> [commit 1271bb4de0c0e0a085be239c2418af9c673ffc87][commit]

[pkgid-spec]: https://github.com/rust-lang/cargo/blob/master/src/doc/src/reference/pkgid-spec.md
[commit]: https://github.com/rust-lang/cargo/commit/1271bb4de0c0e0a085be239c2418af9c673ffc87

### 包 ID 格式

Subcommands of Cargo frequently need to refer to a particular package within a
dependency graph for various operations like updating, cleaning, building, etc.
To solve this problem, Cargo supports Package ID Specifications. A specification
is a string which is used to uniquely refer to one package within a graph of
packages.

#### 格式语法

包 Id 格式的正式语法为：

```notrust
pkgid := pkgname
       | [ proto "://" ] hostname-and-path [ "#" ( pkgname | semver ) ]
pkgname := name [ ":" semver ]

proto := "http" | "git" | ...
```

此处，大括号指明其内容是可选的。

#### 示例格式

这些都可以作为在 `crates.io` 上注册的包 `foo` 的 `1.2.3` 版本。

| pkgid                        | name  | version | url                    |
|:-----------------------------|:-----:|:-------:|:----------------------:|
| `foo`                        | `foo` | `*`     | `*`                    |
| `foo:1.2.3`                  | `foo` | `1.2.3` | `*`                    |
| `crates.io/foo`              | `foo` | `*`     | `*://crates.io/foo`    |
| `crates.io/foo#1.2.3`        | `foo` | `1.2.3` | `*://crates.io/foo`    |
| `crates.io/bar#foo:1.2.3`    | `foo` | `1.2.3` | `*://crates.io/bar`    |
| `http://crates.io/foo#1.2.3` | `foo` | `1.2.3` | `http://crates.io/foo` |

####  简短格式

The goal of this is to enable both succinct and exhaustive syntaxes for
referring to packages in a dependency graph. Ambiguous references may refer to
one or more packages. Most commands generate an error if more than one package
could be referred to with the same specification.

这样做的目的是为了在依赖图中引用包时保证简洁且详尽的语法。模凌两可的引用可能指向一个或多个包。如果同一格式指向了多个包，大多数命令都会产生错误。
