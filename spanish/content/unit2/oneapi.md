+++
title = "Intro a OneAPI y DPC++"
weight = 4
date = "2019-05-11"
type = "content"
+++

Introducción a OneAPI y el entorno de desarrollo DevCloud.

![Download slides](../../images/pdf_web.png) [9-devcloud.pdf](../../pdf/9-devcloud.pdf)



Introducción a DPC++.

![Download slides](../../images/pdf_web.png) [11-dpc++.pdf](../../pdf/11-dpc++.pdf)

{{< embed-pdf url="pdf/11-dpc++.pdf" >}}

**Ejemplos**

*Ejemplo 1: Uso de parallel_for*

```cpp
#include <CL/sycl.hpp>
constexpr int N=16;
using namespace sycl;

int main(){
    queue q;
    std::vector<int> v(N);	
    buffer buff(v);
    q.submit([&](handler& h){
        accessor a(buff, h, write_only);
        h.parallel_for(N,[=](auto i) {
            a[i] = i;
        });
    }).wait();
    for (int i=0; i<N; i++) 
		std::cout << v[i] << "\n";
    return 0;
}
```
*Ejemplo 2: Uso de thread id*
```cpp
#include <CL/sycl.hpp>
#include <array>
#include <iostream>
using namespace sycl;
   
int main(){
    constexpr int size = 16;
    std::array<int, size> data;
    //Se crea una cola
    queue Q;
    //Creación del buffer
    buffer B { data };

    Q.submit([&](handler& h){
        accessor A{B,h};
        h.parallel_for(range<1>(size), [=](item<1> idx){
                auto idx2 = idx.get_id();
                auto R = idx.get_range();
        });
    });

    host_accessor A{B};
    for(int i=0; i<size; i++)
        std::cout << "data" << i << " = " << A[i]  << "\n";
    return 0;
}
```
*Ejemplo 3: Uso de Nd_range*
```cpp
#include <CL/sycl.hpp>
#include <array>
#include <iostream>
using namespace sycl;
   
int main(){
    // Configure el dispositivo SYCL y la cola.
    queue q = queue();

    q.submit([&](handler& h) {
        //buffer para printf
        sycl::stream kernelout(1024, 256, h);

        h.parallel_for(nd_range<3>(range<3>({12,8,16}), range<3>({6,2,4})), [=](nd_item<3> item){
            auto global_id = item.get_global_id();
            auto local_id = item.get_local_id();

            if ((global_id[0] == 6) && (global_id[1] == 1) && (global_id[2] == 0)) {
                kernelout << "global id "<< global_id << cl::sycl::endl;
                kernelout << "local id " << local_id << cl::sycl::endl;

                // más información
                kernelout << "More info: " << cl::sycl::endl;
                kernelout << "global range " << item.get_global_range() << cl::sycl::endl;
                kernelout << "global linear id " << item.get_global_linear_id() << cl::sycl::endl;
                kernelout << "group range " << item.get_group_range() << cl::sycl::endl;
                kernelout << "group " << item.get_group().get_id() << cl::sycl::endl;
                kernelout << "group linear id " << item.get_group_linear_id() << cl::sycl::endl;
                kernelout << "local range " << item.get_local_range() << cl::sycl::endl;
                kernelout << "local linear id " << item.get_local_linear_id() << cl::sycl::endl << cl::sycl::endl;
                sub_group sg = item.get_sub_group();
                kernelout << "subgroup group range " << sg.get_group_range() << cl::sycl::endl;
                kernelout << "subgroup group id " << sg.get_group_id() << cl::sycl::endl;
            }
        });
    });
    return 0;
}
```
*Ejemplo 4: Uso de reducción*
```cpp
#include <CL/sycl.hpp>
#include <array>
#include <iostream>
using namespace sycl;
   
int main(){
    constexpr int size = 16;

    std::array<int, size> data;
    int totalSum = 0;

    //Se crea una cola
    queue Q;

    //Creación del buffer
    buffer buff(data);
    buffer<int, 1> buffSum(&totalSum, 1);

    Q.submit([&](handler& h){
        accessor buff_access(buff, h);

        auto sumReduction = reduction(buffSum, h, maximum<>());

        h.parallel_for(nd_range<1>({size, 1}), sumReduction, [=](auto idx, auto& sum){
            auto id_hilo = idx.get_global_id();
            if (id_hilo%2 == 0) {
                buff_access[id_hilo] = id_hilo;
                sum.combine(id_hilo);
            }
        });
    });

    host_accessor buff_host(buff);
    host_accessor buffSum_host(buffSum);

    for(int i=0; i < size; i++)
        std::cout << "data[" << i << "] = " << buff_host[i]  << std::endl;
    std::cout << "Suma total hilos pares: " << totalSum  << std::endl;
    return 0;
}
```
*Ejemplo 5: Dependencias entre kernels*
```cpp
#include <CL/sycl.hpp>
#include <array>
#include <iostream>
using namespace sycl;
   
int main(){
    constexpr int size = 16;

    std::array<int, size> data;
    int totalSum = 0;

    //Se crea una cola
    queue q;

    //Creación del buffer
    buffer buff(data);
    buffer<int, 1> buffSum(&totalSum, 1);

    auto kernel1 = q.submit([&](handler& h){
        accessor buff_access(buff, h);
        h.parallel_for(nd_range<1>({size, 1}), [=](auto idx){
            auto id_hilo = idx.get_global_id();
            buff_access[id_hilo] = 0;
        });
    });

    auto kernel2 = q.submit([&](handler& h){
        h.depends_on(kernel1);
        accessor buff_access(buff, h);
        h.parallel_for(nd_range<1>({size, 1}), [=](auto idx){
            auto id_hilo = idx.get_global_id();
            if (id_hilo%2 == 0) {
                buff_access[id_hilo] = id_hilo;
            }
        });
    });

    auto kernel3 = q.submit([&](handler& h){
        h.depends_on(kernel2);
        accessor buff_access(buff, h);
        auto sumReduction = reduction(buffSum, h, plus<>());
        h.parallel_for(nd_range<1>({size, 1}), sumReduction, [=](auto idx, auto& sum){
            auto id_hilo = idx.get_global_id();
            if (id_hilo%2 == 0) {
                sum.combine(id_hilo);
            }
        });
    });

    host_accessor buff_host(buff);
    host_accessor buffSum_host(buffSum);

    for(int i=0; i < size; i++)
        std::cout << "data[" << i << "] = " << buff_host[i]  << std::endl;
    std::cout << "Suma total hilos pares: " << totalSum  << std::endl;
    return 0;
}
```