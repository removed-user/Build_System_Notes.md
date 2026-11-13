## Setup 
### Basic
```bash
meson setup [BUILD_DIRECTORY] [SOURCE_DIRECTORY]
 ```
 ### Better - as script
 ```bash
BUILD_DIRECTORY="${BUILD_DIRECTORY:-build}"
SOURCE_DIRECTORY="${SOURCE_DIRECTORY:-program_name}"

meson setup "${BUILD_DIRECTORY}" "${SOURCE_DIRECTORY}"
```
