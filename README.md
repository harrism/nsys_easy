# nsys_easy

This script is a wrapper around the `nsys` command to simplify the profiling process with NVIDIA NSight Systems.
It allows you to specify the trace, sample, and context switch options, as well as the output and report names.
The script will run the nsys profile command and then the nsys stats command.

Usage: 

Put `nsys_easy` somewhere in your `$PATH`.

```bash
nsys_easy [-t trace] [-s sample] [-c ctxsw] [-o output] [-r report] command
```

Example: 

```
nsys_easy -t cuda,osrt -s none -c none -o nsys_easy -r cuda_gpu_kernel_sum ./my_program
```

The above command will profile the my_program executable with the specified options and output files.

By default the script only traces CUDA API calls, but you can specify additional options to trace other events.
The script runs the cuda_gpu_sum report by default, which combines Kernel and CUDA memory copy statistics.
The goal is for the script to act similar to the nvprof command, but with the added flexibility of the NVIDIA Nsight Systems tool.

## Prerequisites

 - [NVIDIA NSight Systems](https://developer.nvidia.com/nsight-systems)
 - Bash or other shell that supports `getopts`

For full `nsys` command line options, see the [NSight Systems User Guide](https://docs.nvidia.com/nsight-systems/UserGuide/index.html).
