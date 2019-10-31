Last name of Student 1:
First name of Student 1:
Email of Student 1:
Last name of Student 2:
First name of Student 2:
Email of Student 2:


----------------------------------------------------------------------------
Report for Question 1
How is the code parallelized?
omp_set_num_threads(threadcnt);
    This makes sure the requested number of threads is used.
#pragma omp parallel private(distance_squared, toss, x, y) reduction(+ : sum)
    This line makes sure that the loop is executed in parallel. Because each
    thread needs it's own copy of distance_squared, toss, x, and y, these 4
    variables are made private. Each thread will compute it's own sum, so sum
    is used as the destination for an addition reduction to ensure the final
    result is equal to the sum of the individual sums.
int tid = omp_get_thread_num();
    This allows us to know the current thread id, which will help in partitioning
    the loop.
for (toss = tid; toss < number_of_tosses; toss += threadcnt)
    This partitions the work among the threads using cyclic mapping based on the
    thread id.

----------------------------------------------------------------------------
Report for Question 2
How is the code parallelized?


----------------------------------------------------------------------------
Parallel time/speedup/efficiency using 2 and 4 cores under different scheduling methods



If there are significant performance differences among different thread scheduling  methods,
provide a short reason.


