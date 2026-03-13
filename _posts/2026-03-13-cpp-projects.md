---
layout: post
title: 有意思的 C++ 项目推荐
date: 2026-03-13 16:00:00+0000
description: 推荐一些有趣且实用的 C++ 项目，适合不同层次的开发者动手实践。
tags: c++ projects programming
categories: cpp
giscus_comments: true
related_posts: true
---

学 C++ 最好的方式就是动手做项目。下面推荐一些有意思、有挑战性的 C++ 项目方向，覆盖从入门到进阶的不同难度，可以根据自己的兴趣和水平来选择。

---

## 🔰 入门级

### 1. 命令行计算器

实现一个支持四则运算、括号优先级、甚至三角函数的命令行计算器。核心练习点：字符串解析、递归下降语法分析。

```cpp
// 简单示例：读取并计算表达式
#include <iostream>
#include <string>

double eval(const std::string& expr);  // 自行实现递归下降解析

int main() {
    std::string line;
    while (std::getline(std::cin, line)) {
        std::cout << "= " << eval(line) << "\n";
    }
}
```

### 2. 贪吃蛇 / 俄罗斯方块（终端版）

利用 `ncurses`（Linux）或 Windows Console API 实现经典小游戏，练习帧循环、键盘输入处理、2D 数组操作。

### 3. 简易 JSON 解析器

从零手写一个能够解析 JSON 字符串并生成 AST（抽象语法树）的库，练习递归解析与数据结构设计。

---

## ⚙️ 中级

### 4. 迷你线程池

用 `std::thread`、`std::mutex`、`std::condition_variable` 实现一个固定大小的线程池，支持任务提交和结果获取（`std::future`）。

```cpp
#include <functional>
#include <future>
#include <queue>
#include <thread>
#include <vector>
#include <mutex>
#include <condition_variable>

class ThreadPool {
public:
    explicit ThreadPool(std::size_t n);
    template<class F, class... Args>
    auto submit(F&& f, Args&&... args)
        -> std::future<std::invoke_result_t<F, Args...>>;
    ~ThreadPool();
private:
    std::vector<std::thread> workers;
    std::queue<std::function<void()>> tasks;
    std::mutex mtx;
    std::condition_variable cv;
    bool stop = false;
};
```

### 5. HTTP/1.1 服务器（从 socket 写起）

只用 POSIX socket API（或 Winsock）实现一个能处理 GET 请求、返回静态文件的 HTTP 服务器，练习网络编程、IO 多路复用（`select` / `epoll`）。

### 6. 内存池分配器

实现一个自定义内存分配器，替代 `new`/`delete`，减少碎片，理解 C++ 内存模型与对象生命周期。

### 7. 简单的 Key-Value 数据库

参考 LevelDB 的 LSM-Tree 思路，实现一个基于有序 SSTable + MemTable 的持久化 KV 存储引擎，练习文件 IO、序列化、跳表/红黑树。

---

## 🚀 进阶

### 8. 迷你编译器 / 解释器

为一门简单语言（如类 C 的玩具语言）实现词法分析器 → 语法分析器 → 语义分析 → 字节码/LLVM IR 生成，深入理解编译原理。可以参考《手写一个解释器》系列或 LLVM Kaleidoscope 教程。

### 9. 光线追踪渲染器

《Ray Tracing in One Weekend》是一本免费在线书籍，用约 500 行 C++ 即可实现光线追踪渲染器，最终输出带有阴影、反射、折射的 PPM 图片，非常有成就感。

```cpp
// 核心循环示意
for (int j = image_height - 1; j >= 0; --j) {
    for (int i = 0; i < image_width; ++i) {
        auto u = double(i) / (image_width - 1);
        auto v = double(j) / (image_height - 1);
        ray r = cam.get_ray(u, v);
        color pixel_color = ray_color(r, world);
        write_color(std::cout, pixel_color);
    }
}
```

### 10. 协程 / Fiber 库

在 x86-64 Linux 上用内联汇编或 `ucontext`/`setjmp` 手动切换栈，实现协程原语，进而搭建一个类 `go` 的轻量协程调度器，深入理解上下文切换与调度。

---

## 📦 值得参考的开源项目

| 项目 | 简介 | 难度 |
|------|------|------|
| [muduo](https://github.com/chenshuo/muduo) | 陈硕写的高性能 C++ 网络库，教学价值极高 | ★★★ |
| [TinyWebServer](https://github.com/qinguoyi/TinyWebServer) | 国内很流行的 C++ Web 服务器练手项目 | ★★ |
| [leveldb](https://github.com/google/leveldb) | Google 出品的 KV 存储，代码质量极高 | ★★★ |
| [folly](https://github.com/facebook/folly) | Meta 的 C++ 基础库，现代 C++ 写法范本 | ★★★★ |
| [abseil-cpp](https://github.com/abseil/abseil-cpp) | Google 的 C++ 公共基础库 | ★★★ |
| [LLVM](https://llvm.org/) | 编译器基础设施，也是学习 C++ 大型项目的绝佳范例 | ★★★★★ |

---

## 💡 选项目的建议

1. **跟着兴趣走**：对网络感兴趣就做服务器，对图形感兴趣就做渲染器，对系统感兴趣就做内存/协程库。
2. **先跑通，再重构**：别一开始就追求完美设计，先让东西能跑起来再迭代。
3. **写文章记录**：把做项目的过程写成博客，既巩固知识又能分享给他人（就像这篇文章一样 😄）。
4. **读优质源码**：做完自己的版本之后，对比 muduo、leveldb 等项目的实现，差距就是成长空间。

---

欢迎在评论区分享你正在做或打算做的 C++ 项目！
