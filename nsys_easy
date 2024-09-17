#!/bin/bash

# This script is a wrapper around the nsys command to simplify the profiling process.
# It allows you to specify the trace, sample, and context switch options, as well as the output and report names.
# The script will run the nsys profile command and then the nsys stats command.
# Usage: nsys_easy.sh [-t trace] [-s sample] [-c ctxsw] [-o output] [-r report] command
# Example: nsys_easy.sh -t cuda,osrt -s none -c none -o nsys_easy -r cuda_gpu_kernel_sum ./my_program
# The above command will profile the my_program executable with the specified options and output files.
#
# By default the script only traces CUDA API calls, but you can specify additional options to trace other events.
# The script runs the cuda_gpu_sum report by default, which combines Kernel and CUDA memory copy statistics.
# The goal is for the script to act similar to the nvprof command, but with the added flexibility of the NVIDIA Nsight Systems tool.

# Default values
trace="--trace=cuda"
sample="--sample=none"
ctxsw="--cpuctxsw=none"
output="nsys_easy"
report="cuda_gpu_sum"

# Usage function
usage() {
    echo "Usage: $0 [-t trace] [-s sample] [-c ctxsw] [-o output] [-r report]"
    echo "  -t trace   : Set the trace option (default: --trace=cuda)"
    echo "  -s sample  : Set the sample option (default: --sample=none)"
    echo "  -c ctxsw   : Set the context switch option (default: --cpuctxsw=none)"
    echo "  -o output  : Set the output file name (default: nsys_easy)"
    echo "  -r report  : Set the report name (default: cuda_gpu_sum)"
    exit 1
}

# Parse command line options
while getopts "t:s:c:o:r:" opt; do
    case $opt in
        t) trace="--trace=$OPTARG" ;;
        s) sample="--sample=$OPTARG" ;;
        c) ctxsw="--cpuctxsw=$OPTARG" ;;
        o) output="$OPTARG" ;;
        r) report="$OPTARG" ;;
        \?) usage ;;
    esac
done

# Shift the processed options away
shift $((OPTIND -1))

# Run the nsys profile command
nsys profile $trace $sample $ctxsw --force-overwrite=true -o $output "$@"

# Run the nsys stats command
nsys stats --force-export=true -r $report $output.nsys-rep