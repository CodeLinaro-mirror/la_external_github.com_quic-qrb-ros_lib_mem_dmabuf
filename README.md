# Lib Mem Dmabuf

[lib_mem_dmabuf](https://github.com/qualcomm-qrb-ros/lib_mem_dmabuf) is a userspace library package for interacting with Linux DMA buffers. It provides:

* C++ APIs and Ament CMake build integration, make it easy to use in ROS 2 projects.
* Importing and accessing underlying file descriptors (fd).
* Flexible buffer management with automatic or manual release.
* Support for buffer release callback registration.

> Prerequisite: Linux kernel version 5.12 or later is required for kernel dma-buf support.

## 🔎 Table of contents
  * [APIs](#-apis)
  * [Supported targets](#-supported-targets)
  * [Usage](#-usage)
  * [Build from source](#-build-from-source)
  * [License](#-license)

## ⚓ APIs

| Function                                                     | Description                             | Parameters                                                   | Return Value              |
| ------------------------------------------------------------ | --------------------------------------- | ------------------------------------------------------------ | ------------------------- |
| DmaBuffer::DmaBuffer(int fd, std::size_t size)               | Constructor for DmaBuffer class         | fd: dma-buf file descriptor, size: dma-buf size              | DmaBuffer object          |
| static std::shared_ptr\<DmaBuffer\> <br>alloc(std::size_t size, const std::string& heap_name) | Alloc dmabuf with size and heap name    | size: buffer size (bytes), heap_name: dmabuf heap name       | Allocated buffer pointer  |
| bool DmaBuffer::release()                                    | Release dmabuf                          | /                                                            | Success or not            |
| bool DmaBuffer::map()                                        | Description                             | /                                                            | Success or not            |
| bool DmaBuffer::unmap()                                      | Description                             | /                                                            | Success or not            |
| bool DmaBuffer::sync_start()                                 | Description                             | /                                                            | Success or not            |
| bool DmaBuffer::sync_end()                                   | Description                             | /                                                            | Success or not            |
| bool DmaBuffer::set_auto_release(bool auto_release)          | Set auto release dmabuf fd when destroy | auto_release: whether to auto release fd when Dmabuf object destroy | Success or not            |
| void DmaBuffer::set_destroy_callback(<br>std::function<void(std::shared_ptr\<DmaBuffer\>)> cb) | Set destroys callback function          | cb: callback function when dmabuf destroy                    |                           |
| bool DmaBuffer::set_data(<br>void* data, std::size_t size, std::size_t offset = 0) | Set data into dmabuf                    | data: data to be saved, size: data size, offset: offset into dma-buf address | Success or not            |
| int DmaBuffer::fd() const                                    | Get dmabuf fd                           | /                                                            | Dmabuf file descriptor    |
| int DmaBuffer::size() const                                  | Get dmabuf size                         | /                                                            | Dmabuf size               |
| void* DmaBuffer::addr()                                      | Get dmabuf CPU memory mapped address    | /                                                            | Dmabuf CPU mapped address |


## 🎯 Supported targets

- Qualcomm Dragonwing™ RB3 Gen2
- Qualcomm Dragonwing™ IQ-9075 EVK

---

## 🚀 Usage

This section shows how to use `lib_mem_dmabuf` to interact with Linux DMA buffers in your projects.

Add the dependencies in your `package.xml`:

```xml
<depend>lib_mem_dmabuf</depend>
```

Configure dependencies in your `CMakeLists.txt`:

```cmake
find_package(ament_cmake_auto REQUIRED)
ament_auto_find_build_dependencies()
```

Allocate a DMA buffer using the C++ APIs:

```c++
#include "lib_mem_dmabuf/dmabuf.hpp"

// Allocate dmabuf with size and DMA heap name
auto buf = lib_mem_dmabuf::DmaBuffer::alloc(1024, "/dev/dma_heap/system");

// Get the file descriptor of the buffer
std::cout << "fd: " << buf->fd() << std::endl;

// Get the CPU-accessible address
if (buf->map()) {
    std::cout << "CPU address: " << buf->addr() << std::endl;
}

// The fd will be automatically closed when buf goes out of scope
```

---

## 👨‍💻 Build from source

Source is located at `sources/quic-qrb-ros/lib_mem_dmabuf/` in the workspace.

```bash
cd build-utils/ubuntu/
python3 build.py --gen-debians --package ros-jazzy-lib-mem-dmabuf
```

Built `.deb` files are output to:

```
<workspace>/debian_packages/oss/ros-jazzy-lib-mem-dmabuf/
```

### Build dependencies

| Package | Source |
| ------- | ------ |
|         |        |

## 📜 License

Project is licensed under the [BSD-3-Clause](https://spdx.org/licenses/BSD-3-Clause.html) License.

