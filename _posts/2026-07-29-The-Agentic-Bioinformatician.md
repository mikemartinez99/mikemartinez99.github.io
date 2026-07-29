---
title: "The Agentic Bioinformatician"
date: 2026-07-29
categories: [AI, Opinions, Bioinformatics Musings]
tags: [Learning, Bioinformatics Musings, Opinions]
---


## TL;DR

Artificial intelligence has democratized the *syntax* of bioinformatics, not the science. What's left (and what was always the harder part), is the judgment that comes from having debugged enough of your own mistakes to recognize one forming in someone else's output. 

I am by no means "anti-AI." Agentic frameworks like Claude Code have become daily drivers for me in my workload, and I truly believe they are an invaluable tool in today's world. However, that doesn't come without its pitfalls and the need for new paradigms.

<img src="../img/July_2026/pilot_meme.jpeg" alt="Meme" style="float: right; width: 30%; margin: 0 0 1em 1.5em;" />

## So you want to be a pilot...

Autopilot transformed the world of flight. In fact, it flies a plane better than any human can on a perfect day with ideal conditions. But when the weather is rough, everyone on the flight manifest takes comfort in knowing a *trained* and *experienced* pilot is in the cockpit.

Yet, no one says autopilot democratized aviation.

A perspective paper published this May in *npj Digital Medicine*, "Rethinking Bioinformatics Expertise in the Era of Artificial Intelligence" (Goh, Polster, Wong & Cvijovic, 2026), neatly summed up thoughts that my colleagues and I have been having for quite some time now. I want to start this blog off with a quote from the paper:

*"If a large language model can draft an RNA-seq pipeline, annotate variants, and produce a publication-ready interpretation of a differential expression analysis, what exactly is left for the bioinformatician to do? The answer is: nearly everything that matters."*

## AI Custodians

I keep coming back to Tommy Tang's bioinformatics origin story, trying to open a ChIP-seq dataset in Excel as a grad student and *needing* to learn how to navigate the command line to analyze his data. For those of us who learned through countless nights of debugging, self-guided reading, lectures, classes, YouTube videos, and so on, watching that barrier disappear has been a strange feeling to sit with. Not because bioinformaticians want to gatekeep, but because the bottlenecks and the learning curves *were* the training. The upside of wrestling with messy, real-world data is that you learn to spot inconsistencies and recognize common pain points.

That's where AI changes the narrative. A model can write a working script, but the coding syntax has only ever been half the battle. The questions underneath it (is this the right tool, why am I getting this error, am I violating a statistical assumption, should my object have this shape at this step, is biology or artifact driving differences in my samples, etc.) are the bigger and far more important piece of the puzzle.

A PhD student with zero experience in high-dimensional omics data can generate a fully functioning pipeline in minutes, run it, and get results back. Woo! Time to publish... except they don't understand what the pipeline did, or whether the results it generated make any biological sense. Funnily enough, the pipeline that runs error-free can sometimes be more dangerous than the one that *doesn't*.

*"Awesome, I got 900 differentially expressed genes!"* 

🚩 But is the p-value histogram distributed uniformly? 

*"Wow, my conditions separate clearly on principal components!"*

🚩 Cool, but you know it's because all your treatment samples were from a separate sequencing batch, right? 

*"We found no statistical difference between groups, this treatment does nothing."*

🚩 Yeah, probably because you failed to account for a covariate in the statistical model.


Just to drive the point home, I myself have encountered this issue in a blood/vasculature development multiome dataset. I had used CellRank2 to calculate fate probabilities of certain progenitor cells. Claude had helped me structure some of the code...sure enough, it ran. Strangely though, all my endothelial cells seemed to have high fate probabilities for becoming erythroid cells. Not only is this biologically implausible, but Claude confidently assured me that my results held water. When I pushed back, Claude fell back to sycophancy...*"You're absolutely right to push back..."*

This is where the Goh et al. perspectives paper describes a shift in bioinformaticians from being *AI consumers* to *AI custodians*. We aren't just using AI to get outputs, but instead being *personally* responsible for whether those outputs are correct, reproducible, and biologically sound and meaningful. The barriers of execution are essentially gone, but with this comes a plethora of "AI-slop," haphazard analyses, half-baked Claude skills missing key nuances validated and benchmarked by no one, and confident misinformation disguised as sycophancy. As the authors eloquently put it: *"AI does not democratize science, it merely democratizes the appearance of science."*

## New frameworks, new best practices, new headaches

Over the years, we've established some best practices for coding (version control, documentation, environment management) that *should* be commonplace. However, as any bioinformatician knows, many repositories lack documentation, don't work out-of-the-box due to dependency issues across machines, and have no framework in place to actually ensure reproducibility.

With AI "democratizing" the ability for people to write code, we have a whole new set of hurdles to overcome. Not only do we need to version control and document the code (of which Claude helps immensely), but we currently have no best practice for how we document our agent use. Traditional non-AI analyses are deterministic in the best case scenario. Environment locks and version-controlled code *should* accept the same inputs, and generate the same outputs every time. The fact is that AI is the Wild West in terms of reproducibility. Responses vary *heavily* on what model was used, the model effort, the context window, what the tool has access to, what skills were used (and how *those* were written), and even how the prompt was written. Model updates can cause widely different outputs from the same prompt, and skills are only as good as the instructions within them. How do we install reproducible practices and ensure an AI-driven analysis stands the test of time when there are so many variables at play? Perhaps some method of semantic versioning specific to AI? If we "treat AI as a living asset," in a way, we are only increasing the amount of diligence needed to reproduce a result. Time saved up front becomes time added at the backend.

And let's be completely frank...even the most experienced bioinformaticians have had days where they don't document their code to the best of their abilities (call it laziness, rushing to meet a deadline, whatever)...do we really expect someone without the *traditional* best practices established to do that PLUS the extra leg work to version control their prompts?


## Garbage in, garbage out (now with extra steps)

I touched briefly on Skills and prompting and I think it's an area where we are likely to see a boom in the coming months or years as AI continues to accelerate in the life-science space. It's not hard to imagine a future where life-science companies start to offer Skill markdown files as part of software licenses in addition to connectors and plugins.

The thing about AI is that it's really good at doing exactly what you tell it. The bad thing about AI is...well, it's really good at doing *exactly* what you tell it.

As such, a skill is only as good as the instructions within it. A conversation I had at the Festival of Genomics in Boston this year sticks out to me. Someone had mentioned ClawBio and someone else asked *"yeah, but who is validating and benchmarking these skills?"* Take for example a skill to assess RNA-Seq alignments and quality control. If that skill fails to account for differences in prep chemistry (i.e., full length kits vs 3-prime kits), your skill might tell you that your 3' bias and high duplication rates mean your data is trash, when in reality, your data is exactly what you should expect from a 3-prime kit. The fail-point is that you haven't accounted for all possible use-cases, and within that, edge-cases. For example, to set thresholds for expected GC content, you would need to tell the skill what to expect for human, mouse, zebrafish, fly, or whatever you're working with. As the omics modality complexity increases, the content within the skill needs to increase.

You could use `/skill` which is Claude's native Skill building skill, but even then, its querying will only get you so far.

The same principles apply when prompting. How you talk to a model is everything. Leaving out crucial information or context can be the difference between garbage output and a sound, biologically grounded output. This is why you can't just blindly trust an output. Everything must be scrutinized, including the underlying prompts and skills.

## Are we breaking even?

Friction used to force validation to happen at every step in a bioinformatics analysis. AI saves us time and energy on the coding side, but removes this friction and moves it elsewhere in unprecedented ways. Debugging forced us to understand our code and computational decisions. Deterministic environments forced reproducibility. Manual steps forced us to reason through results one step at a time. Now, models add new layers of complexity into the bioinformatics space. I can't help but wonder if "removing this friction" is just adding more complicated friction elsewhere...

I think it's worth noting that I've come across quite cynical throughout this blog about AI. Like anything, it's good to be skeptical about things and go into something knowing its pitfalls. I've been on the Claude Max plan for a few months now and have used it extensively to help me automate more tedious tasks and structure my code based on prior existing code that *I* wrote. Overall, it has saved me time. But I do think that this is only because I have some prior knowledge of computer and bioinformatics.
When I use Claude on tools I *don't* have as much experience with, I'm less confident and feel the need to do my own homework and human validation.


## Conclusion

Come back to the cockpit analogy for a second. Autopilot handles the smooth air fine. But turbulence in this field doesn't look like one thing. It's a batch effect nobody flagged. It's a 3' kit's expected bias getting read as failed QC by a skill that was never told the difference. It's a model confidently telling you your endothelial cells are becoming erythroid cells and holding that line when you push back. Every section of this post was a different flavor of turbulence, and the trained pilot isn't the person who never hits it, it's the person who's hit enough of it to recognize it on instinct, before the plane does.

Regardless of where the AI landscape goes, I'm a firm believer that trainees should still always learn the basics (and to that same point, seasoned bioinformaticians should always be learning things too). Whether they want to use AI to supplement or accelerate that learning is their choice. But when they hit their usage limits, or the model confidently hands them something wrong, they need to be able to write their own code, make their own decisions, and understand what that code is actually doing. The friction that used to force this, debugging your own mistakes, fighting your own dependency hell, is disappearing. That doesn't mean the judgment it built is optional now. It means you have to build it on purpose instead of by accident.

For new and old bioinformaticians alike: we all need to learn how to best use and work with our new coworkers.

**Compute and Conquer!**

## References

- Goh, W. W. B., Polster, A., Wong, L. & Cvijovic, M. Rethinking bioinformatics expertise in the era of artificial intelligence. *npj Digit. Med.* 9, 398 (2026). https://doi.org/10.1038/s41746-026-02777-1