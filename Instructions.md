# Compile

## Using MinGW GCC on Windows


To build the makefiles, Use something like the following, changing the relative paths accordingly:
```
cmake.exe -D CMAKE_C_COMPILER="C:/msys64/mingw64/bin/gcc.exe" -D CMAKE_CXX_COMPILER="C:/msys64/mingw64/bin/g++.exe" -D ZLIB_LIBRARY="C:/msys64/mingw64/lib/libz.a" -D ZLIB_INCLUDE_DIR="C:/msys64/mingw64/include" -D CMAKE_CXX_STANDARD="11" -G "MinGW Makefiles" -S . -B build
```

check:  https://github.com/jakublevy/glucose-win for errors in windows



-----------------------------

Windows does not directly support `getrlimit()` or the equivalent functionality that is available on Unix-like operating systems, such as Linux and macOS. The `getrlimit()` system call is used in Unix-based systems to query resource limits (e.g., memory, CPU time) for processes.

However, there are ways to manage resource limits on Windows, although they are handled differently, often at the process or thread level. Here are some alternatives for controlling and querying resource limits on Windows:

### 1. **Memory Limits**
   - **Virtual Memory Limits**: You can set limits on the memory a process can use by configuring job objects or using the `SetProcessWorkingSetSize()` function to limit the working set size. However, this is not as fine-grained as `getrlimit()`, and it only affects the process’s working set (physical memory), not virtual memory.
   - **Job Objects**: A Job Object can be used to set resource limits on a group of processes. You can set limits like CPU time, memory usage, and more for processes associated with a job object.

     Example to create and manage job objects in Windows:
     ```cpp
     HANDLE hJob = CreateJobObject(NULL, NULL);
     JOBOBJECT_BASIC_LIMIT_INFORMATION jbli;
     jbli.PerProcessUserTimeLimit.QuadPart = 600000000; // 10 minutes in 100ns units
     SetInformationJobObject(hJob, JobObjectBasicLimitInformation, &jbli, sizeof(jbli));
     ```

### 2. **CPU Time Limits**
   - Windows does not natively expose a way to limit CPU time per process like `getrlimit(RLIMIT_CPU)` in Unix-like systems. However, you can use job objects to set limits on CPU time, or you can use performance counters and monitoring tools to track CPU usage, then manually terminate or adjust the process if it exceeds certain thresholds.

### 3. **File Descriptors**
   - Windows doesn’t have a concept of file descriptor limits like Unix systems do (e.g., `RLIMIT_NOFILE`). You can, however, manage the number of open files programmatically by limiting how many files your application opens, using mechanisms like handle limits in your application logic.

### 4. **Thread and Process Limits**
   - Windows allows you to manage the number of threads a process can spawn, typically through application logic and OS APIs. However, limiting the number of threads or processes in a way that directly corresponds to `getrlimit()` would require custom management.

### Workaround via Windows API

While there's no direct equivalent of `getrlimit()` on Windows, you can emulate similar functionality using the following Windows APIs:

- **`SetProcessAffinityMask()`**: Restrict the process to a specific set of CPUs (restricting CPU usage).
- **`SetProcessWorkingSetSize()`**: Restrict the working set size of a process.
- **Job Objects (`CreateJobObject`, `SetInformationJobObject`)**: Control process resource limits, like CPU time, memory usage, and the number of processes.

### Third-Party Libraries
For more advanced control over resource limits on Windows, some third-party libraries or tools might be available that simulate the functionality of `getrlimit()` by wrapping the appropriate Windows APIs or providing higher-level abstractions.

### Conclusion
While Windows does not provide a direct `getrlimit()` equivalent, you can manage process resource limits through mechanisms like **job objects** for memory and CPU limits, **process working set management**, and **custom thread/process management**. You will need to use specific Windows APIs to achieve the desired behavior, depending on the resource type you want to limit.