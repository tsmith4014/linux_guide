# NUMA and numactl: A Comprehensive Guide for Platform Engineers at High-Speed Trading Firms

## Table of Contents

- [Introduction](#introduction)
- [Understanding NUMA and numactl](#understanding-numa-and-numactl)
  - [What is NUMA?](#what-is-numa)
  - [Why is NUMA Important in High-Speed Trading?](#why-is-numa-important-in-high-speed-trading)
  - [What is numactl?](#what-is-numactl)
- [Using numactl](#using-numactl)
  - [Viewing NUMA Topology](#viewing-numa-topology)
  - [Running Applications with numactl](#running-applications-with-numactl)
  - [Binding Memory and CPU](#binding-memory-and-cpu)
  - [Default NUMA Policies](#default-numa-policies)
  - [Creating Custom NUMA Policies](#creating-custom-numa-policies)
- [Optimizing NUMA for Trading Environments](#optimizing-numa-for-trading-environments)
  - [Latency Reduction](#latency-reduction)
  - [Throughput Optimization](#throughput-optimization)
  - [Balancing CPU and Memory Loads](#balancing-cpu-and-memory-loads)
- [Best Practices](#best-practices)
  - [Application Profiling](#application-profiling)
  - [Testing and Validation](#testing-and-validation)
  - [Documentation and Collaboration](#documentation-and-collaboration)
- [Examples](#examples)
  - [Example 1: Binding an Application to a Specific NUMA Node](#example-1-binding-an-application-to-a-specific-numa-node)
  - [Example 2: Interleaving Memory Across NUMA Nodes](#example-2-interleaving-memory-across-numa-nodes)
- [Conclusion](#conclusion)
- [References](#references)

## Introduction

In the realm of high-speed trading, where every microsecond counts, optimizing system performance is essential. One critical aspect of performance tuning is managing how applications utilize the system’s memory architecture. Non-Uniform Memory Access (NUMA) systems are common in modern multi-processor servers, and understanding how to leverage NUMA can lead to significant performance gains.

This guide provides a comprehensive introduction to NUMA and the numactl tool, tailored for platform engineers working in high-speed trading environments. We will explore how to inspect NUMA topology, control application placement, and optimize system performance using numactl.

## Understanding NUMA and numactl

### What is NUMA?

Non-Uniform Memory Access (NUMA) is a computer memory design used in multiprocessing where the memory access time depends on the memory location relative to a processor. In NUMA architectures:

- **Local Memory**: Each CPU has its own local memory.
- **Remote Memory**: Memory attached to other CPUs, accessible over an interconnect.

**Key Characteristics**:

- **Faster Access to Local Memory**: CPUs access their local memory faster than remote memory.
- **NUMA Nodes**: Logical groupings of CPUs and memory banks.

### Why is NUMA Important in High-Speed Trading?

In high-frequency trading (HFT), applications demand:

- **Low Latency**: Rapid access to data and execution of trades.
- **High Throughput**: Ability to process large volumes of data quickly.

Optimizing NUMA configurations can lead to:

- **Reduced Memory Access Latency**: By ensuring that processes run on CPUs close to their memory.
- **Improved Cache Utilization**: Minimizing cross-node memory traffic enhances cache performance.
- **Enhanced Predictability**: Consistent performance by avoiding unexpected memory bottlenecks.

### What is numactl?

`numactl` is a command-line utility that allows you to control NUMA policy for processes or shared memory. It can:

- **Set CPU Affinity**: Bind processes to specific CPUs or NUMA nodes.
- **Set Memory Allocation Policy**: Control how memory is allocated across NUMA nodes.
- **Inspect NUMA Topology**: Display system NUMA configuration and statistics.

## Using numactl

### Viewing NUMA Topology

To understand how your system is configured, you can use `numactl` and related tools.

**Display NUMA Hardware Configuration**:

```sh
numactl --hardware
```

**Output Example**:

```
available: 2 nodes (0-1)
node 0 cpus: 0-15
node 0 size: 32768 MB
node 0 free: 16000 MB
node 1 cpus: 16-31
node 1 size: 32768 MB
node 1 free: 15000 MB
```

**Explanation**:

- **available**: Number of NUMA nodes.
- **node X cpus**: CPUs associated with node X.
- **node X size**: Total memory of node X.
- **node X free**: Available memory on node X.

**Show NUMA Maps of a Process**:

```sh
numastat -p <PID>
```

**Alternative**:

```sh
cat /proc/<PID>/numa_maps
```

### Running Applications with numactl

You can control how an application uses CPUs and memory.

**Basic Syntax**:

```sh
numactl [options] <command> [arguments]
```

### Binding Memory and CPU

**Run a Process on Specific NUMA Node**:

```sh
numactl --cpunodebind=<node> --membind=<node> ./application
```

- `--cpunodebind=<node>`: Bind CPUs to NUMA node.
- `--membind=<node>`: Allocate memory from NUMA node.

**Example**:

```sh
numactl --cpunodebind=0 --membind=0 ./trading_app
```

**Run a Process on Specific CPUs**:

```sh
numactl --physcpubind=<cpu-list> ./application
```

- `<cpu-list>`: Comma-separated list of CPUs (e.g., 0,2,4,6).

**Example**:

```sh
numactl --physcpubind=0-7 ./data_processor
```

### Default NUMA Policies

If no NUMA policy is specified, the kernel uses the default policy:

- **Local Allocation**: Memory is allocated from the node where the process is running.

### Creating Custom NUMA Policies

**Interleave Memory Allocation Across Nodes**:

```sh
numactl --interleave=all ./application
```

- **Interleave**: Distributes memory allocations evenly across all NUMA nodes.

**Preferred Node for Memory Allocation**:

```sh
numactl --preferred=<node> ./application
```

- Allocates memory from the preferred node if possible.

## Optimizing NUMA for Trading Environments

### Latency Reduction

**Minimize Remote Memory Access**:

- **Strategy**: Ensure that processes access local memory as much as possible.
- **Implementation**:
  - Bind latency-sensitive applications to specific NUMA nodes.
  - Use `--cpunodebind` and `--membind` options.

**Example**:

```sh
numactl --cpunodebind=1 --membind=1 ./low_latency_app
```

### Throughput Optimization

**Utilize All NUMA Nodes**:

- **Strategy**: Distribute workloads across all nodes to maximize resource usage.
- **Implementation**:
  - Run multiple instances of applications bound to different nodes.
  - Interleave memory for applications that benefit from higher memory bandwidth.

**Example**:

```sh
numactl --interleave=all ./high_throughput_app
```

### Balancing CPU and Memory Loads

**Avoid Overloading a Single Node**:

- **Monitor CPU and memory usage per NUMA node**.
- **Adjust process bindings to balance the load**.

**Tools**:

- `numastat`: Displays NUMA memory allocation statistics.
- `htop` or `top`: With NUMA-aware configuration.

## Best Practices

### Application Profiling

- **Profile Applications**: Understand memory and CPU usage patterns.

**Steps**:

1. **Monitor Performance**:

   ```sh
   numastat -p <PID>
   ```

2. **Identify Bottlenecks**:

   - High remote memory accesses.
   - Imbalanced CPU utilization.

3. **Adjust NUMA Policies**:
   - Use `numactl` options to optimize placement.

### Testing and Validation

- **Test in a Controlled Environment**: Before applying changes to production.
- **Benchmark Performance**:
  - Use relevant metrics (latency, throughput).
  - Compare before and after NUMA optimizations.
- **Iterative Tuning**:
  - Gradually adjust settings.
  - Monitor impacts at each step.

### Documentation and Collaboration

- **Document NUMA Configurations**:
  - Record the reasoning behind settings.
  - Include in system documentation.
- **Collaborate with Developers**:
  - Understand application requirements.
  - Align NUMA policies with application behavior.
- **Automate Deployment**:
  - Use scripts or configuration management tools to enforce NUMA policies.

## Examples

### Example 1: Binding an Application to a Specific NUMA Node

**Scenario**: You have a latency-critical trading application that should run on NUMA node 0 to minimize memory access latency.

**Steps**:

1. **Determine NUMA Nodes and CPUs**:

   ```sh
   numactl --hardware
   ```

   **Output**:

   ```
   available: 2 nodes (0-1)
   node 0 cpus: 0-15
   node 1 cpus: 16-31
   ```

2. **Run the Application Bound to Node 0**:

   ```sh
   numactl --cpunodebind=0 --membind=0 ./trading_app
   ```

3. **Verify CPU Affinity**:

   ```sh
   taskset -pc $(pgrep trading_app)
   ```

   **Output**:

   ```
   pid 12345's current affinity list: 0-15
   ```

4. **Monitor Memory Access Patterns**:

   ```sh
   numastat -p $(pgrep trading_app)
   ```

   **Output**:

   ```
   Per-node process memory usage (in MBs):
   PID     Node 0      Node 1
   12345   5000        0
   ```

### Example 2: Interleaving Memory Across NUMA Nodes

**Scenario**: You have a data processing application that benefits from high memory bandwidth and can tolerate slightly higher latency.

**Steps**:

1. **Run the Application with Memory Interleaving**:

   ```sh
   numactl --interleave=all ./data_processor
   ```

2. **Verify Memory Allocation**:

   ```sh
   numastat -p $(pgrep data_processor)
   ```

   **Output**:

   ```
   Per-node process memory usage (in MBs):
   PID     Node 0      Node 1
   67890   2500        2500
   ```

3. **Monitor Performance**:
   - Use application-specific metrics to assess throughput.
4. **Adjust if Necessary**:
   - If performance is not optimal, try binding to a single node or adjusting interleaving.

## Conclusion

Optimizing NUMA configurations is a vital aspect of performance tuning in high-speed trading environments. By leveraging `numactl`, platform engineers can control how applications utilize system resources, reducing latency and maximizing throughput.

**Key takeaways**:

- **Understand Your System’s NUMA Topology**: Use `numactl --hardware` and `numastat`.
- **Control Application Placement**: Use `numactl` options to bind processes to specific CPUs and memory nodes.
- **Profile and Monitor Applications**: Continuously assess the impact of NUMA policies on performance.
- **Collaborate and Document**: Work with development teams and maintain clear documentation of configurations.

By following best practices and employing careful tuning, you can significantly enhance system performance, directly impacting the efficiency and success of high-frequency trading operations.

## References

- **numactl Manual Page**: `numactl(8)` - Linux manual page
- **NUMA Documentation**: Linux Kernel NUMA Memory Policy
- **Performance Tuning Guides**:
  - Red Hat Enterprise Linux Performance Tuning - NUMA
  - Intel Optimizing Applications for NUMA

**Note**: The examples and configurations provided in this guide are intended as starting points. System behavior can vary based on hardware, kernel versions, and application characteristics. Always thoroughly test NUMA policies in a controlled environment before deploying them to production systems.
