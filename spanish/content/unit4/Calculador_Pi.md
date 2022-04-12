+++
title = "Calulo de Pi"
weight = 4
date = "2022-04-12"
type = "practice"
+++

**Calculo de pi por integración númerica**

Matemáticamente, sabemos que:
$$\int_{0}^{1}\frac{4.0}{(1+x^{2})}dx = \Pi $$

Podemos aproximar la integral como una suma de n rectángulos:
$$\sum_{i=0}^{n}F(x_{i})\Delta x \approx  \Pi$$

donde cada rectángulo tiene ancho \\(\Delta x\\) y alto \\(F(x_{i})\\) en la mitad del intervalo \\(i\\)

<p>
  <img src="../../images/img.png">
</p>

***Serial Code***
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
