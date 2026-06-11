---
layout: single
show_date: true
tags : pawsey assembly autocycler
title: Assembling genomes on Pawsey using Autocycler
author: Rob Edwards
---


[Autocycler](https://github.com/rrwick/Autocycler/wiki) is undoubtedly the best assembler, especially for Oxford Nanopore sequencing of bacterial genomes. You often get complete circular genomes.

We are going to use `autocycler` on Pawsey to assembled a genome!

We are really [following the instructions that Ryan Wick provided, and they are excellent](https://github.com/rrwick/Autocycler/tree/main/pipelines/Conda_environment_file_by_Ryan_Wick)

Before you begin, please [install conda and create an autocycler environment](https://fame.flinders.edu.au/blog/2026/06/11/pawsey_conda).

Once you have installed those commands, please run this command to update `plassembler`:

```
mamba activate  /scratch/$PAWSEY_PROJECT/$USER/software/miniconda3/autocycler
plassembler download -d "$CONDA_PREFIX"/plassembler_db
```

>Note, we also have an [autocycler installer](https://github.com/linsalrob/pawsey/blob/main/microbial_genome_annotation/autocycler_install.slurm) slurm script that does everything!

To run `autocycler` you need:

1. A fastq file with your sequences that you want to assemble.
2. The [autocycler run assmebly slurm script](https://github.com/linsalrob/pawsey/blob/main/microbial_genome_annotation/autocycler_run.slurm)

You should be able to run this with the command:

```
sbatch autocycler_run.slurm reads.fastq assembly
```

The assembly process will take a while, and you can see the outputs in the temporary directory. Check the error log for the location of those files.

Once complete, your output should be in `assembly`



