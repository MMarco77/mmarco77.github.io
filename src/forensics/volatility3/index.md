# Volatility 3 Commands

Running [Volatility 3](https://github.com/volatilityfoundation/volatility3) in console windows

- Check version.

```shell
$ python vol.py -v
```

- Get image information.

```shell
$ python vol.py -f [ImageName] windows.info
```

- See process list.

```shell
$ python vol.py -f [ImageName] windows.pslist | more
```

- Filter process list searching for keyword “chrome”

```shell
$ python vol.py -f [ImageName] windows.pslist | Select-String chrome
```

- Find all handles open by process 1328.

```shell
$ python vol.py -f [ImageName] windows.handles --pid 1328
```

- Find file handles and filter by - type.

```shell
$ python vol.py -f [ImageName] windows.handles --pid 1328 | Select-String File | more
$ python vol.py -f [ImageName] windows.handles --pid 1328 | Select-String File | Select-String history | more
```

- Dump a file from process 1328 at virtual address.

```shell
$ python vol.py -f [ImageName] -o "dump" windows.dumpfile --pid 1328 --virtaddr 0xbf0f6abe9740
```

- Dump all files associated with PID 2520.

```shell
$ python vol.py -f [ImageName] windows.dumpfiles.DumpFiles --pid 2520
```

- See executed programs with command option history.

```shell
$ python vol.py -f [ImageName] windows.cmdline.CmdLine
```

- See active network connections and listening programs.

```shell
$ python vol.py -f [ImageName] windows.netstat
```

- Dump the Windows user password hashes.

```shell
$ python vol.py -f [ImageName] windows.hashdump.Hashdump
```

- Print out the Windows Registry UserAssist.

```shell
$ python vol.py -f [ImageName] windows.registry.userassist.UserAssist
```

- List all available Windows Registry hives in memory.

```shell
$ python vol.py -f [ImageName] windows.registay.hivelist.HiveList
```

- Dump the ntuser hive based on a keyword filter to the “dump” folder.

```shell
$ python vol.py -f [ImageName] -o "dump" windows.registry.hivelist --filter Doe\ntuser.dat --dump
```

- Print a specific Windows Registry key.

```shell
$ python vol.py -f [ImageName] windows.registry.printkey --key "Software\Microsoft\Windows\CurrentVersion" --recurse
```

- Print a specific Windows Registry key, subkeys and values.

```shell
$ python vol.py -f [ImageName] windows.registry.printkey --key "Software\Microsoft\Windows\CurrentVersion" --recurse
```