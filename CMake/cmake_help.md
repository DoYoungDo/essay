# CMake查看帮助

- 查看总体的帮助

```bash
$ cmake --help
```

- 查看所有命令

```bash
$ cmake --help-command-list
```

- 查看某一条命令的详细信息

```bash
$ cmake --help-command ${cmd}
```

- 查看所有内置变量

```bash
$ cmake --help-variable-list
```

- 查看某一条变量的详细信息

```bash
$ cmake --help-variable ${var}
```

> 可以同时配合`grep`过滤出想查找的信息

```bash
$ cmake --help-command-list | grep add_

$ cmake --help-variable-list | grep PROJECT
```