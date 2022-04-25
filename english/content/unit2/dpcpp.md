+++ 
title = "DPC++" 
weight = 8
date = "2019-05-12" 
type = "wiki" 
+++

  

Links:

  

-  [Data Parallel C++ Book](https://link.springer.com/content/pdf/10.1007%2F978-1-4842-5574-2.pdf)

-  [Examples in C++](https://github.com/oneapi-src/oneAPI-samples/tree/master/DirectProgramming/C%2B%2B)

-  [Examples in DPC++](https://github.com/oneapi-src/oneAPI-samples/tree/master/DirectProgramming/DPC%2B%2B)

  

Always use in code:

```cpp
#include  <CL/sycl.hpp>

Using namespace  sycl;
```

  

### 1. Memory Management
------
#### Malloc
To manage memory use a pointer-based approach, more specifically Malloc. 
For example, create memory location to share:

```cpp
char *example = malloc_shared<char>(variable,queue);
```

In SYCL you can use 3 types: *malloc_host*, *malloc_shared* and *malloc_device*:

```Cpp
int* host_array = malloc_host(N, Q);
int* shared_array = malloc_shared(N, Q);
int* device_array = malloc_device(N, Q);
```

Characteristics:

Type | Description | Accessible on host? | Accessible on device? | Located on
--- | --- | --- | --- | ---
**device** | Allocation in device memory | No |Yes | device
**host** | Allocations in host memory | Yes | Yes | host
**shared** | Allocations shared between host and device | Yes | Yes | can migrate back and forth

  <br><br>

#### Buffer

Another option is the use of buffers:
The easiest way to declare them is by indicating the data source in their constructor: array, vector, pointer, etc.
Example:
```cpp
buffer  my_buffer(my_data);
```

  

To access these you must use an 'accessor'

Example:
```cpp
accessor  my_accessor(my_buffer, h)
```

  

Access type| Description|
| ---|---|
| *read_only* | Read only access |
| *write_only* | Write only access |
| *read_write* | Read and write access |

<br><br>
  
### 2. Implementation and management of Kernels
------
#### Queues
The queues allow us to connect with the devices, with them we send kernels to execute work and move data.
```cpp
Queue <queue name>;
```

  

#### Parallel_for
It allows to define a parallel loop that will be executed by many threads depending on the number of iterations. In its simplified version, the compiler performs the division of work (iterations) in the threads.
```cpp
h.parallel_for (range {N}, [=] (id <1> idx) {
```

As you can see it is like a lambda function, the function takes two arguments, the first is called a range which specifies the number of elements to throw in each dimension and the second is a kernel function that will be executed for each index of the range .

Note: They can use for several dimensions, for two dimensions it would be:
```cpp
h.parallel_for (range {N, M}, [=] (id <2> idx)
```
<br><br> 

**A complete code example:**
```cpp
#include <CL/sycl.hpp>
#include <array>
#include <iostream>

using namespace sycl;

int main(){
	constexpr int size = 16;
	std::array<int, size> data;

	queue Q;
	buffer B { data };
	Q.submit([&](handler& h){
	accessor A{B,h};

	h.parallel_for(size, [=](auto& idx){
		A[idx] = idx;
		});
	});


	host_accessor A{B};
	for(int i=0; i<size; i++)
	std::cout << "data" << i << " = " << A[i] << "\n";

return 0;
}
