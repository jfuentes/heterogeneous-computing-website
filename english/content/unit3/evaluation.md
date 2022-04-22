+++
title = "Test 3"
weight = 6
date = "2019-05-11"
type = "eval"
+++

## Heterogeneous computing

1. Which of the following operations does not correspond to a reduction function in DPC++? (10 points)
    - maximum (max)
    - plus (add)
    - minimum (mul)
    - parallel_for

2. Given the following code. Indicate the order in which each kernel is executed: (10 points)

    ```cpp
    queue q;
    auto A = q.submit([&])(handler& h){});

    auto C = q.submit([&])(handler& h){
        h.depends_on(A);
    });

    auto B = q.submit([&])(handler& h){
        h.depends_on(A);
        h.depends_on(C);
    });

    auto D = q.submit([&])(handler& h){
        h.depends_on(A);
        h.depends_on(B);
    });

    auto E = q.submit([&])(handler& h){
        h.depends_on(C);
        h.depends_on(D);
    });
    ```
    ---

        Type your answer here

    ---

3. Which of the following design patterns considers using neighbor data to calculate an end result? (10 points)
    - fork-join
    - map
    - stencil
    - scan
    - recurrence

4. Indicate if the following statement is true or false: A parallel reduction will require fewer operations than a sequential reduction. (10 points)
    - True
    - False

5. Indicate if the following statement is true or false. A parallel scan will require fewer operations than a sequential scan. (10 points)
    - True
    - False

6. Indicate the resulting vector after applying to vector A a Gather operation with the indexes in B: (10 points)

    Values vector  -->  A: [4, 2, 3, 5, 0, 1, 3, 2]  
    Indexes vector -->  B: [5, 1, 1, 0, 3, 2, 4, 0]

    ---

        Type your answer here

    ---

7. Indicate the purpose of adding padding when programming AoS and SoA structures. (10 points)

    ---

        Type your answer here

    ---