---
title: "Pixi, Conda's Cooler and Faster Cousin."
date: 2025-12-12
categories: [Bioinformatics Tooling]
tags: [Learning]
---

## TL;DR

**Pixi is a blazing-fast, project-based package and environment manager built by the creators of Mamba. I can't believe I didn't switch sooner, it is literally a game-changer.**

## My 3 Major Qualms with Conda

Before I crap on conda, I want to give it its flowers.

Conda was and absolute trailblazer in the package management space. It was arguably the first *reliable* package manager to be widely adopted for cross-language, cross-platform package management and literally shaped an entire generation of data science and bioinformatics. I will still end up using conda for *some* tasks but, like anything, conda has its share of shortcomings and for me, the list of cons finally outweighed the pros. It was time to make a switch. 

If you are in the data science world in any capacity, you've probably heard the buzz about [Pixi](https://pixi.sh/latest/), a package manager from the developers at Prefix (the same group that brought us Mamba). Personally, I have never really enjoyed using conda. It was just something myself, like many others, *had* to deal with. As a bioinformatics core facility, my team and I are often running a wide variety of analyses that span multiple languages and tools. Conda environments extend beyond just Python, which means we end up creating and maintaining *lots* of environments. 

Why is this painful?

1.	**Conda shoves every environment into one giant global trash pile.**

Different projects need slightly different versions of the same package? You now have redundant, bloated, multi gigabyte Frankenstein environments stacked on top of each other.
And because everything shares the same global directory, environment drift is practically guaranteed.
	
2.	**You end up with a graveyard of environments, and you’re always in the wrong one.**

A single project can spawn an ungodly number of environments.
Documentation becomes chaos, provenance becomes guesswork, and every debugging session begins with the sacred ritual of: “Wait… am I even in the right conda env?” (Spoiler: you’re not.)

3.	**Conda environment solves are slow enough to qualify as geological events.**

Even with faster solvers like Mamba, you still end up starting an environment build, going home, sleeping, hoping and praying it solves by the morning. And if it doesn’t...back to square one, because conda has no respect for your time, your deadlines, or your sanity. (This is not *always* the case, but more times than not, your environment is pretty beefy and will take a substantial amount of time.)


## Switching to Pixi: The "Why and How?"

Reproducibility. Is. Paramount. 

Unfortunately with conda, shared global environment storage *almost guarantees* environment drift. And with that, comes headaches. 

Picture this scenario:

You just finished a single-cell project that uses Packages A, B and C. Your `singleCell.yaml` is placed in the github repository, everything is documented, the client gets their results and everyone is happy (ominous foreshadowing). 4 months later, a team member uses the same environment for a different project, but adds package D. The problem? Package D conflicts with the Package C. Conda “fixes” this by reshuffling dependencies: downgrading some, upgrading others, quietly mutating the environment that produced your results. 
The following week, your client comes to you and asks to re-run an analysis with different parameters. Now Package C behaves differently. Functions changed, arguments shifted, objects have a new structure. Your workflow breaks. What’s your only recourse? Rebuild the environment from scratch, wasting time and HPC storage while conda churns away at yet another painfully slow solve. 

Pixi eliminates this with a project-specific environment paradigm. Drift is near impossible without explicitly updating the project’s lock file or `toml` (synonymous to a conda yaml).  Side not:, if you've ever worked with `renv` in R, you know how useful a lockfile can be for reproducibility, *especially* when working with collaborators. A lockfile records every detail of your environment from package versions, platforms, hashes, SHA-codes, dependency trees, and the whole nine-yards. And the best part: Pixi keeps these automatically synced. Add a dependency? Your `.toml` and `.lockfile` update instantly.

The result is an environment that is *fully* reproducible. Anywhere. Anytime. On any machine (by any user? Well, that part isn't guaranteed.)

*“But Mike, doesn’t Pixi waste space the same way? Aren’t you just putting multiple envs in different folders instead of one?”*

Don't worry...I asked myself that same question. This is the other elegant feature of Pixi. It *doesn't* install every tool again and again in a different location. Each environment in Pixi is isolated, but instead of creating separate copies of packages, they are installed to a central cache that pixi then reuses (via symlinks) in your project. This means the individual files are stored *only once* on disk, even if multiple environments use the same package version. 

Why is this feature amazing? Not only does this greatly reduce storage space, but environment solves become *absurdly* faster (up to three orders of magnitude faster than conda in some cases). And less importantly, from an aesthetics standpoint, the build progress bars just look cleaner...no more long strings of pound-symbols assaulting your terminal window.

On top of this, the `toml` is far more powerful than a conda `yaml`. Each `toml` has a `tasks` section. Each task can add additional dependencies or execute a script or command. This is an extremely elegant way of defining different environments for one project and running reproducible tasks. No more activating environments. No more guessing which tool versions you installed. No more `bash run_all.sh` spaghetti code. Just clean, reproducibile data science.

## The Drawbacks of Pixi

Pixi is fantastic, but it is not without some rough edges. The biggest limitation (especially for bioinformaticians) is that Pixi *does not* natively support Bioconductor packages for R. (Python-biased bioinformaticians are laughing at us). For R/Bioconductor heavy workflows (think Single cell projects with the `Seurat` or `SingleCellExperiment` ecosystems), this matters *a lot.*

CRAN packages are covered (thankfully), but many core bioinformatics tools live on Bioconductor. The good news: Pixi *does* support bioconda and the community has packaged a growing subset of Bioconductor tools under that namespace. For those, you can simply declare `bioconda` as a channel and then specify the dependency as `bioconductor-packagename` (by convention, Bioconda package names will always be all lowercase, and *yes* it is confusing that the bioconda channel uses "bioconductor" as a prefix...I don't make the rules, I just follow them.) 

The second pain point for me personally is facilitating Pixi with R projects that live on a remote server like an HPC. My workflow for this involves working locally in RStudio while mounting the HPC file system under `/Volumes/` (for instructions on how to do this, see [this link](https://support.apple.com/guide/mac-help/connect-mac-shared-computers-servers-mchlp1140/mac)). I want to emphasize, I say "pain-point" loosely. It's not really a pain, it just requires a bit of additional work up front, but it's all in the name of reproducibility. 

1. Create the pixi environment **locally**

2. Point Pixi tasks to a .Rproj living on your HPC or remote connection

3. You can now run RStudio locally with your project and HPC paths are mounted. Changes are reflected on your HPC.

4. Copy the `toml` and `lock` file to the HPC or github repo for your project when you're done.

It's not a deal breaker, just mildly clunky...but it still gets the job done!

## A Real World Example

**A Python Focused Project on VSCode, Directly on an HPC**

I've essentially integrated Pixi into nearly all of my projects. Python-based tooling works out of the box and VSCode integration is effortless. Below is a real example from one of my on-going projects where I needed to leverage [`multivelo`](https://github.com/welch-lab/MultiVelo) for a multiome velocity inference. The `kernel` task allows me to set a Jupyter Kernel so I can run my python code interactively directly on VSCode. 

```
[workspace]
authors = ["Mike Martinez <f007qps@dartmouth.edu>"]
channels = ["conda-forge", "bioconda"]
name = "multivelo"
platforms = ["linux-64"]
version = "0.1.0"

[tasks]
kernel = "python -m ipykernel install --user --name multivelo-pixi --display-name 'Python (MultiVelo Pixi)'"

[dependencies]
python = "3.11.*"
pip = "*"
ipykernel = "*"
multivelo = "*"
loompy = ">=3.0.8,<4"
```

**An R Focused Project Using Local RStudio Mounted to the HPC**

Here's another example of one of my projects (a basic differential expression analysis). Here I created the environment on a folder locally and pointed the task to open RStudio (locally) with my `.Rproj` file that lives on the HPC. Several bioconductor packages were needed so I pointed the dependencies to the `bioconda` versions of these tools. I also needed to install my tool, `RGenEDA` from github, so I set that as a task. (By the way, shameless plug incoming... if you haven't heard of [`RGenEDA`](https://github.com/mikemartinez99/RGenEDA), my R package for reproducible genomic exploratory data analysis, go check it out!)

```
[workspace]
authors = ["Mike Martinez <f007qps@dartmouth.edu>"]
channels = ["conda-forge", "bioconda"]
name = "envs"
platforms = ["osx-arm64","linux-64"]
version = "0.1.0"

[tasks]

# Run RStudio project
rstudio = "open -n -a Rstudio /Volumes/GMBSR_bioinfo/Labs/Smith/251201-Smith-DEGs/251201-Smith-DEGs.Rproj"

# Install github packages
install-git = """
Rscript -e '
devtools::install_github("mikemartinez99/RGenEDA")
'
"""

[dependencies]
r-base = "4.4.1"
r-sessioninfo = "*"
r-remotes = "*"
r-devtools = "*"
r-tidyverse = "*"
r-dplyr = "*"
r-tidyr = "*"
"r-data.table" = "*"
r-pheatmap = "*"
r-ggplot2 = "*"
r-ggpubr = "*"
r-ggrepel = "*"
r-RColorBrewer = "*"
r-ggh4x = "*"
r-MatchIt = "*"
bioconductor-deseq2="*"
bioconductor-apeglm="*"
bioconductor-clusterprofiler="*"
bioconductor-annotationdbi="*"
"bioconductor-org.hs.eg.db"="*"
```

So if it isn't abundantly clear where I stand on Pixi, I'll leave you with this: Life is short. Dependency Hell will make it shorter. Add Pixi to your tool box. 

**Compute and Conquer!**


