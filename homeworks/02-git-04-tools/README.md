1. Найдите полный хеш и комментарий коммита, хеш которого начинается на `aefea`.

aefead2207ef7e2aa5dc81a34aedf0cad4c32545,Update CHANGELOG.md
```
artem@serv1:~/terraform$ git show -s --format=%H,%s aefea
aefead2207ef7e2aa5dc81a34aedf0cad4c32545,Update CHANGELOG.md
```
2. Какому тегу соответствует коммит `85024d3`?

v0.12.23
```
artem@serv1:~/terraform$ git show -s --oneline 85024d3
85024d3100 (tag: v0.12.23) v0.12.23
```
3. Сколько родителей у коммита `b8d720`? Напишите их хеши.

2 родителя: 56cd7859e05c36c06b56d013b55a252d0bb7e158 9ea88f22fc6269854151c571162c5bcf958bee2b

```
artem@serv1:~/terraform$ git show -s --format=%P b8d720
56cd7859e05c36c06b56d013b55a252d0bb7e158 9ea88f22fc6269854151c571162c5bcf958bee2b
```
4. Перечислите хеши и комментарии всех коммитов которые были сделаны между тегами v0.12.23 и v0.12.24.

- b14b74c493 [Website] vmc provider links
- 3f235065b9 Update CHANGELOG.md
- 6ae64e247b registry: Fix panic when server is unreachable
- 5c619ca1ba website: Remove links to the getting started guide's old location
- 06275647e2 Update CHANGELOG.md
- d5f9411f51 command: Fix bug when using terraform login on Windows
- 4b6d06cc5d Update CHANGELOG.md
- dd01a35078 Update CHANGELOG.md
- 225466bc3e Cleanup after v0.12.23 release

```
artem@serv1:~/terraform$ git log v0.12.23..v0.12.24 --oneline
33ff1c03bb (tag: v0.12.24) v0.12.24
b14b74c493 [Website] vmc provider links
3f235065b9 Update CHANGELOG.md
6ae64e247b registry: Fix panic when server is unreachable
5c619ca1ba website: Remove links to the getting started guide's old location
06275647e2 Update CHANGELOG.md
d5f9411f51 command: Fix bug when using terraform login on Windows
4b6d06cc5d Update CHANGELOG.md
dd01a35078 Update CHANGELOG.md
225466bc3e Cleanup after v0.12.23 release

```
5. Найдите коммит в котором была создана функция `func providerSource`, ее определение в коде выглядит
так `func providerSource(...)` (вместо троеточего перечислены аргументы).

8c928e83589d90a031f811fae52a81be7153e82f
```
artem@serv1:~/terraform$ git log -S'func providerSource' --oneline
5af1e6234a main: Honor explicit provider_installation CLI config when present
8c928e8358 main: Consult local directories as potential mirrors of providers
artem@serv1:~/terraform$ git show 8c928e8358
commit 8c928e83589d90a031f811fae52a81be7153e82f
...
+func providerSource(services *disco.Disco) getproviders.Source {
...
```
6. Найдите все коммиты в которых была изменена функция `globalPluginDirs`.

- 78b12205587fe839f10d946ea3fdc06719decb05
- 52dbf94834cb970b510f2fba853a5b49ad9b1a46
- 41ab0aef7a0fe030e84018973a64135b11abcd70
- 66ebff90cdfaa6938f26f908c7ebad8d547fea17
- 8364383c359a6b738a436d1b7745ccdce178df47
```
artem@serv1:~/terraform$ git grep globalPluginDirs
commands.go:            GlobalPluginDirs: globalPluginDirs(),
commands.go:    helperPlugins := pluginDiscovery.FindPlugins("credentials", globalPluginDirs())
internal/command/cliconfig/config_unix.go:              // FIXME: homeDir gets called from globalPluginDirs during init, before
plugins.go:// globalPluginDirs returns directories that should be searched for
plugins.go:func globalPluginDirs() []string {
artem@serv1:~/terraform$ git log -L :globalPluginDirs:plugins.go -s --format=%H
78b12205587fe839f10d946ea3fdc06719decb05
52dbf94834cb970b510f2fba853a5b49ad9b1a46
41ab0aef7a0fe030e84018973a64135b11abcd70
66ebff90cdfaa6938f26f908c7ebad8d547fea17
8364383c359a6b738a436d1b7745ccdce178df47

```
7. Кто автор функции `synchronizedWriters`?

Martin Atkins
```
artem@serv1:~/terraform$ git log -SsynchronizedWriters --oneline
bdfea50cc8 remove unused
fd4f7eb0b9 remove prefixed io
5ac311e2a9 main: synchronize writes to VT100-faker on Windows
artem@serv1:~/terraform$ git show -s --format=%an 5ac311e2a9
Martin Atkins
```

