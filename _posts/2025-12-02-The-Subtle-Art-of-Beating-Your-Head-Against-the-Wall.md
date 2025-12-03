---
title: "The Subtle Art of Beating Your Head Against the Wall"
date: 2025-12-02
categories: [Bioinformatics Musings]
tags: [Learning]
---

## TL;DR

**Learning bioinformatics is less about instant wins and more about learning to persist through repeated failure, and that persistence is the real skill.**

# Bioinformatics: The Wall. 

Let's set the scene. I'm 21 years old sitting in a zoom room at the height of the COVID-19 pandemic. The class is "Molecular Evolution and Bioinformatics" at the University of Connecticut. I had swapped out of another totally unrelated course in palce of this 2 weeks into the semester at the recommendation of a friend. I remember the first lecture. So many terms were being casually tossed around by my professor in his thick German accent. "Synteny", "sequence-space", "orthology"...I was starting to wonder what I had signed up for. The lab component of the class was where I would encounter the real trouble. 

While the first few labs were pretty straight forward...running BLAST from the NCBI website, the course began to take a turn as we leared about gene transfers, specifically genetic parasites known as inteins. We started to dive heavily into phylogenetics, genome alignments, and the command-line. This is where I first saw what I'll call "The Wall." I was exposed for the first time to my computer's terminal, and beyond that, our university HPC (Xanadu). I remember the first time I heard my TA say "go ahead and SSH into Xanadu." I thought he was speaking some other language. 

Like any new skill, the learning curve is steep at first. In the case of bioinformatics, I wish I could say I fell in love instantly, but truthfully, I dreaded Thursday afternoons where i'd log onto zoom and fumble around my terminal for 3 hours. I can't tell you how many hours I wasted in the early days of my bioinformatics journey wondering why my "file was not found" or "did not exist", only to realize, I was one directory up from where I needed to be, or my connection to the universities high performance cluster had timed-out without me realizing. The technical struggles were compounded by the conceptual struggles. I had never been taught the concepts we were learning, let alone how to *think* about biology computationally. I felt as though I was staring up at a wall so high that my neck would break. 

# Trial and Error: The Beating

Grad school. The "pre-chat GPT days" as I call them now. The days when Stack Overflow and BioStats where the go-to places for debugging an error. I frequented these forums on many days. In fact, most days, it felt as though I spent more time struggling to fix my code than making sense of results. By now my terminal and bash skils had improved, but now the R learning phase began. When I think back to things I struggled with in R, I almost laugh. But back then, every line that ran error-free felt like climbing that wall a few inches higher. 

My biostats professor once told me: **Anyone can read the book about how to code, but the person that goes out and codes is the person that will truly learn it.**

Thrown into R with almost no real background, those countless days of chasing error messages, meeting with my supervisor, re-writing loop after loop, developing a crippling energy-drink addiction to keep up (oops)...eventually things finally began to click. 

This is one thing that you *can't* learn in a class. Tutorials only show the best-case-scenarios; but your data will almost never be perfect.
You *will* fight dependency errors (and in some of these fights, you will be bested). Your code *will* break in strange and creative ways. You *will* encounter a tool that was last updated 10 years ago with the last commit message being "works now"...it's par for the course. 

Learning how to beat your head against a wall (gracefully) is the real skill: resilience, patience, resourcefulness, and a bit of stubborness.

# AI: Helmet for the beating??

With the rise of AI,  this "head beating" has lessened to some extent. It's much easier to paste your error message into your favorite LLM and get back 30 lines of neartly linted code sprinkled with comments and emojis. Great! Until, you realize you don't actually know what your code does now or what the fix was. AI is powerful and in today's bioinformatics world, it is an indispensable resource. For beginners however, it is a double-edged sword. Rely on it blindly, and you don't truly learn...

AI should not replace the real learning that occurs from wrestling with your code and the traditional "rights or passage" (i.e., finally getting the right numpy version in your conda env). Just because you can blast through the wall with brute force, doesn't mean you actually climbed it. 

As Dean Lee and Tommy Tang like to say:
**Ask your LLM to explain everything line by line.**
**Read it. Rewrite it. Make sure you understand it.**

# My Two Cents

If you're just getting started and feeling overwhelmed, do not give up. You won't master this field in a year, or ten. Nobody does. You will just become really good at writing code, and really good at beating your head against a wall. 

1.	**Start small:** Pick a figure from a paper you enjoyed. Look for the data in public repositories. Try and replicate it.

2.	**Follow your confusion:** Embrace uncertainty. Every unfamiliar term or concept is a branch point in your education. 

3. **Utilize your resources:** StackOverflow. Youtube. Literature. Mentors. Spend time every day supplementing your learning.

3.	**Use AI as a tutor, not a crutch:** ask it to explain code, then re-implement the logic yourself.

4. **Break things intentionally:** you learn more from errors than successes.

5. **Celebrate progress:** every working script is a step up the wall.

The important thing is that you learn to beat your head against the wall without breaking your spirit...this is the subtle art. When you finally make it to the other side, enjoy the view, but not for too long...someone just invented a new omics technology and your client needs the results by Friday.

**Compute and Conquer!!**
