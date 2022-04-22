+++
title = "Lab 2"
weight = 5
date = "2019-05-12"
type = "practice"
+++
  

**Professor**: Joel Fuentes

**Assistants:** Daniel López, Sebastián González

## Description

In this laboratory you must implement a program in DPC ++ that calculates the dot product (also known as the internal product) between two vectors. The dot product is an algebraic operation that takes two sequences of numbers of equal dimension (usually in the form of vectors) and returns a single number.

Considering that there are two vectors **A** and **B** in a space (R^n) , the dot product is realized as a matrix product as follows :

<p align="center">
  <img src="../../images/product.png">
</p>

  
Or equivalently like:

$$\sum_{1}^{n} A_iB_i$$

The objective is that this operation on vectors is executed in parallel in an accelerator (multi-core CPU, GPU or FPGA), for which you can program one or more kernels in DPC++

  
<ol>

<li>Considering the following requirements: The kernel must use ND_Range</li>

<li>The sum of products must be done using reductions </li>

</ol>

Base code to program your solution:


```cpp
#include  <CL/sycl.hpp>
#include  <iostream>

using  namespace  sycl;

int  main(){
	constexpr  int size = 16;

	std::vector<int> A(size);
	std::vector<int> B(size);
	std::vector<int> C(size);

int innerProd = 0;

// Initialize Vectors

	for (int i = 0; i < size; i++) {
		A[i] = (rand() % 100) + 1;
		B[i] = (rand() % 100) + 1;
	}

	queue q;

// Buffer creation

	buffer buffA(A);
	buffer buffB(B);
	buffer buffC(C);

// Buffer for final internal product result

buffer<int, 1> buffSum(&innerProd, 1);

/***
* Add kernel (s) here
*
*
***/

// Show final results
host_accessor buffC_host(buffC);
host_accessor innerProd_result(buffSum);

	for(int i=0; i < size; i++)
		std::cout << "C[" << i << "] = " << buffC_host[i] << std::endl;
		std::cout << "Internal product result: " << innerProd << std::endl;
		return  0;
	}

```

In the following link you will find a Wiki with details about DPC ++: [http://www.face.ubiobio.cl/~jfuentes/classes/hc/unit2/dpcpp/](http://www.face.ubiobio.cl/~jfuentes/classes/hc/unit2/dpcpp/)
  
To test your implementation, it is recommended to run it on Devcloud servers. In the following link you will find a Wiki with details on the configuration of the Devcloud development environment and compilation with DPC ++: [http://www.face.ubiobio.cl/~jfuentes/classes/hc/unit2/devcloud/](http://www.face.ubiobio.cl/~jfuentes/classes/hc/unit2/devcloud/)

**Solution**

```cp

#include <CL/sycl.hpp>
#include <iostream>
using namespace sycl;
   
int main(){

    constexpr int size = 16;
    std::vector<int> A(size);
    std::vector<int> B(size);
    std::vector<int> C(size);
    int innerProd = 0;

    // Initialize Vectors
    for (int i = 0; i < size; i++) {
        A[i] = (rand() % 100) + 1;
        B[i] = (rand() % 100) + 1;
    }

    queue q;

    // Buffer creation
    buffer buffA(A);
    buffer buffB(B);
    buffer buffC(C);
    
    // Buffer for internal product final result
    buffer<int, 1> buffSum(&innerProd, 1);

    auto kernel1 = q.submit([&](handler& h){
        accessor bA(buffA, h);
        accessor bB(buffB, h);
        accessor bC(buffC, h);
        h.parallel_for(nd_range<1>({size, 1}), [=](auto idx){
            auto id_hilo = idx.get_global_id();
            bC[id_hilo] = bA[id_hilo]*bB[id_hilo];
        });
    });

    auto kernel2 = q.submit([&](handler& h){
        h.depends_on(kernel1);
        accessor bC(buffC, h);
        auto sumReduction = reduction(buffSum, h, plus<>());
        h.parallel_for(nd_range<1>({size, 1}), sumReduction, [=](auto idx, auto& sum){
            auto id_hilo = idx.get_global_id();
            sum.combine(bC[id_hilo]);
        });
    });

    host_accessor buffC_host(buffC);
    host_accessor innerProd_result(buffSum);

    for(int i=0; i < size; i++)
        std::cout << "C[" << i << "] = " << buffC_host[i]  << std::endl;
    std::cout << "Inner product result: " << innerProd  << std::endl;
    return 0;
}
```

