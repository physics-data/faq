## Python 中的并行计算

通常来说，并行计算可分为数据并行、多线程、多进程、混合并行。下面将对这些方法在 Python 中的使用进行描述。

!!! tip "负载均衡"

    考虑到木桶原理（最慢的一部分决定了整体的速度），无论使用何种并行方式，负载均衡都是很重要的。

### 数据并行

数据并行（data parallel）又名尴尬并行（embarrassingly parallel），正如其名，指那些无需借助任何手段即可实现的并行。例如，多次实验的观测数据天然地可以分为数个可以同时处理的互不依赖的部分。

在这种情况下，只需要简单地启动多个进程，充分 CPU 提供的物理核心即可。通常，推荐正确书写 `Makefile` 后，使用 `make -j$(nproc)` 来并行启动程序。这样，可以保证任务数量始终能够占满物理核心。

### 多线程

多线程（multithreading）是指在一个进程内启动多个计算线程，使用共享内存的方式进行通信。通常这是最简单的并行方式，尤其是使用 OpenMP 等框架可以快速地将一个 C/C++ 程序转化为多线程，并获得客观的性能提升。

然而，由于全局解释器锁（GIL）的存在，通常使用的 CPython 并不能从多线程中获得计算加速收益，甚至 **会变得更慢**，而只有 IO 等阻塞操作或者少部分计算能够获得收益。下面是一些相关的材料：

* <https://python3-cookbook.readthedocs.io/zh_CN/latest/c12/p09_dealing_with_gil_stop_worring_about_it.html>
* <https://realpython.com/python-gil/>
* <https://cenalulu.github.io/python/gil-in-python/>
* <https://cloud.tencent.com/developer/news/743497>

鉴于上述的原因， **在你没有深入地了解多线程、锁、 GIL 等原理前，不要在 Python 中手工编写多线程代码**。

事实上，一些 Python 库（如 `numpy`）会调用原生代码（native code）并使用多个线程进行计算，此时则不会受到 GIL 的影响。

### 多进程

多进程（multiprocessing）是指使用多个进程来进行并行计算，并使用消息传递（message passing）来进行通信。它与数据并行最本质的不同是进程间会互相传递消息，并可能有依赖关系。

#### Python 标准库

Python 的 [`multiprocessing` 标准库](https://docs.python.org/zh-cn/3/library/multiprocessing.html) 中提供了对多进程并行的支持。通常来说，使用 `Pool` 可以完成类似数据并行的计算（并允许在非并行区域中保留全局的状态），从而做到加速；
如果有更复杂的需求，也可以通过其提供的各种通信手段来完成。

!!! warning "小心陷阱"

    尽管 `multiprocessing` 的模式是 fork-join，使用体验接近多线程；但一旦计算进程启动，它与原始进程就拥有 **互相独立的状态**。
    一个典型的问题是，任何一个进程中对全局变量的修改在其他进程中都不可见，必须通过通信来传递它们。  
    此外，对计算进程参数的传递完全是使用 IPC（进程间通信）完成的，有较大的开销（序列化、拷贝、反序列化）；如果传递较大的参数，可能 **严重影响性能**。
    同时，并非所有类型的对象都能被完美地序列化和反序列化，因此可能产生一些非预期的问题。

### MPI 通信

[MPI](https://en.wikipedia.org/wiki/Message_Passing_Interface) 是一套被并行计算业界广泛使用的编程接口，提供了完善的点对点（P2P）、集合（collective）通信原语，有强大的能力。各类超算平台、高速网卡、加速器等都能够完善地支持 MPI，绝大部分复杂的计算软件都是使用 MPI 接口编写的。

Python 的 [MPI for Python](https://mpi4py.readthedocs.io/) 包提供了在 Python 中使用 MPI 接口的能力。它并非一个 MPI 库，在使用前你依旧应当在系统中安装一个 MPI 的实现，如 [OpenMPI](https://www.open-mpi.org/)、[MPICH](https://www.mpich.org/) 或者 [MVAPICH](https://mvapich.cse.ohio-state.edu/)。在使用 `mpi4py` 前，你需要对 MPI 的编程接口、运行方式等有基本的认识。

!!! warning "审慎判断"

    消息传递与共享内存的并行模式并无孰优孰劣，通常根据实际计算模式确定。

### 混合并行

混合使用多进程和多线程即称为混合并行（hybrid parallel），通常是使用多个进程，进程内部使用多线程。这样能够有效地结合两种并行模式的优点，写出更灵活的代码。但考虑到 Python 多线程存在的问题，通常也不建议手工编写混合并行代码。如果在多进程代码中使用了自带多线程计算的库，则事实上已经在使用混合并行的模式。

## 其他加速手段

现代计算机通常还可使用 SIMD 指令集、GPU 加速器等手段来加速大量数据的并行运算。通过下面列出的一些库/框架，可以充分利用此类资源。

### JIT 编译器

即时编译（Just-In-Time, JIT）可以将频繁运行的 Python 函数编译为原生代码，通常能够大幅提升运行效率。

#### PyPy

[PyPy](https://www.pypy.org/) 是使用 Python 实现的 JIT 编译器，在很多情况下相比 CPython 能提供即插即用（drop-in replace）的性能提升。

目前阻碍 PyPy 取代 CPython 的主要原因是其与包含原生代码的 Python 库兼容性不佳。同时，如果代码中已经包含了大量的原生代码（如 `numpy`），则 PyPy 并不能提供更多的性能提升。

#### Numba

[Numba](https://numba.pydata.org/) 同样是基于 Python 的 JIT 编译器，不同的是它专注于提供与 NumPy 兼容的科学计算接口，并同样可以即插即用。
虽然 NumPy 本身具有大量的优化，但 Numba 通常能够进一步地提高计算性能，如智能的多线程、使用 SIMD、在 GPU 运行等。Numba 还提供了较自由的编程接口，让用户能以较少的心智负担书写性能较高的数值计算程序。

### 深度学习框架

除了上面提到的 Numba 外，[PyTorch](https://pytorch.org/)、[TensorFlow](https://www.tensorflow.org/) 等大量的深度学习框架同样提供了方便使用（基于 Tensor 抽象）且性能很高的数值计算功能。

!!! warning "注意开销"

    由于这些框架的核心功能在于训练神经网络，其中包括了大量可能用不到的功能（如自动求导、反向传播、多进程支持），因此也会带来较大的开销（包括库的体积、导入时间等）。
    在使用时，需要谨慎评估需求，避免杀鸡用牛刀。
