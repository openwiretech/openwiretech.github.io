---
layout: post
title: Assessing ChatGPT-4o's Effectiveness in System Diagram Analysis Using Object Process Methodology
date:   2024-05-15 16:00:00 -500
categories: [article]
tags: [article, opm, chatgpt, llm] # coma separated lower case
permalink: /article_post_1
---

<img src="/assets/images/issue_1/superimposed.png" alt="SD" width="300">

ISSUE NO. 1

Date: 2024-05-15 16:00

Last updated: 2024-05-20 16:00

Copyright © 2024 Mario Colina

## An Experience Report
# Background


As systems engineering evolves, so does the demand for improved modeling in Model-Based Systems Engineering. One such method is Object Process Methodology (OPM) [[1]](#ref1). OPM was created by Professor Dov Dori[^1]. It’s an elegant and powerful modeling methodology that combines both visual and textual information in one diagram, and as testers modelling our system under test is important. The intricacies of OPM are beyond the scope of this report.

OpenAI [[2]](#ref2) just released ChatGPT-4o which extends its functionality to accept audio, image, and video. It enhances the ability to work with graphical data and could significantly improve system analysis. 

I decided to have a go at it by testing its image analyzing capabilities for the Systems Diagram (SD) I made using Robert Sabourin's [^2] Wrap-O-Matic chocolate wrapping system, which can be found in his latest book "Charting the Course: Coming Up with Great Ideas, Just in Time" [[3]](#ref3). I modeled the system using OPM to create my Object Process Diagram (OPD). 

This report, although not exhaustive, explores how ChatGPT-4o treats and identifies things within OPM diagrams and demonstrates AI’s potential to empower systems engineering. This is my **reaction** to ChatGPT-4o abilities.

[^1]: Dov Dori is a Lecturer at MIT System Design, and a Professor of Information Systems Engineering at Technion – Israel Institute of Technology Management.
[^2]: Robert Sabourin has over 30 years of management experience leading teams of software development professionals. He has provided extensive consultancy services, is an adjunct professor at McGill University, and is a popular speaker at software engineering conferences worldwide.

# System overview

The Wrap-o-Matic handles the packaging stage in a system designed to produce boxes of chocolates. Here is the SD of the Wrap-O-Matic chocolate wrapping system generated using OPCAT modeling CASE tool [[4]](#ref4). This is a high-level view of the overall system. The green boxes are Objects, and the blue ellipses are Process. 

<a href="/assets/images/issue_1/2.png" target="_blank">
    <img src="/assets/images/issue_1/2.png" alt="SD" width="auto">
</a>
<p style="text-align:center;">Click to expand image</p>




Now, what is unique to OPM is that it has an Object Process-Language (OPL).  OPL uses structured natural language to describe objects, processes, and their interactions within a system, thus making complex systems diagrams accessible and understandable through short textual descriptions. These were not fed into ChatGPT and are here to provide a better understanding of the model. The OPL sentences for the SD are as follows:

    Chocolate box set is physical.
    Chocolate box set exhibits Production Volume.
                Production Volume can be high or low.
    Company Stakeholder Group is physical.
    Company Stakeholder Group exhibits Business Success.
                Business Success can be current or improved.
    Wrapping System is physical.
    Operator Group is physical.
    Operator Group handles Automatic Chocolate Wrapping and Boxing.
    Electrical Energy is environmental and physical.
    Automatic Chocolate Wrapping and Boxing is physical.
    Automatic Chocolate Wrapping and Boxing requires Wrapping System and Electrical Energy.
    Automatic Chocolate Wrapping and Boxing changes Business Success from current to improved and Production Volume from low to high.
    Human-Centered Wrapping and Boxing is environmental and physical.
    Human-Centered Wrapping and Boxing yields current Business Success and low Production Volume. 

Let’s perform a quick analysis of the system to get us acquainted before we start with ChatGPT. Here are the main components of the system.
1.	**The Beneficiaries**: Company Stakeholder Group
2.	**The Benefit**: Business success
3.	**The Main Process**: Automatic Chocolate Wrapping and Boxing
4.	**The Main Object**: Chocolate box set.
5.	**The Benefit Providing Attribute**: Production Volume
6.	**The Enablers of the system**:
    1. **Agent**: Operator Group
    2. **Instruments**: Wrapping System and Electrical Energy
7.	**The Problem Occurrence**: Human-centered Wrapping & Boxing

**Note**: An enabler is an object that is required for a process to happen but is not transformed by the process. Agents are human, and instruments are inanimate.

# Image upload

I proceeded to prompt ChatGPT to analyze my diagram. I disabled its memory feature to make it as unbiased as possible within my other chats.

<img src="/assets/images/issue_1/3.png" alt="SD" width="auto">

<img src="/assets/images/issue_1/4.png" alt="SD" width="auto">

# Initial analysis

I next uploaded a PNG image file of my SD and prompted it to identify all the elements in the diagram...let’s see what it came up with

<img src="/assets/images/issue_1/5.png" alt="SD" width="auto">

<img src="/assets/images/issue_1/6.png" alt="SD" width="auto">

<img src="/assets/images/issue_1/7.png" alt="SD" width="auto">

<img src="/assets/images/issue_1/8.png" alt="SD" width="auto">


Let’s examine what it identified correctly and incorrectly according to my model:
- Objects - **correct**
- Processes - **correct**
- States - **correct**
- Environmental object 
    - Electrical energy – **correct**
    - Did not identify the “Human-centered Wrapping & Boxing” process as environmental
-  Enablers (Agents, Instruments)
    - Agents
        - Company Stakeholder Group – **incorrect**
            - Although you can debate Company stakeholder group since it’s not explicitly connected to the process “Automatic Chocolate Wrapping & Boxing”
        - Operator Group - **correct**
    - Instruments
        - Wrapping System - **correct**
        - Electrical Energy - **correct**
- Structural links
    - It identified a structural link, but not the type of link.
- Procedural links
    - Company Stakeholder Group (agent) - **incorrect, not explicitly linked**
    - Operator Group (agent) - **correct**
    - Wrapping System (instrument) - **correct**
    - Electrical Energy (instrument) – **correct**
    - Human-centered Wrapping & Boxing (process) is linked to:
        - Business Success (effect) - **correct**
        - Production Volume (effect) - **correct**
- Automatic Chocolate Wrapping & Boxing (process) affects:
    - Business Success (affects the state to improved or current) – **correct**.
    - Production Volume (affects the state to high or low) – **correct**.

Not bad for a first attempt! 

It incorrectly identified **Company Stakeholder Group** as an Agent, it was not explicitly connected by a procedural link.

# Structural Links

Now let’s have some fun and prompt it to identify the type of structural relation the diagram has.

<img src="/assets/images/issue_1/9.png" alt="SD" width="auto">

<img src="/assets/images/issue_1/10.png" alt="SD" width="auto">

At the beginning of its response, it didn’t correctly classify some of the structural relations, the correct classifications are:

    Exhibition-characterization is Type and its features
    Generalization-specialization is Type and its subtypes

And it forgot one:

    Classification-instantiation is type and its realizations

It did define **Aggregation-participation** correctly as a structural link, but as well shall see, it misused this in its identification. 

### What it got correct:

It identified the structural relation between **Company Stakeholder Group** and its attribute **Business Success** correctly as **Exhibition-Characterization** (relates a thing to its attribute), denoted by the symbol: 

<img src="/assets/images/issue_1/11.png" alt="SD" width="100">

Its OPL sentence is:

    Company Stakeholder Group exhibits Business success. 

Specifically, Business success characterizes Company Stakeholder Group

### What it got wrong:

It identified the structural relation between **Chocolate Box Set** and its attribute **Production Volume** incorrectly as **Aggregation-participation**. 

Its symbol is:

<img src="/assets/images/issue_1/35.png" alt="SD" width="100">

Why was it unable to correlate the same symbols as identical? Both symbols do contain triangles

- One is solid filled.

- The other has a smaller solid filled triangle inside of a bigger triangle

A future experiment would be to feed in all 4 types of structural symbols and prompt it to identify them.


# Some Improvements

Before we proceed, let’s update ChatGPT to correct the Agent. After this correction was made, it revised its analysis and did not identify the Stakeholder group as an Agent of the system.

<img src="/assets/images/issue_1/12.png" alt="SD" width="auto">

<img src="/assets/images/issue_1/13.png" alt="SD" width="auto">

# Main Process identification

Let’s drill down more and see if it can identify the systems main process

<img src="/assets/images/issue_1/14.png" alt="SD" width="auto">

<img src="/assets/images/issue_1/15.png" alt="SD" width="auto">

It did identify the main process correctly and I was satisfied with the explanation for its rationale.

# Identify the Prblem Occurance or the Problem we are trying to solve

Let’s see if it can identify the problem we are trying to solve, the occurring problem.

<img src="/assets/images/issue_1/16.png" alt="SD" width="auto">

<img src="/assets/images/issue_1/17.png" alt="SD" width="auto">

Wow, that is cool! Indeed, we are trying to increase the production volume and increase company success.

Our Problem occurrence from the OPL is defined as:

    Human-Centered Wrapping and Boxing yields current Business Success and low Production Volume. 

The Automatic Chocolate Wrapping & Boxing system will solve the above problem, as stated in the OPL sentence:

    Automatic Chocolate Wrapping and Boxing changes Business Success from current to improved and Production Volume from low to high. 


# Color identification

Now as a last measure of fun, I prompted it if it could identify the colors of the diagram.

<img src="/assets/images/issue_1/18.png" alt="SD" width="auto">

<img src="/assets/images/issue_1/19.png" alt="SD" width="auto">

Well, there are only three colors, Blue, Green and Gold/yellow (states)

# Identify some potential test ideas from Systems Diagram

Let’s prompt it to identify any potential test ideas

<img src="/assets/images/issue_1/20.png" alt="SD" width="auto">

<img src="/assets/images/issue_1/21.png" alt="SD" width="auto">

<img src="/assets/images/issue_1/22.png" alt="SD" width="auto">

<img src="/assets/images/issue_1/23.png" alt="SD" width="auto">

<img src="/assets/images/issue_1/24.png" alt="SD" width="auto">

<img src="/assets/images/issue_1/25.png" alt="SD" width="auto">

It generated generic answers and didn’t provide an gap analysis.

**Using the Systems diagram to identify failure modes**

Failure mode tests are a good strategy, lets find out what it can come up with 
 
<img src="/assets/images/issue_1/26.png" alt="SD" width="auto">

<img src="/assets/images/issue_1/27.png" alt="SD" width="auto">

<img src="/assets/images/issue_1/28.png" alt="SD" width="auto">

<img src="/assets/images/issue_1/29.png" alt="SD" width="auto">

<img src="/assets/images/issue_1/30.png" alt="SD" width="auto">

Again, It generated generic answers.


# Improvements

A more accurate analysis and test idea generation would be by feeding ChatGPT the OPL sentences.  Since this textual and provides a description of the SD, ChatGPT should be able to provide a better analysis and improved test ideas. Although this report focuses on Image identification capabilities, lets try feeding the OPL for failure mode:
 
<img src="/assets/images/issue_1/31.png" alt="SD" width="auto">

<img src="/assets/images/issue_1/32.png" alt="SD" width="auto">

<img src="/assets/images/issue_1/33.png" alt="SD" width="auto">

<img src="/assets/images/issue_1/34.png" alt="SD" width="auto">


A slight improvement in its diagram analysis. Let’s summarize the differences between the SD and OPL for failure modes that ChatGPT provided:

**Differences Between Failure Modes from the Diagram and the OPL**:

**Operator Group**

Diagram: Focuses on human error, absenteeism, fatigue, and communication failures.

OPL: Emphasizes training effectiveness and workload management.

**Wrapping System**

Diagram: Highlights mechanical failures, software glitches, and maintenance lapses.

OPL: Focuses on operational reliability and the efficacy of maintenance schedules.

**Electrical Energy**

Diagram: Considers power outages, voltage fluctuations, and energy supply interruptions.

OPL: Emphasizes power stability and energy efficiency.

**Automatic Chocolate Wrapping & Boxing**

Diagram: Concerns include system overload, integration failures, and quality control failures.

OPL: Focuses on system integration, throughput and capacity, and error handling and recovery.

**Chocolate Box Set (only from OPL)**

OPL: Adds specific tests for quality assurance and durability of the chocolate box set, which were not explicitly mentioned in the diagram-based analysis.

This is a high level-view, and one could delve deeper into failure mode scenarios. This can provide an overview to stimulate other ideas or implement better prompting. 

# Conclusion 
This initial experiment evaluated ChatGPT-4o’s ability to identify the elements in a Systems Diagram using Object Process Methodology (OPM). It correctly identified most objects and processes, however it overlooked the 'Human-centered Wrapping & Boxing' process as an environmental process (dashed ellipse) and struggled to identify some structural links. 

Despite these issues, it was capable of refining and correcting initial misinterpretations based on user feedback. A shortfall was its ability to identify some of the basic colors of the diagram.  
Where it didn’t perform well in my opinion was the test idea generation.  It didn’t provide any deep ideas, did not identify any tests that could find any gaps or flaws in the systems model. 

This report showcases the capabilities and limitations of large language models in analyzing model-based systems engineering diagrams. This evaluation, although not exhaustive, can serve as a valuable resource for understanding how an LLM can potentially be used as an aid in systems design while also provide caution to the wind about its limitations in effective model symbol identification, analysis and test idea generation.



# References

1. <a id="ref1"></a>Dori, Dov. *Model-Based Systems Engineering with OPM and SysML*. 1st ed. New York: Springer, 2016. 

2. <a id="ref3"></a>OpenAI. 2024. "Homepage." Accessed May 14, 2024. [https://openai.com/index/hello-gpt-4o/](https://openai.com/index/hello-gpt-4o/)

3. <a id="ref2"></a>Sabourin, Robert. 2024. *Charting the Course: Coming Up with Great Ideas;Just in Time*. Notion Press.

4. <a id="ref4"></a>OPCAT. 2023. "Homepage." Accessed May 14, 2024. [https://esml.technion.ac.il/opm/opcat-installation/](https://esml.technion.ac.il/opm/opcat-installation/)

# Footnotes





