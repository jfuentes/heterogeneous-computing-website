+++
title = "Cálulo de Pi"
weight = 3
date = "2022-04-12"
type = "practice"
+++

**Cálculo de Pi por integración númerica**

Matemáticamente, sabemos que:
$$\int_{0}^{1}\frac{4.0}{(1+x^{2})}dx = \Pi $$

Podemos aproximar la integral como una suma de \\(n\\) rectángulos:
$$\sum_{i=0}^{n}F(x_{i})\Delta x \approx  \Pi$$

donde cada rectángulo tiene ancho \\(\Delta x\\) y alto \\(F(x_{i})\\) en la mitad del intervalo \\(i\\)

<center>
<p>
  <img src="../../images/img.png">
</p>
</center>

Podemos implementar el cálculo de Pi de la siguiente forma:

***Solución secuencial***
```cpp
#include <stdio.h>
#include <omp.h>  //Aquí usamos OpenMP para funciones de temporización

static long num_steps = 100000000;
double step;
int main()
{
    int i;
    double x,pi,sum=0.0;
    double start_time, run_time;
    step = 1.0/(double) num_steps;   //calcular step = Δx

    start_time = omp_get_wtime();

    for (i=1; i<= num_steps; i++){
        x = (i-0.5)*step;  //Obtenga la coordenada x del centro del rectángulo
        sum = sum + 4.0/(1.0+x*x);
    }
    pi = step * sum;  //Multiplicar por Δx
    run_time = omp_get_wtime() - start_time;
    printf("\n pi con %d pasos es %f en %f segundos", num_steps.pi,run_time);
}
```

***Solución paralela en DPC++***

```cp

#include <CL/sycl.hpp>
#include <array>
#include <iostream>
#include <sys/time.h>
#include <limits>

using namespace sycl;

double f(double x) {
    return (4.0 / (1.0 + x*x));
}

double CalcPi (int n) {
    const double fH = 1.0 / (double) n;
    double fSum = 0.0;
    
    queue q;
    buffer<double, 1> fSum_buff(&fSum, 1);
    
    const int NUM_THREADS = q.get_device().get_info<info::device::max_compute_units>();
    std::cout << "Num hilos = " << NUM_THREADS << std::endl;
    
    q.submit([&](handler& h){
        auto sumReduction = reduction(fSum_buff, h, plus<>());
        h.parallel_for(nd_range<1>({NUM_THREADS, 1}), sumReduction, [=](auto idx, auto& sum){
            auto id = idx.get_global_id();
            double fX;
    
            int portion = n/NUM_THREADS;
            int start = id * portion;
            int end = start + portion;
    
            for (int i = start; i < end; i++){       
                fX = fH * ((double)i + 0.5);
                sum.combine(f(fX));
            }
        });
    });
    host_accessor buffSum_host(fSum_buff);
    return fH * fSum;
}
    
int main(){
    struct timeval start_time, stop_time, elapsed_time;
    
    gettimeofday(&start_time,NULL); // Unix timer
    int n = std::numeric_limits<int>::max();
    double pi = CalcPi(n);
    gettimeofday(&stop_time,NULL);
    
    std::cout << "Pi = " << pi << std::endl;
    timersub(&stop_time, &start_time, &elapsed_time);
    std::cout << "Total time  " << (elapsed_time.tv_sec+elapsed_time.tv_usec/1000000.0) << "sec" << std::endl;
         
    return 0;
}

```