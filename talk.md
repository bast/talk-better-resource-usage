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
  shows how to specify these .emph[how do we know what values to use?]
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


### Discussion

- What are the advantages of this approach?
- What are possible challenges of this approach?

---

## How to grow your calculation

.quote["But my systems are big to start with!"]

.quote["-- Try to make them artificially smaller."]

.quote["But then it's scientifically not meaningful!"]

- To calibrate the computation parameters the scientific results do not have to be meaningful
- This can still be meaningful for the calibration
- Don't be afraid to create fantasy datasets to test the scaling

---

## Create a small example for you

- Something that runs in 5 minutes
- Where you know the result and the timing


### Next time you are unsure whether it's the machine or "you"?

- Run the small example
- If it suddenly stopped working or runs slower, you know it's probably the machine
- Now you can send us the example so that we fix the machine

---

class: center, middle, inverse

# Demo time

---

## Demo (1/2): How much memory do I need?

- Several strategies are outlined in
  [How to choose the right amount of memory](https://documentation.sigma2.no/jobs/choosing-memory-settings.html)
- We will together try some of this
- If this is your own code and needs excessive memory perhaps contact us for
  [extended support](https://documentation.sigma2.no/getting_help/extended_support.html)?
- Also consider which queue/partition you submit to

---

## Demo (2/2): How many cores should we ask for?

- Several strategies are outlined in
  [How to choose the numer of cores](https://documentation.sigma2.no/jobs/choosing-number-of-cores.html)
- We will together try some of this
- Do not hesitate to contact
- [support](https://documentation.sigma2.no/getting_help/support_line.html) if
- you are unsure about how many cores/tasks/threads to ask for.
