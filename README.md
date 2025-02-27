[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/IAASVEAZ)
# CSIT5970 Assignment-1: EC2 Measurement (2 questions, 4 marks)

### Deadline: 11:59PM, Feb, 28, Friday

---

### Name: TAN, Ruang
### Student Id: 21142966
### Email: rtanae@connect.ust.hk

---

## Question 1: Measure the EC2 CPU and Memory performance

1. (1 mark) Report the name of measurement tool used in your measurements (you are free to choose *any* open source measurement software as long as it can measure CPU and memory performance). Please describe your configuration of the measurement tool, and explain why you set such a value for each parameter. Explain what the values obtained from measurement results represent (e.g., the value of your measurement result can be the execution time for a scientific computing task, a score given by the measurement tools or something else).

    > ### 1. **Measurement Tool Name:**
    >
    > The measurement tool used in my performance tests is **sysbench**.
    >
    > ### 2. **Configuration of the Measurement Tool:**
    >
    > The tool was configured with the following commands:
    >
    > #### **CPU Performance Test:**
    >
    > ```
    > sysbench cpu --cpu-max-prime=20000 --threads=<vCPUs> run
    > ```
    >
    > - **`--cpu-max-prime=20000`**: This parameter sets the maximum value for prime number calculations, which tests the CPU's ability to handle computationally intensive tasks. A value of 20000 was selected because it provides a challenging but manageable task for benchmarking.
    > - **`--threads=<vCPUs>`**: This parameter sets the number of threads to match the number of virtual CPUs (vCPUs) available on each EC2 instance. This ensures that each core is utilized during the test, which is important for measuring multi-core performance.
    >
    > #### **Memory Performance Test:**
    >
    > ```
    > sysbench memory --memory-block-size=1M --memory-total-size=10G --threads=<vCPUs> run
    > ```
    >
    > - **`--memory-block-size=1M`**: This sets the size of each memory block for the test to 1MB, simulating realistic memory operations.
    > - **`--memory-total-size=10G`**: This tests memory performance using a total of 10GB of memory, which helps simulate the handling of large data sets.
    > - **`--threads=<vCPUs>`**: The number of threads was set to the number of vCPUs to ensure that memory operations are spread across all available cores for a more accurate measurement.
    >
    > ### 3. **Explanation of Parameter Choices:**
    >
    > - **Prime number limit (`--cpu-max-prime=20000`)**: I chose this value to stress the CPU with a challenging but manageable number, ensuring the benchmark is realistic for the instance's computational capability.
    > - **Thread count (`--threads=<vCPUs>`)**: Setting the number of threads equal to the vCPU count ensures that the system utilizes all available CPU cores, which is crucial for accurate performance measurement, especially in multi-core systems like the `t2.medium` and `c5d.large`.
    > - **Memory block size and total size**: These values simulate real-world memory usage scenarios where large data sets need to be processed. The 10GB memory size is large enough to test memory bandwidth and throughput without overwhelming the system.
    >
    > ### 4. **Interpretation of Measurement Results:**
    >
    > - The **CPU performance** results include metrics such as **events per second**, representing the rate at which the CPU performs calculations. A higher value indicates better CPU performance.
    > - The **memory performance** results focus on **throughput**, representing the efficiency of memory operations. Higher throughput values indicate better memory performance.

2. (1 mark) Run your measurement tool on general purpose `t2.micro`, `t2.medium`, and `c5d.large` Linux instances, respectively, and find the performance differences among these instances. Launch all the instances in the **US East (N. Virginia)** region. Does the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource?

    In order to answer this question, you need to complete the following table by filling out blanks with the measurement results corresponding to each instance type.

    | Size        | CPU performance | Memory performance |
    | ----------- | --------------- | ------------------ |
    | `t2.micro` | 892.69 | 19287.53 |
    | `t2.medium`  | 1591.10 | 27850.30 |
    | `c5d.large` | 701.82 | 19209.79 |

    > Region: US East (N. Virginia). Use `Ubuntu Server 24.04 LTS (HVM)` as AMI.
    >
    > The performance of EC2 instances does not always scale directly with the number of vCPUs and memory. While **t2.medium** shows clear improvements in both CPU and memory over **t2.micro**, **c5d.large** does not exhibit the expected improvement in CPU performance, suggesting that performance scaling depends on the specific workload and instance optimizations.

## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.

    | Type                      | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | `t3.medium` - `t3.medium` |                |          |
    | `m5.large` - `m5.large`   |                |          |
    | `c5n.large` - `c5n.large` |                |          |
    | `t3.medium` - `c5n.large` |                |          |
    | `m5.large` - `c5n.large`  |                |          |
    | `m5.large` - `t3.medium`  |                |          |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. Note: Use private IP address when using iPerf within the same region. You'll need iPerf for measuring TCP bandwidth and Ping for measuring Round-Trip time.

2. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

    | Connection                | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | N. Virginia - Oregon      |                |          |
    | N. Virginia - N. Virginia |                |          |
    | Oregon - Oregon           |                |          |

    > Region: US East (N. Virginia), US West (Oregon). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. All instances are `c5.large`. Note: Use public IP address when using iPerf within the same region.
