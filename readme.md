# Disclaimer
This repository is based on *spECK: Accelerating GPU Sparse Matrix-Matrix Multiplication Through Lightweight Analysis* (link TBA). Please cite this work if you use it for research.

# Getting Started
1. Install CUDA 10.1 or 10.2 from [https://developer.nvidia.com/cuda-downloads](https://developer.nvidia.com/cuda-downloads)
2. Download cub 1.8.0 from [https://nvlabs.github.io/cub/](https://nvlabs.github.io/cub/) and extract content into include/external
- wget https://codeload.github.com/NVlabs/cub/zip/1.8.0 
3. Install g++ > 7.0 and gcc on Linux or Visual Studio with "Desktop development with C++" workload
4. Install CMake 3.15.5 or newer from [https://cmake.org/](https://cmake.org/)
5. Set spECK_STATIC_MEM_PER_BLOCK and spECK_DYNAMIC_MEM_PER_BLOCK in include/Multiply.h line 9 & 10 to the values your hardware supports
    - spECK_STATIC_MEM_PER_BLOCK should be 49152 for all recent devices
    - spECK_DYNAMIC_MEM_PER_BLOCK should be 
        - 49152 for all devices before Volta and Turing
        - 65536 for Turing devices
        - 98304‬ for Volta devices
    - if you do not know your GPU generation or hardware limits, compile and run spECK and it will throw errors with information about the correct values
6. Build
    - Windows (use CMake GUI to setup project) or Build the project using "cmake -DCUDA_BUILD_CC70=TRUE -S . -B build -A x64" (Set CCXX to correct Compute Capability) followed by opening "runSpeck.sln", select "Release" configuration and build 
        -- run spECK using ".\build\Release\runspECK.exe <path-to-csr-matrix> config.ini"
    - Linux:
    1. Set the correct ComputeCapability (Default is CC70) in "linuxsetup.sh" and run "./linuxsetup.sh"
    2. run spECK using "./build/runspECK <path-to-csr-matrix> config.ini"


# Notes

spECK is compiled into a library "spECKLib.lib" and an executable "runspECK.exe" (or linux equivalents).
The executable comes with an .mtx (Matrix Market File Format) reader which converts the .mtx file into and saves an ".hicsr" binary file for faster runtimes.

runspECK takes 2 input parameters: 
- path to matrix in .mtx file format (required)
- path to config.ini (optional)

Config.ini contains helpful options for: 
- TrackCompleteTimes: enable/disable benchmarking
- TrackIndividualTimes: enable/disable benchmarking of all stages of speck (comes with performance overhead)
- CompareResult: enable/disable result matrix structure comparison with CUSPARSE. Prints an error message if column indices do not match
- IterationsWarmUp/IterationsExecution: set number of warm up and execution iteration for benchmarking. WarmUp is helpful to make sure that the GPU is running at it's highest clock rate
- InputFile: override input matrix - if an input file is defined in the config, this will override the first command line parameter


# Bibtex
https://ppopp20.sigplan.org/details/PPoPP-2020-papers/17/spECK-Accelerating-GPU-Sparse-Matrix-Matrix-Multiplication-Through-Lightweight-Analy
