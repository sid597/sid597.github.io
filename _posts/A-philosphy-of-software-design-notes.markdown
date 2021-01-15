---
layout: post
category: distributed systems
title: A Philoshy of Software Design Notes
data: 2021-01-12
---


## Chapter 2 The Nature of Complexity

<b>Exactly what is “complexity”? How
can you tell if a system is unnecessarily complex? What causes systems to
become complex? </b>


- <b>Complexity Defined</b>

-  Complexity is anything related to the structure of a software system that makes it hard to understand and modify the system.

Complexity can take many forms. For example: 
- It might be hard to understand how a piece of code works.
- It might take a lot of effort to implement a small improvement.
- It might not be clear which parts of the system must be modified to make the improvement

<i>People often use the word “complex” to describe large systems with sophisticated features, but if such a system is easy to work on, then, for the purposes of this book, it is not complex.</i>

- The overall complexity of a system (C) is determined by the complexity of
each part p (cp) weighted by the fraction of time developers spend working on
that part (tp). Isolating complexity in a place where it will never be seen is almost as good as eliminating the complexity entirely.

- <b>Symptoms of complexity</b>

Complexity manifests itself in <b>three</b> general ways, which are described in the paragraphs below :

- Change amplification :  A seemingly simple change requires code modifications in many different places.

- Cognitive load: The second symptom of complexity is cognitive load, which
refers to how much a developer needs to know in order to complete a task. System designers sometimes assume that complexity can be measured by
lines of code,  I have seen frameworks that allowed applications to be written
with only a few lines of code, but it was extremely difficult to figure out what
those lines were. <b>Sometimes an approach that requires more lines of code is
actually simpler, because it reduces cognitive load.</b>

- Unknown unknowns: The third symptom of complexity is that it is not
obvious which pieces of code must be modified to complete a task, or what
information a developer must have to carry out the task successfully


Of the three manifestations of complexity, unknown unknowns are the worst.
An unknown unknown means that there is something you need to know, but there
is no way for you to find out what it is, or even whether there is an issue. You
won’t find out about it until bugs appear after you make a change.

- <b>Causes of complexity</b>

- <b></b>

- <b></b>

- <b></b>

- <b></b>

- <b></b>



