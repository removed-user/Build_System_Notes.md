# TIPS AND TRICKS to use meson as little as required

## Dump Meson Details

## Dump raw Build Commands 
### As raw list
```bash
ninja -C builddir -t commands > all_commands.txt
```

### Only for 'Target_A'
```bash
ninja -C builddir -t commands Target_A
```

## Failing Setup?
## Can't gen build directory?
When meson fails to detect GCC properly
> set fake CC, and gen build-dir, in order to dump with ninja
```bash
CC=./fake_cc.sh CXX=./fake_cc.sh meson setup builddir
```
