# Fourier Space Image Similarity Optimization (AVX2 + OpenMP)

> **Note:** This repository contains a student technical report for a university coursework project. It is shared for educational and demonstration purposes only.

Optimized **Fourier-space image similarity computation** used in cryo-electron microscopy (cryo-EM) by combining **data structure redesign**, **manual load balancing**, and **AVX2 vectorization** in C++.  
The optimized version achieves an average **5× speedup** compared with the baseline OpenMP implementation, demonstrating how low-level SIMD programming and multi-thread optimization can accelerate scientific computing workloads.

---

## 🧩 Overview

Cryo-EM 3D reconstruction requires computing image similarity in Fourier space for thousands of projections — a task that is computationally intensive.  
This project proposes and benchmarks a series of optimizations to overcome the major CPU bottlenecks:

| Optimization                | Description                                                  | Impact |
| --------------------------- | ------------------------------------------------------------ | ------ |
| **Data Structure Redesign** | Split complex arrays into real/imag parts and use single-precision floats for improved cache locality. | ~1.8×  |
| **Manual Load Balancing**   | Evenly distribute workloads across CPU threads to avoid OpenMP imbalance. | ~2.3×  |
| **AVX2 Vectorization**      | Replaced scalar arithmetic with AVX intrinsics (`_mm256_fmadd_ps`, `_mm256_fnmadd_ps`) to process 8 floats per instruction. | ~5.0×  |

---

## ⚙️ Features

- 🚀 **AVX2 Intrinsics**: SIMD vectorization for fused multiply-add pipelines  
- 🧵 **OpenMP Parallelism**: Explicit control over thread-level workload  
- 🧮 **Fourier-Space Similarity Calculation**: Efficient distance computation for image sets  
- 📊 **Benchmark Results**: Speedup sustained across increasing dataset sizes (10k–100k images)

---

## 🧠 Methodology

1. **Baseline:** Serial implementation using double-precision complex numbers.  
2. **Data Optimization:** Replaced complex objects with real arrays (`dat0`, `dat1`, `pri0`, `pri1`).  
3. **Parallelization:** Used OpenMP with manual workload partitioning (`begin[]`, `end[]`).  
4. **Vectorization:** Introduced AVX2 intrinsics to handle 8-way SIMD operations.  

Example kernel fragment:
```cpp
__m256 tmp1 = _mm256_fnmadd_ps(ctfVec, pri0Vec, dat0Vec);
__m256 tmp2 = _mm256_fnmadd_ps(ctfVec, pri1Vec, dat1Vec);
__m256 tmpSum = _mm256_fmadd_ps(tmp1, tmp1, _mm256_mul_ps(tmp2, tmp2));
localSum = _mm256_fmadd_ps(tmpSum, sigRcpVec, localSum);
```

---

## 🧪 Experimental Setup

| Spec     | Value                        |
| -------- | ---------------------------- |
| CPU      | Intel i9-13900HX (16C/32T)   |
| Memory   | 16 GB DDR5                   |
| OS       | Ubuntu 20.04 LTS             |
| Compiler | GCC 9.4.0 (AVX2, OpenMP 4.5) |

**Average Speedup:** 5.01× across datasets from 10k – 100k images.
**Consistency:** Variance < 10% across all workloads.

---

## 📂 Repository Structure

```
/src
 ├── main.cpp              # core Fourier similarity algorithms
 ├── avx_opt.cpp           # AVX2-optimized implementation
 ├── openmp_opt.cpp        # manual load-balanced version
 ├── benchmark.cpp         # test harness and timing
/report
 ├── report.pdf            # full technical report (LaTeX + IEEE format)
 └── Figure1.png           # performance graph
```

---

## 🧾 Results Summary

![Speedup Chart](report/Figure1.png)

The AVX2-optimized algorithm consistently outperforms the baseline OpenMP version,
with the highest gains observed for medium-scale datasets (20k–50k images).

---

## 🔬 Educational Value

This project demonstrates:

* Efficient **SIMD and multi-thread cooperation** on modern CPUs
* **Manual load balancing** and data-level parallelism in HPC
* Practical understanding of **compiler optimization, cache locality**, and **instruction-level parallelism**

---

## 🧰 Tech Stack

**Languages:** C++, OpenMP
**Technologies:** AVX2 Intrinsics, Linux, GCC, Make
**Category:** High-Performance Computing, Parallel Programming, Cryo-EM Image Processing

---

## 📄 Citation

> Zhang, Ian. *Application of AVX Instruction Set in Fourier Space Similarity Calculation.*
> Renmin University of China, 2023. Technical Report.

---

## 🧑‍💻 Author

**Yangyang (Ian) Zhang**
M.Eng. in Computing & Software, McMaster University
📧 [zhang787@mcmaster.ca](mailto:zhang787@mcmaster.ca) · 🌐 [LinkedIn](https://linkedin.com/in/) · [GitHub](https://github.com/)

