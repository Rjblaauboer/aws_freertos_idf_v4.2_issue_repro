# Use FreeRTOS in own CMake project for ESP32 (ESP-IDF 4.2)

## Before building

Update submodules: 
- `git submodule update --init --recursive`

Setup python environment: 
- `python3 -m venv env`
- `source env/bin/activate`
- `pip install -r requirements.txt`

Make xtensa compiler toolchain available via PATH:
- `export PATH=$HOME/path/to/toolchain/xtensa-esp-elf/bin:$PATH`
Note: ESP-IDF 4.2 requires toolchain 8.2.0. 
( https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started-legacy/macos-setup.html )

Build using instructions under header "Using FreeRTOS in your own CMake project for ESP32" (https://docs.aws.amazon.com/freertos/latest/userguide/getting_started_espressif.html)

In short:
- `cmake -S . -B build-directory -DCMAKE_TOOLCHAIN_FILE=freertos/tools/cmake/toolchains/xtensa-esp32.cmake`
- `cmake --build build-directory`


## Issue 

REPRO
- Follow instructions until (and including) first cmake build step (`cmake -S . -B ...`)

EXPECTED
- Build succeeds

ACTUAL
- Configuration is incomplete as errors have occured.

Relevant section in error log:
```
CMake Error at freertos/vendors/espressif/esp-idf/tools/cmake/build.cmake:468 (set_property):
  set_property could not find TARGET aws_demos.  Perhaps it has not yet been
  created.
Call Stack (most recent call first):
  freertos/vendors/espressif/boards/esp32/CMakeLists.txt:540 (idf_build_executable)
  freertos/CMakeLists.txt:70 (include)
```

Note: This does not occur on the release branch of amazon-freertos (which uses ESP-IDF v3.3)


