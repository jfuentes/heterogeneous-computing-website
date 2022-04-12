+++
title = "Pi Calculator"
weight = 4
date = "2022-04-12"
type = "practice"
+++

***Calculating pi by numerical integration***

Mathematically, we know that:
$$\int_{0}^{1}\frac{4.0}{(1+x^{2})}dx = \Pi $$

We can approximate the integral as a sum of n rectangles:
$$\sum_{i=0}^{n}F(x_{i})\Delta x \approx  \Pi$$


Where each rectangle has width \\(\Delta x\\) and height \\(F(x_{i})\\) at the middle of interval \\(i\\)
<p>
  <img src="../../images/img.png">
</p>

***Serial Code***
```cpp
#include <stdio.h>
#include <omp.h>  //Here we use OpenMP for timing functions

static long num_steps = 100000000;
double step;
int main()
{
    int i;
    double x,pi,sum=0.0;
    double start_time, run_time;
    step = 1.0/(double) num_steps;   //calculate step = Δx

    start_time = omp_get_wtime();

    for (i=1; i<= num_steps; i++){
        x = (i-0.5)*step;  //Get the x coordinate of center of rectangle
        sum = sum + 4.0/(1.0+x*x);
    }
    pi = step * sum;  // Multiply by Δx
    run_time = omp_get_wtime() - start_time;
    printf("\n pi with %d steps is %f in %f seconds ", num_steps.pi,run_time);
}
```
