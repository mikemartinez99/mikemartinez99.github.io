---
title: "The Pillars of a Sane Project"
date: 2026-01-12
categories: [Bioinformatics]
tags: [Learning]
---

## TL;DR

Future-you doesn't have the memory present-you thinks you have.

✅ Use Git / GitHub

✅ Develop an organizational style, and keep it consistent

✅ Document. Document. Document. 

## Who this post is for

This post is for early-career bioinformaticians, bench biologists who want to take their data into their own hands, analysts inheriting legacy projects, or anyone who has ever opened a directory and audibly said "what the hell am I looking at?"


## The Pipeline That Had No Past

I'll start this blog off with every bioinformatician's nightmare. You are a bright-eyed and bushy-tailed bioinformatician at your first job. You are ready to write code, dive into data, and obtain everyone's favorite buzz words: "actionable insights". Your first project comes...your boss gives you a path to a folder. You look, full of excitement only to realize you're inheriting a project riddled with red flags that has been passed down collaborator to collaborator, all of which have since left. The data type is a more-or-less obscure technology with relatively little online resources to guide you. The project is scattered across multiple directories...a plethora of files whose names mean nothing...slightly different iterations of the same scripts, with zero comments and no README explaining what the code does (or which one was *~the~* code used for the actual analysis). 

(Side note: I was the person in this horror story, and I consider it to be my "rite of passage" into the field...or perhaps some cruel form of hazing early-career bioinformaticians, I don't know.)

For a few months, my job title really should have been "Detective" because I spent more time piecing together jigsaw pieces of an insane puzzle than actually coding, or analyzing anything. The ironic part is, this could have all been avoided. But, this nightmare had a major silver lining: it taught me everything I *didn't* want my code to be. 

## The Essentials

Let's make one thing clear: *you will not remember what you did 3 months down the line. Maybe not even 3 weeks.*

When I came back from Christmas vacation this year, it took me a good while to get back up to speed with where I was before the holidays...and I had things documented to the n-th....imagine if I did not? There are many factors that go into making a project "sane", reproducible, and digestible, but I'll touch on what I consider to be the 3 pillars of a sane project: *version control*, *organizational style*, and *documentation*.

**Version Control**

Version control platforms like Git and GitHub create a paper trail for your project. By pushing your code additions and modifications to a remote repository you now have a record of not only *what* was changed, but *when* it was changed, and crucially, *why* it changed. (You also have a backup of all your work in case something happens to your computer, or if you just want to go back in time to a previous iteration of something!) At the very start of my career, I found Git incredibly intimidating. For some reason, I had established in my mind that only hardcore software engineers used it. Not only was I grossly mistaken but I was pleasantly surprised to realize that GitHub Desktop made version control painless (most of the time). Git and GitHub are rich with features, some more complex than others, but at the end of the day, as long as you know how to pull, push, and commit...this is really all you need. [This tutorial](https://docs.github.com/en/get-started/start-your-journey/hello-world) from GitHub should get you started!

I like to keep my GitHub repositories consistently formatted so that there is never ambiguity about what something is. This is my *organizational style*, developed through plenty of trial and error, and something that is still evolving as I learn more. If you have an organizational style you are particularly fond of, you can formalize it via a [template repository](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-template-repository) and initialize future repos with that template. For example, here is [my template](https://github.com/mikemartinez99/Project-Template) that I use for all my bioinformatics analysis projects.

**Organizational Style**

We've touched briefly on organizational style, but it's worth going deeper. There's no inherently "correct" or "incorrect" way to organize a project, but what *does* matter is the consistency with which you organize them. This includes directory structure, naming conventions, and even code-styling. A predictable structure takes away the guesswork. For example, I like to title my code files and outputs as such:

```
── MyRepo/
    ├── README.md
    ├── Code/
    │   ├── 00-Preprocess.R
    │   ├── 01-QC.R
    │   ├── 02-Exploratory-Data-Analysis.R
    │   ├── 03-Differential-Expression.R
    │   ├── 04-GSEA.R
    │   └── README.md
    ├── Data/
    │   └── Raw_Data/
    ├── Outputs/
    │   ├── 00-Preprocess-Outputs/
    │   ├── 01-QC-Outputs/
    │   ├── 02-Exploratory-Data-Analysis-Outputs/
    │   ├── 03-Differential-Expression-Outputs/
    │   └── 04-GSEA-Outputs/
```

In this way, I can be 100% certain that the outputs in `04-GSEA-Outputs/` were generated from the code file that shares the same name. There is no guess work involved. I also prefer to keep code and analyses modular and bite-sized. Sure, it is totally valid to combine all 5 scripts above into one called `RNASeq-analysis.R`, but your output folder will quickly become crowded and overwhelming for anyone else who may stumble upon it down the road. 

Naming conventions also matter! Projects will follow a `YYMMDD-Project` structure. Output files are trickier because there is a breadth of variability. I try to be informative in the file name without being too long, for example `STAR-RNASeq-Counts-Raw`, or `RNASeq-Counts-Processed-TPM`. You can play around with what works best for you!

Depending on your level of compulsion, you can take your organizational style a step further and create templates for your code. I have instructions for how to set up RScript templates on my LinkedIn [here](https://www.linkedin.com/posts/michael-martinez99_documentation-is-really-important-setting-activity-7379983600563277824-hc1M?utm_source=share&utm_medium=member_desktop&rcm=ACoAADdw18oB1LGTG-ztptUQxclCITEMk-8akd0)

Why is this helpful? For starters, it takes less than 60 seconds to set up and 100% customizable, but I like to think there are a few other reasons as well. 

(A), you demonstrate consistency and attention to detail.

(B), needing to fill out documentation info and key code details *before* you even write a line of code is a good way to keep yourself accountable to documentation, and 

(C), having a consitent formatting style makes your code *easy* to view and *digestible* to collaborators. 


**Documentation**

This brings me to my next pillar of a sane project...the Golden Word **documentation**. 

This one should speak for itself. But unfortunately, all bioinformaticians have one time or another experienced the universal pain of encountering a tool where the author could not be bothered to explain how to install or run said tool...

Plain and simple: I am bullish on multi-level documentation.

*Level 1:* Your R files have documentation inside of them: headers explaining what the project is, what the code does (broadly), when you wrote it, and what the inputs are (including relevant paths). The code then has subsection headers; each subsection contains extensive comments. In my mind, every operation deserves some degree of comment, even if it is as simple as `# trim whitespace from column names`...yes, we all probably can tell that by seeing the `trimws()` command, but you stand to lose *nothing* by "idiot-proofing" your code. 

*Level 2:* Your repo contains a `README` at the top level (see my example template above). This is a high-level explanation of the project as a whole. Typically, I will include a badge showing the 'project's status, some biological background, the questions I am chasing, any software information (i.e., Conda YAMLs, Pixi lock paths, etc), and a list of all the code files included in the repo, grouped by the logical flow of the analysis (i.e., the major steps).

*Level 3:* Within the code folder, you have *another* `README` that goes into extensive detail on the technical aspects of the code. Think of this almost as a polished lab notebook. It includes things that might have failed initially, your logic for fixing them, a path to a helpful Stack Overflow post, etc. It also includes what files were output by the code. This can literally be as detailed as you want. Your top-level `README` is the *sketch*, but your code `README` is the full drawing with color and shading.

## The Pipeline With a Bright Future

How do you know if you documented your code well enough? 

Ask yourself: **"If I were to get hit by a bus tomorrow, would my co-worker be able to pick up my repo and continue where I left off?"**

Then go ahead and add some more comments to your README.


**Honorable Mentions**

1. This is not a necessity by any means, but knowing [markdown](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax) is a great way to take your project repo to the next level. Documentation can just be plain text...it gets the job done. But making documentation digestible and easy to look at encourages people to *actually* read it. 

2. I touched in passing on Conda environment YAMLs / Pixi locks...this is another key component of a reproducible analysis, but it is deserving of its own post. You can read my thoughts and opinions on that matter in my last blog post [Pixi, Conda's Cooler and Faster Cousin](https://mikemartinez99.github.io/posts/Pixi-Condas-Cooler-and-Faster-Cousin/). If you're new to software environments I recommend reading up on [Conda](https://docs.conda.io/projects/conda/en/stable/user-guide/getting-started.html) to get a footing on what this all means. 

**Compute and Conquer!!**
