Last name of Student 1: Baas
First name of Student 1: Kailash
Email of Student 1: kailashbaas@ucsb.edu
Last name of Student 2: Gamliel
First name of Student 2: Daniel
Email of Student 2: dgamliel@ucsb.edu


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
omp_set_num_threads(threadcnt);
    This makes sure the requested number of threads is used.

if (mappingtype == BLOCK_MAPPING)
    omp_set_schedule(omp_sched_static, ceil(no_iterations / threadcnt));
else if (mappingtype == BLOCK_CYCLIC)
    omp_set_schedule(omp_sched_static, chunksize);
else if (mappingtype == BLOCK_DYNAMIC)
    omp_set_schedule(omp_sched_dynamic, chunksize);
else
    omp_set_schedule(omp_sched_guided, chunksize);
        These lines set the scheduling type depending on the value of mappingtype

#pragma omp parallel for schedule (runtime) shared(i)
    This line parallelizes the first inner loop using the runtime scheduling
    specified by omp_set_schedule
#pragma omp parallel for schedule (runtime) shared(i)
    This line parallelizes the second inner loop using the runtime scheduling
    specified by omp_set_schedule

----------------------------------------------------------------------------
Parallel time/speedup/efficiency using 2 and 4 cores under different scheduling methods
1 core:
    Block Mapping:
        Sequential time: 53.970537 s
    Block Cyclic Mapping (block size 1):
        Sequential time: 53.933626 s
    Block Cyclic Mapping (block size 16):
        Sequential time: 53.935546 s
    Dynamic Method:
        Sequential time: 53.948657 s
2 cores:
    Block Mapping:
        Parallel time: 27.278706 s
        Speedup: 1.978
        Efficiency: 0.989
    Block Cyclic Mapping (block size 1):
        Parallel time: 27.277122 s
        Speedup: 1.977
        Efficiency: 0.9885
    Block Cyclic Mapping (block size 16):
        Parallel time: 27.288873 s
        Speedup: 1.976
        Efficiency: 0.988
    Dynamic Method:
        Parallel time: 27.605053 s
        Speedup: 1.954
        Efficiency: 0.977
4 cores:
    Block Mapping:
        Parallel time: 13.641491 s
        Speedup: 3.956
        Efficiency: 0.989
    Block Cyclic Mapping (block size 1):
        Parallel time: 14.550201 s
        Speedup: 3.707
        Efficiency: 0.927
    Block Cyclic Mapping (block size 16):
        Parallel time: 13.849451 s
        Speedup: 3.894
        Efficiency: 0.9735
    Dynamic Method:
        Parallel time: 13.773678 s
        Speedup: 3.917
        Efficiency: 0.979


If there are significant performance differences among different thread scheduling  methods,
provide a short reason.


