---
layout: single
show_date: true
title: "The 
author: Robert Edwards
excerpt: 
---
![](/assets/images/singularity.png)


# Vibe coding, vibe analysing, and the singularity.

Recently, we've been vibe coding bioinformatics projects and vibe analysing bioinformatics data. Have we reached the singularity yet?

## Vibe coding bioinformatics

We've been writing bioinformatics tools for over 20 years, but over the last year, vibe coding has completely changed what we do, and how we do it. 

A typical bioinformatics tool that we'd write previously, was predominantly written in Python (or back in the day, in Perl), and maybe had a bit of C or C++ to make things go faster.

Nowadays, with vibe coding, we are no longer limited to a single language. We can seamlessly integrate C++, Python, node.js, Rust, Typescript, JavaScript, Svelte, and a host of other languages.


For example, here are a few of our recent projects:

- [agviz](https://linsalrob.github.io/agviz/) for visualisation of assembly graphs.
- [genbank viewer](https://linsalrob.github.io/genbank_viewer/) to visualise GenBank  files in your browser. This project uses Rust, TypeScript, Svelte, and JavaScript
- [genbank_to](https://linsalrob.github.io/genbank_to/) converts GenBank files from one format to another. 
- [genome entropy](https://linsalrob.github.io/genome_entropy/) to compare the entropies of DNA, amino acids, 3Di encodings, and 12-state encodings. This project uses Python, Jupyter Notebook, Shell, C, and Makefiles
- [oligo designer](https://linsalrob.github.io/OligoDesigner/) to permute and design random oligos and structured oligos. This project is primarily Python, but also uses javascript and HTML. 
- [PhiSpy web](https://linsalrob.github.io/PhiSpyWeb/) for in-the-browser identification of prophages. This project uses TypeScript, CSS, JavaScript, and HTML, although of course PhiSpy is written in Python and C and is exposed via pyodide.

To build these projects, we use an agentic workflow, generally relying on [codex](https://openai.com/codex/) to do most of the work for us. 

Installation of codex is straightforward:

```bash
curl -fsSL https://chatgpt.com/codex/install.sh | sh
codex auth login
codex
```

Partly because we are working on a machine which automatically purges untouched documents after 21 days, and partly because it is generally best practice, especially for code, we always work in GitHub repository.

We created an [AGENTS.md](https://github.com/linsalrob/EdwardsLab/blob/master/VibeCoding/AGENTS.md) markdown file which resides in our `~/.codex/` directory, and puts some boundaries on codex, although this also authorises it to do a lot of work to the git repository.

For each project, we define the expected goals and required outcomes, and work in small steps. A common prompt that we use now is something like:

```
I have merged this PR. Please checkout main and update it, delete this local branch, and then identify the next milestone along the development pathway. Implement the steps to achieve that milestone, commit them, and make a PR. Once you have made the PR wait for the reviews to complete, and then pull the reviews and comments. Fix any outstanding comments, resolve the threads, and then push the updated changes back to the PR.
```

With this prompt, we are getting codex to basically do all the work. Of course, the key is making sure that the milestones are tightly defined, with appropriate test cases if possible, and that codex doesn't wander down a rabbit hole. You need to keep an eye on that.

## Vibe analysing

Based on our experience with vibe coding, we started vibe analysing data. We start with a similar structure, but because we are analysing data, we don't often work in a git repository, because git can't handle large datasets easily (there is a work around with `git-lfs` but its not really suitable for metagenome scale data).

Nonetheless, the `git` structure provides a lot of advantages, especially when working with agentic code, so we create a local git repository using `git init` and then we can add some of our code to the repository. Later, if we want, we can upload that to GitHub.

We also use an [AGENTS.md](https://github.com/linsalrob/AI_Data_Analysis/blob/main/AGENTS.md) markdown file to define what the agent should be doing, but here our target is not to make code, but to answer the scientific questions defined in [SCIENTIFIC_BRIEF.md](https://github.com/linsalrob/AI_Data_Analysis/blob/main/SCIENTIFIC_BRIEF.md). This example is one we are using to analyse a phage genome. We combine those two with an [ANALYSIS_PLAN.md](https://github.com/linsalrob/AI_Data_Analysis/blob/main/ANALYSIS_PLAN.md) which translates the scientific brief into actionable items.

We provide a blank DECISIONS.md and STATUS.md that the agent can append to as it is developing its solutions to our problem. 

Here is an overview of the primary files we have:

| File | Question it answers | How Codex uses it |
| --- | --- | --- |
| `AGENTS.md` | How must the agent behave while doing the work? | Supplies persistent operating rules: scientific validity first, source data read-only, explicit assumptions, durable evidence, and reporting obligations. Codex reads this automatically when working in the directory. |
| `SCIENTIFIC_BRIEF.md` | What are we trying to discover, and why? | Establishes hypotheses, scientific questions, available evidence, scope, non-goals, and success criteria. |
| `ANALYSIS_PLAN.md` | What evidence and analyses could answer those questions? | Provides phases, controls, outputs, decision gates, validation criteria, and stop conditions. |
| `DECISIONS.md` | Why were consequential methodological choices made? | Preserves choices about thresholds, exclusions, models, statistical methods, and changes of direction. |
| `STATUS.md` | What is currently known, uncertain, blocked, or next? | Acts as concise scientific working memory across sessions. It should reflect evidence, not merely task completion. |
| `README.md` | How should a person adopt and operate this template? | Provides the human-facing entry point. |

In addition, we use an immutable `input/` directory with data that the agent should start with and should, under no circumstances, change. We have a mutable `results/` directory where the agent can write new data, analyses, or outputs.

We keep several directories for additional outputs: `report/` contains a markdown written report and the figures directory contains images that are linked into the report, while `notebooks` contains a growing collection of Jupyter notebooks used to make those figures. We use `scripts/` for additional code that the agent needs to analyse the data, and a `work/` directory to give the agent a space to work in.

Overall, our directory structure looks something like this:



```text
├── inputs
│   ├── Metadata
│   ├── MGI
│   ├── ONT_MinION
│   └── ONT_PromethION
├── notebooks
├── report
│   └── figures
├── results
│   ├── atlas
│   ├── bam
│   ├── coverage
│   ├── flagstat
│   ├── sample_qc
│   └── stats
├── scripts
│   └── __pycache__
├── SRA
│   ├── fastq
│   ├── mapping
│   ├── sra_cache
│   └── work
└── work
    ├── fastq
    ├── longread_assembly
    └── recruit_assembly
```

This is probably how you would end up with a directory if you were doing the analysis yourself, but I know from experience, usually you start with everything in one directory, and only when you accumulate enough cruft, do you begin to move things around! With vibe analysing, it`s easy to accumulate so many files, and better to get some organisation going from the beginning.

# So what about the singularity?

The [technological singularity](https://en.wikipedia.org/wiki/Technological_singularity) is, as Vernor Vinge put it in 1993[^1], the [creation by technology of entities with greater than human intelligence](https://mindstalk.net/vinge/vinge-sing.html). Vernor points out, in that essay, that he'd be surprised if the singularity occurs before 2005 or after 2030.

I don't think we're at the singularity yet, but we are really, really close. I think we're living in the transition period, where the singularity it upon us, and at some point &em; which we may not recognise &em; it will have passed us!

For _vibe coding_, we don't write code any more. All of the code is written by AI. The user doesn't need to know the language, or necessarily care about the language [^2] to write effective (but not necessarily efficient) code. We are in the singularity but not through it, otherwise the AI would know the next questions and milestones, and be already coding towards them before I've even realised that we are done with the last milestone.

For _vibe analysis_, we are also close. With a single prompt, I can get AI to go search the [SRA](https://www.ncbi.nlm.nih.gov/sra/) use [fasterq-dump](https://edwards.flinders.edu.au/fastq-dump/) to download the sequences (based, presumably, on my blog posts), and compare them to genomes of interest. All this is hands-off, and the AI and running the analysis. _But_, it's not there designing the analysis yet. It doesn't ask the interesting questions.

However, AI is helping with the interesting questions. It is providing quicker ways to get to the interesting questions, and helping me filter out the less interesting questions. It's downloading the data, processing the data, and letting me spend time thinking about the results. We are not AI, but we are definitely at in the midst of Intelligence Amplification &em; IA[^3].

Have we passed the singularity? No, not yet. Is it close? Definitely, it feels like the last year or two it has got so much closer.

When will the singularity arrive? Soon.



[^1]: Vernor was also a Professor at San Diego State University, and when I was recruited there as a lowly Assistant Professor, I inherited Vernor's office. Alas, his brilliant writing was not, apparently, caused by the office. 
[^2]: I believe this is true, but it is different from not knowing or caring about computer science fundamentals. For example, I need to remind AI to not use a _O_(n**2**) algorithm from time to time.
[^3]: This post wasn't written by AI but the image was generated by AI.

