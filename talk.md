class: center, middle

# How to improve job scripts for better resource usage

## (cores and memory)

## Radovan Bast ([@__radovan](https://twitter.com/__radovan))

---

## Goal

```bash
#!/bin/bash

#SBATCH --account=MyProject
#SBATCH --job-name=MyJob
#SBATCH --time=1-00:00:00
#SBATCH --mem-per-cpu=2G
#SBATCH --ntasks=16

set -o errexit        # exit the script on any error
set -o nounset        # treat any unset variables as an error
module --quiet purge  # clear any inherited modules

./myprogram < input.txt > output.txt
```

- This presentation is about the `--ntasks`, `--mem-per-cpu`, and `--time`.
- We will not talk about Slurm partitions.
- Many documentation pages (including [our documentation](https://documentation.sigma2.no))
  shows how to specify these but falls a bit short in showing how to find out good values.
- Documentation expects me to know whether my job uses MPI or OpenMP but how can I tell?

---

## Motivation/ learning outcomes

- Be able to tell what resources (.emph[memory, cores, time]) your job needs
- Understand why knowing the resource needs can be .emph[good for you] and for all other users

There will be few slides and a demo in the terminal where I will try some of this out.

---

## How we imagine that job scripts are prepared

- Taking some training
- Reading documentation
- Careful calibration and profiling
- Growing and benchmarking the model to meaningful parameters
- Then running the actual computations
- ... while monitoring resource usage from time to time

---

## How job scripts are often prepared

- Job scripts are often passed from generation to generation
- Tweaking until it does not crash
- Then running the actual computations
- If it crashes, contact support and/or check documentation

.quote["Why are you asking for 16 cores here?"]

.quote["-- I don't know, I got this script from my colleague"]

### Discuss possible problems

---

## Why it matters

### Memory

- Asking for too little cuts the job
- Asking for too much can mean that you block idle CPUs and get charged for them
- Asking for too much can mean a lot longer queuing


### Cores

- Asking for too few can lead to underused nodes or longer run time
- Asking for too too many can mean wasted CPU resources
- Asking for too much can mean a lot longer queuing


### Time

- Asking for too little cuts the job
- Asking for too much can mean a lot longer queuing and temporarily negative funds

---

## Few points which are often misunderstood

### None of this is expected to be obvious for a beginner

- We request resources from the scheduler (queuing system).
- But the scheduler cannot tell how long the job will run and what resources it
  will really consume.
- Just the fact that I am asking the scheduler for 40 cores
  .emph[does not mean that the code will actually run in parallel] and use all of them.
- .emph[Number of cores and amount of memory are not independent]. If you ask for more memory
  than is available on the number of cores, you will reserve and block more cores.
- Asking for a lot of memory "just to be on the safe side" can affect your queuing time
  and compute budget.

---

class: center, middle, inverse

# First time on a new machine?

## Please do not start immediately with a 16-node and 40-hour calculation

---

## How to grow your calculation

- Start with a short 5-minute run on 1 core
- Then try more cores on the same node
- Then try going beyond one node
- Then increase the system size and make your calculation longer

### Discuss the advantages of this approach

---

class: center, middle, inverse

# Demo time

---

## Demo (1/3): How much memory do I need?

- Several strategies are outlined in
  [How to choose the right amount of memory](https://documentation.sigma2.no/jobs/choosing_memory_settings.html)
- We will together try some of this
- If this is your own code and needs excessive memory perhaps contact us for
  [extended support](https://documentation.sigma2.no/getting_help/extended_support.html)?
- Also consider which queue/partition you submit to

---

## Demo (2/3): How many cores should we ask for?

- Timing a series of runs
- [Slurm browser](https://documentation.sigma2.no/jobs/monitoring.html)
- [Arm perf-report](https://documentation.sigma2.no/jobs/performance.html)


### Notes

- [Amdahl's law](https://en.wikipedia.org/wiki/Amdahl%27s_law)

---

## Demo (3/3): Example for a memory-bound job

Write me ...

---

class: center, middle, inverse

# Discussion time

---

## What is MPI and OpenMP and how can I tell? (1/2)

### If you wrote the software

- Then you probably know


### If it is written by somebody else

- It can be difficult to tell
- Consult manual for the software or contact support (theirs or ours)
- `grep -i mpi` and `grep -i omp` the source code


### Examples

- Write me ...

---

## What is MPI and OpenMP and how can I tell? (2/2)

### Python/R/Matlab

- Often not parallelized
- But can use parallelization (e.g. `mpi4py` or `multiprocessing`)


### Code may call a library which is shared-memory parallelized

- Example: BLAS libraries

---

## How to run many sequential tasks in parallel

- [ResearchSoftwareHour](https://researchsoftwarehour.github.io/)
  - [Session noes](https://researchsoftwarehour.github.io/sessions/rsh-008/)
  - [Examples](https://github.com/ResearchSoftwareHour/demo-parallel-tasks)
- [Array jobs](https://documentation.sigma2.no/jobs/job_scripts/array_jobs.html)
