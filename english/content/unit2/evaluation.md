+++
title = "Test 2"
weight = 3
date = "2019-05-11"
type = "eval"
+++

## Heterogeneous computing

1. The SIMD computational model refers to: (5 points)
    - Model in which a thread processes individual data
    - Model in which multiple threads process the same individual data
    - Model in which mutiple threads process multiple data at the same time
    - Model in which multiple threads process individual data

2. According to Flynn's Taxonomy. What model does the processing found in multi-core CPUs correspond to? (5 points)
    - SISD
    - SIMD
    - MISD
    - MIMD

3. In the CUDA programming language for Nvidia GPUs, what is the purpose of the cudaMalloc(...) call? (5 points)
    - Reserve memory in host
    - Launch a running kernel
    - Reserve memory in GPU device
    - Copy data to GPU device

4. In SYCL and DPC++ there are different classes for managing kernels and data. Indicate for each of the following classes its purpose: (12 points)

    | Class          | SRAM     | DRAM    |
    | -------------- | :---:    | :---:   |
    | sycl::device   | &#9744;  | &#9744; |
    | sycl::queue    | &#9744;  | &#9744; |
    | sycl::buffer   | &#9744;  | &#9744; |
    | sycl::handler  | &#9744;  | &#9744; |
    | sycl::accessor | &#9744;  | &#9744; |
    | sycl::range    | &#9744;  | &#9744; |

5. Given the following code in DPC++, What type of accessor should be defines for A and B? (10 points)

    ```cpp
    int main(){
        queue q;
        std::vector<int> v1(N);
        std::vector<int> v2(N);
        ...
        buffer buff1(v1);
        buffer buff2(v2);
        q.submit([&](handler& h){
            accessor A(buff1, h, XXXXX);
            accessor B(buff2, h, XXXXX);
            h.parallel_for(N, [=](auto i){
                B[i] = i + A[i];
            });
        }).wait();
    }
    ```

    - A: read_write and B: read_write
    - A: read_only and B: read_only
    - A: read_only and B: write_only
    - A: write_only and B: write_only

6. Given the following kernel definition, how many threads will be created for parallel_for execution? (10 points)

    ```cpp
    h.parallel_for(nd_range<3>(range<3>({4,5,2}), range<3>({4,2,1})), [=](nd_item<3> item){
        auto global_ = item.get_global_id();
        auto local_id = item.get_local_id();
        ...
    });
    ```
    - 48
    - 4
    - 40
    - 3
    - 20

7. Given the following kernel definition. How many workgroups are defined for the parallel_for? (10 points)

    ```cpp
    h.parallel_for(nd_range<3>(range<3>({4,5,2}), range<3>({4,2,1})), [=](nd_item<3> item){
        auto global_ = item.get_global_id();
        auto local_id = item.get_local_id();
        ...
    });
    ```
    - 40
    - 3
    - 8
    - 1
    - 5

8. GPU Computing Talk: What do the ASICs in the new Nvidia GPUs correspond to? (5 points)
    - Specialized hardware for certain specific tasks
    - Hardware available to run general-purpose computation
    - New programmable L1 cache available in Raytracing and Tensor cores
    - Streaming multiprocesors available on the chip

9. GPU Computing Talk: Explain what the term Multi-chip-module refers to in modern GPUs (8 points)

    ---

        Type your answer here
    
    ---