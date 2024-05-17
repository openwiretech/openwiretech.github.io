---  
layout: post  
title:  "Evaluating ChatGPT-4o’s Image Capabilities in Analyzing OPM System Diagrams"  
date:   2024-05-15 16:00:00 -500  
categories: [ai, systems, llm, image]  
tags: [opm, chatgpt, llm] # coma separated lower case  
permalink: /rant*post  
---  
# Evaluating ChatGPT-4o’s Image Capabilities in Analyzing OPM System Diagrams

\<img src="/assets/images/memory*leak.jpg" alt="SD" width="300"\>

ISSUE NO. 1

Copywrite © 2024 Mario Colina 


OpenAI released ChatGPT-4o. I decided to have a go at it by testing its capabilities analyzing the Systems Diagram (SD) I made using Robert Sabourin's Wrap-O-Matic chocolate wrapping system example, which can be found in his latest book "Charting the Course: Coming Up with Great Ideas, Just In Time"[2]. 

This is my first initial reaction to ChatGPT-4o ability to identify image elements, and it is not a thorough analysis.

I modeled the system using Object Process Methodology [1] (OPM) to create my Object Process Diagram (OPD). OPM was co-created by Dov Dori. It’s an elegant and powerful modeling methodology that combines both visual and textual information in one modelling diagram, and as testers modelling our system under test is important. The intricacies of OPM are beyond the scope of this article.

# System overview

Here is the SD of the Wrap-O-Matic chocolate wrapping system generated using OPCAT modeling tool [3]. This is a high-level view of the overall system. The green boxes are Objects, and the blue ellipses are Process. 

\<img src="/assets/images/issue*1/Picture2.png" alt="Memory leak havoc" width="1200"\> 

   
Now, what is unique to OPM is that it has an Object Process-Language (OPL).  The OPL sentences for the SD are.

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

1. The Beneficiaries: Company Stakeholder Group
2. The Benefit: Business success
3. The Main Process: Automatic Chocolate Wrapping and Boxing
4. The Main Object: Chocolate box set.
5. The Benefit Providing Attribute: Production Volume
6. The Enablers of the system:

Agent: Operator Group  
Instruments: Wrapping System and Electrical Energy

9. The Problem Occurrence: Human-centered Wrapping & Boxing

**Note**: An enabler is an object that is required for a process to happen but is not transformed by the process. Agents are human, and instruments are inanimate. 

## ChatGPT – image upload
I proceeded to prompt ChatGPT to analyze my diagram. I disabled its memory feature to make it as unbiased as possible.

  
  


ChatGPT – initial analysis  
I next uploaded an image of my SD and prompted it to identify all the elements in the diagram...let’s see what it came up with  


  
  
  
Let’s examine what it identified correctly and incorrectly according to my model:

•    Objects - correct
•    Processes - correct
•    States - correct
•    Environmental object 

o    Electrical energy – correct  
o    Did not identify the “Human-centered Wrapping & Boxing” process.

•    Enablers (Agents, Instruments)

o    Agents  
    Company Stakeholder Group – incorrect

•    Although you can debate Company stakeholder group since it’s not explicitly connected to the process “Automatic Chocolate Wrapping & Boxing”

    Operator Group - correct  
o    Instruments  
    Wrapping System - correct  
    Electrical Energy - correct

•    Structural links

o    It identified a structural link, but not the type of link.

•    Procedural links

o    Company Stakeholder Group (agent) - incorrect  
o    Operator Group (agent) - correct  
o    Wrapping System (instrument) - correct  
o    Electrical Energy (instrument) – correct

•    Human-centered Wrapping & Boxing (process) is linked to:

o    Business Success (effect) - incorrect  
o    Production Volume (effect) - incorrect

•    Automatic Chocolate Wrapping & Boxing (process) affects:

o    Business Success (affects the state to improved or current) – correct.  
o    Production Volume (affects the state to high or low) – correct.  
Not bad for a first attempt!  
Structural Links  
Now let’s have some fun and prompt it to identify the type of structural relation the diagram has.  
  


It identified the structural relation between Compony Stakeholder Group and Business Success correctly but got Chocolate Box Set and its attribute Production Volume incorrectly.   
Exhibition-Characterization relates a thing to its attribute, denoted by the symbol: 



The structural relation in this diagram is Exhibition-characterization. For example,  
Company Stakeholder Group exhibits Business success.   
Business success characterizes Company Stakeholder Group  
Some Corrections  
Before we proceed, let’s update ChatGPT to correct the Agent.  
  
  
After this correction was made, it revised its analysis and did not identify the Stakeholder group as an Agent of the system.  
Main Process identification  
Let’s drill down more and see if it can identify the systems main process,  



  
It did identify the main process correctly, and provided a reasonable explanation for its rationale. 







Let’s see if it can identify the problem we are trying to solve, the occurring problem  
  
  
Wow, that is cool! Indeed, we are trying to increase the production volume and increase company success.  
Our Problem occurrence from the OPL is defined as:  
Human-Centered Wrapping and Boxing yields current Business Success and low Production Volume. 

The Automatic Chocolate Wrapping & Boxing system will solve the above problem, as stated in the OPL sentence:  
Automatic Chocolate Wrapping and Boxing changes Business Success from current to improved and Production Volume from low to high.   
Color identification.  
Now as a last measure of fun, I prompted it if it could identify the colors of the diagram.  
  
  
Well, there are only three colors, Blue, Green and Gold/yellow (states)

Using the Systems diagram to identify some potential tests  
Let’s prompt it to identify any potential   
  
  
  
  
  


It generated some generic high-level tests. Nothing inspiring about it.  
Summary 

This initial experiment demonstrates ChatGPT-4o's performance was acceptable in analyzing a Systems Diagram using OPM.  It correctly identified most objects and processes but missed the "Human-centered Wrapping & Boxing" process.   
It had some challenges to accurately identify some structural links. It was capable of refining and correcting initial misinterpretations based on user feedback.  
in my opinion, it missed the mark in identifying the basic colors of the diagram, which there weren’t many. This initial analysis could serve as a resource for understanding the practical applications and limitations of Large Language Models in systems analysis.  
References

[1] Dori, Dov. 2022. "Modeling Knowledge with Object-Process Methodology." PDF file. Accessed May 10, 2023. https://dovdori.technion.ac.il/wp-content/uploads/2022/05/Modeling-Knowledge-with-Object-Process-Methodology.pdf.  
[2] Sabourin, Robert. 2023. Charting the Course: Coming Up with Great Ideas. Notion Press.  
[3] OPCLOUD. 2023. "Homepage." Accessed May 14, 2024. https://esml.technion.ac.il/opm/opcat-installation/






