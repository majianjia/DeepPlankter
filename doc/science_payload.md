# Science Payload

What is science payload and how does it implemented. 

## Data and On-board Processing

When the boat is in the middle of an ocean, the communication ability will be very limited. Normally, only heart-beat package and status data will be transferred back to us in "real-time".  All raw sensor data will be stored into the on board memory. If we are lucky enough, the boat can safely sail back to us,  we then can retrieve these valuable data.

Due to the limitation of memory, most of the data will be record at a lower data rate (1Hz), but some sensors might have a real sampling rate of 20Hz or higher. Which means a huge loss of data if we only analysis the recorded data. 

Also, some data is more valuable in real-time than post processing, such as weather or water data. These data is preferable to be processed and send back to us in real-time rather than being post processing months later.  

Therefore, for some application, data processing is better to perform onboard. 

- data processing that needs high rate sensor data, such as motion data
- data processing results that needs to send back in "real-time".

For example, we can use the high rate motion data to calculate the wave-height, then send the result back to us every hour. The data processing results is only a few bytes which is small enough to be packed into the heart-beat message.  

## Virtual Machine and Small Program

Science payload are a small programs that can run on the main MCU through a Virtual Machine (VM). 

Using an independent VM has some benefits.

- programs can be loaded from file system so it is exchangeable. 
- can be upgraded over-the-air. 
- split working environments, therefore protect the controller functions from the user program.  
- can also be used to overwrite controller's native functions such as navigation. 

There are some trade-off when using VM. VM are always slower compared to native and the runtime normally takes a great amount of resources (especially with RAM). Since the MCU is quite powerful and it has a large 1MB RAM, it is not a problem to run an embedded VM with some small programs. 

I compared a few different VM, such as micropython, lua, and a few wasm engines, also the rt-thread's own dynamic module. The final choose is [WASM-Micro-Runtime (wamr)](https://github.com/bytecodealliance/wasm-micro-runtime). It has some clear advantages. 

- It has ahead of time (AoT) compiling. Can achieve 0.7 ~0.9x native code speed. 
- It is small enough. Support both Interpreter mode or AoT mode. 
- Binary is small enough so that can be transfer even with satellites communication. 
- It has memory boundary checking. 
- User side programing is very easy, it is basically a WebAssembly program that can be written by any supported language, rust, C, C++..

# Setup Development Environment

The user side is very easy. Just download the [wasi-sdk](https://github.com/WebAssembly/wasi-sdk/releases) and set up according to its tutorial.

A simple program compiled by default config is enough, which is a normal webassembly application with bytecode for interpreter-based interpreter. You could put it to online site to run already. Most importantly, our boat controller is already capable to run this. 

The program takes around 29kB by default compiling option. 

However, this program will be quite large and slow. We can follow the wamr's suggest options to optimize the code. 

```
/opt/wasi-sdk/bin/clang -O3 -nostdlib \
    -z stack-size=8192 -Wl,--initial-memory=65536 \
    -o test.wasm test.c \
    -Wl,--export=main -Wl,--export=__main_argc_argv \
    -Wl,--export=__heap_base -Wl,--export=__data_end \
    -Wl,--no-entry -Wl,--strip-all -Wl,--allow-undefined
```

Finally, we can further use  wamr's tools and follow [the tutorial](https://github.com/bytecodealliance/wasm-micro-runtime/blob/main/doc/build_wasm_app.md#compile-wasm-to-aot-module) to translate the interpreter-based bytecode to platform native bytecode to achieve native level speed. 

# App Framework

For science payloads, it is treated as an simple App. To be notice that the this App framework is not the App Framework in wamr. 

There are 3 methods for each App. 

```c
init() {};
epoch() {};
uninit() {};
```

- `init()` will be called when the system start up, where the app should resume the state when needed. Normally after a day reset. 

- `epoch()`will be called at every period to do the majority of the works. (Setting such as period, heap size, etc. will be set through related parameters).  

- `uinit()` will be called before the system reset. It is where the app should store the current state if needed.  

Each App has it own set of parameters, can be used to keep the state or communicate to the remote. User app can also simply use global variable to stored data and state across different epoch call.   

App can access filesystem when required, but should be limited to loading or writing configuration files. Instead, for data recording, the system has provided an async recorder interfaces, managed by an independent thread and the memory is in system space. It will queue the data, automatically split the file when the size is too large, and flush data into the filesystem every a few seconds. It is much safer to use the recorder interface for recording than accessing filesystem from App.  

# Example

 Run an hello world in .
