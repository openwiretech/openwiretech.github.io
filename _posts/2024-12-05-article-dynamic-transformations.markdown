---
layout: post
title: Dynamic Transformations - Metamorphic Testing for AI and Other Non-Deterministic Systems
date:   2024-12-05 16:00:00 -500
categories: [article]
tags: [article, testing, ai, llm] # coma separated lower case
#permalink: /article_post_1
---

![](https://media.licdn.com/dms/image/v2/D4E12AQFckRwYlz9pDg/article-cover_image-shrink_720_1280/article-cover_image-shrink_720_1280/0/1733408507851?e=1744243200&v=beta&t=Z81OHwTrT5wIV1kkAhESDU3cPzFhJeS4LKUviOv3THk)

**NOTE:** Originally Published on LinkedIn: 2024-12-05

ISSUE NO. 2

Date: 2024-12-05 16:00

By Mario Colina


_Copyright © 2024 Mario Colina_

## 1\. Introduction

Imagine deploying an AI system responsible for real-time decision-making in fields such as autonomous driving or financial forecasting. What happens when such a system encounters scenarios beyond its training data? My experience suggests that testing large complex systems requires systems analysis, decomposition, and domain knowledge. Systems that have non-deterministic outputs have their challenges, particularly Large Language Models (LLMs), which are known for their complex and unpredictable non-deterministic outputs. My background in testing large systems underscores the necessity for handling these uncertainties.

One method that may help with these types of systems is Metamorphic Testing. Metamorphic Testing (MT) \[1\]\[2\] is a software testing method that helps in identifying a problem by focusing on the relationships between inputs and outputs across multiple executions of the program, rather than examining single outputs in isolation (Figure 1). The process involves using relationships and then testing that relationship by running a source test on the program to obtain an output result. Next, you transform the source test (i.e., modify variables including existing data, conditions or factors which you suspect relate to the behavior of the system under tests) to create a new transformed follow-up test. This follow-up test is executed on the program to produce a follow-up output result. The two output results are then compared against the relation known as a Metamorphic Relation (MR). The output relation is expected to hold true depending on the context or else there is a violation or inconsistency, which indicates that a potential problem has been identified with the program.

![](https://media.licdn.com/dms/image/v2/D4E12AQEFV01Rxr3YAA/article-inline_image-shrink_1500_2232/article-inline_image-shrink_1500_2232/0/1733359827531?e=1744243200&v=beta&t=XAQnrj1scrjLxRO1lnIY0PbhYZF5GCN6F3Sv-hxIDSg)

Figure 1. Metamorphic test execution

The prefix "meta," in Greek means "beyond" or "about," in this context it refers to a higher level of abstraction, focusing on examining properties or relationships. The suffix "morphic," in Greek means "morph," meaning "form" or "shape," indicates transformation, allowing an input to be changed into a different form.

As a tester, recognizing relationships within the systems I analyzed was always a fundamental aspect of my methodology, underscoring a systems thinking approach to my practice. However, metamorphic testing provides a structured framework in dealing with non-deterministic systems. I will explore the various applications and benefits of MT, highlighted through practical examples and case studies. This article aims to provide testers with the knowledge to effectively implement this methodology, ensuring that their AI systems are not only functional but also robust and reliable.

## 2\. The Oracle Reality

In software testing, oracles are used to identify issues. However, we are sometimes faced with situations when it is theoretically possible but impractical to determine the correct output \[3\]. This challenge is commonly known as the oracle problem, and a metamorphic relation provides a useful type of oracle. Edsger W. Dijkstra famously stated, “_Program testing can be used to show the presence of bugs, but never to show their absence!_”¹ This underscores a limitation in testing, while we can detect errors, we cannot conclusively prove their absence.

In the context-driven world of software testing, oracles are understood in a few ways:

_“Oracle is a heuristic principle or mechanism by which you recognize a potential problem”²_

_“Oracles are by their nature heuristic. That is, oracles are fallible and context dependent. Oracles do not tell us conclusively that there is a problem; rather, they suggest that there may be a problem.”_ ³

In this article, I acknowledge the oracle problem as a reality ⁴ and highlight how metamorphic relations are particularly helpful as oracles. In my practice, I establish oracles and apply multiple methods to independently assess software correctness. Given these challenges, metamorphic testing offers a structured approach to identifying discrepancies in outputs.

## 3\. Understanding the Concepts

I summarize the basis of MT in Figure 2.

![](https://media.licdn.com/dms/image/v2/D4E12AQHFK1SoB-qimQ/article-inline_image-shrink_1000_1488/article-inline_image-shrink_1000_1488/0/1733359715886?e=1744243200&v=beta&t=IMgiBjx3xS8d-2NC6fZ3wH5asWtS6lSBnBY3SghZAvU)

Figure 2. Main concepts of metamorphic tests

Metamorphic testing builds and draws inspiration from these methods (Figure 2) where:

-   Metamorphic relations are specific properties that relate multiple inputs and outputs
-   Transformations of the original source input data are used to generate a new test

Identifying relations is at the core of MT, and these properties are checked between sets of inputs and corresponding sets of outputs when executing several instances of the same program. A problem is identified when the outputs violate the relation. The key attributes of a test are listed in Table 1.

![](https://media.licdn.com/dms/image/v2/D4E12AQEwMsN8-vszAw/article-inline_image-shrink_1500_2232/article-inline_image-shrink_1500_2232/0/1733360110197?e=1744243200&v=beta&t=ggcegCSCaIVZJH1Z4qsLOnwavT6xSAfDsLiB_cqViv4)

Table 1. Key attributes of a metamorphic test

These attributes form the components of a metamorphic test, providing a structured approach to uncovering potential issues within complex systems.

## 4\. Metamorphic Relations (MRs)

One of the key steps is to identify the metamorphic relations. MRs specify how the output should change (or remain consistent) when the existing input data is transformed in a specific way. It defines the observable properties or behaviors for the program or system under test which should anticipate the resulting changes in output. For instance, if we have a function that adds two numbers, an additive metamorphic relation states that if you increase the input by a certain constant, the output will also increase by that same constant. A metamorphic relation should anticipate the resulting changes in output.

### 4.1 Structure of Metamorphic Relations

The basic structure of a metamorphic relation consists of an MR and transformations of inputs. In my experience, I label the MR as a "Root MR" as I sometimes define sub-relations of this root to provide more clarity or to group a set of sub-relations under the same root MR. This approach helps me organize and categorize the variations more effectively, as illustrated in Figure 3 and explained below:

**Metamorphic Relation (Root MR)**

-   Defines the general relation between outputs that should consistently apply to a system under test. Each root MR can include various transformations and may also have multiple sub-relations, each with its own specific transformations.

**Sub-relations (MR Sub-relation)**

-   This optional relation represents specific variations or detailed aspects of the root MR that I extended to further clarify the relationships between outputs. Each sub-relation under a root MR can encompass multiple transformations tailored to these variations.

**Transformation**

-   Involves specific modifications applied to the input data to generate follow-up tests. Both root MRs and their respective sub-relations may involve multiple transformations to explore different scenarios.

![](https://media.licdn.com/dms/image/v2/D4E12AQF3xat_Ai-DnA/article-inline_image-shrink_1500_2232/article-inline_image-shrink_1500_2232/0/1733360120720?e=1744243200&v=beta&t=0nBtf7viR6QGaBldbAaunyk_qnL2yfm5cn-Xkp0c0FA)

Figure 3. Structure of metamorphic relations

## 5\. Criteria for Identifying Effective Metamorphic Relations

One of the challenges in MT is to identify good MRs.  A balanced approach (Figure 4) can provide comprehensive coverage and ensure robustness in testing.

![](https://media.licdn.com/dms/image/v2/D4E12AQH7cXP04UbytQ/article-inline_image-shrink_1500_2232/article-inline_image-shrink_1500_2232/0/1733360146696?e=1744243200&v=beta&t=Cf60UHw8eg0UwloV8XlL9B7L-sfwhLGCc0LGqR2bItQ)

Figure 4. Finding relations

Below are some suggestions to help with this identification:

### 5.1 Exploratory Test Sessions for Identifying MRs

Exploratory test sessions can reveal practical and relevant MRs as your learning about the product, feature or specific area of software. Observing how the software responds to a variety of input transformations can help in spotting unexpected behavior patterns.

### 5.2 Subject Matter Experts (SMEs)

SMEs such as architects, product owners, systems engineers, product managers, or anyone with deep knowledge about the system that can provide insights in the complex behaviors of the system. They can be an asset to help align closely with business requirements and user expectations and another way to help in formulating MRs.

### 5.3 Specifications

Specification documents, user stories, etc., can provide a formal basis to help formulate MRs since the requirements provide information about the intended design and functional criteria.

### 5.4 Heuristic Output Patterns

Another way to help ease with the identification of MR is the use of Metamorphic Relation Output Patterns (MROP)⁵. These patterns encapsulate common metamorphic relations found in Web API testing, artificial intelligence, security, etc. Some output patterns are listed in Table 2.

![](https://media.licdn.com/dms/image/v2/D4E12AQE8rGs7h7mU-g/article-inline_image-shrink_1500_2232/article-inline_image-shrink_1500_2232/0/1733360225384?e=1744243200&v=beta&t=x-m7aYcwpw3s7XyALuGi-VZt6ykDuawiO8ltrqj1IDM)

Table 2. Definitions of metamorphic relation patterns⁶

### 5.5 General Guide to Formulate an MR

In my experience I have found that a rich variety of metamorphic relations is essential in the creation of effective tests by either using the output patterns or defining your own relations.

**Identify the output relation(s)**

Identify the property or behavior that should remain consistent or change in a specific way when the source data input is transformed. To help guide us in identifying relations, we can pose the question:

-   What relation do we want between outputs that would hold true?

A scenario that I have tested is the checkout process of an e-commerce application that had factors like dynamic pricing models, complex business rules and external API dependencies. Some relations that I identified are shown in Table 3  to illustrate the difference between the relations and sub-relations.

![](https://media.licdn.com/dms/image/v2/D4E12AQF-4AOieFMHJg/article-inline_image-shrink_1500_2232/article-inline_image-shrink_1500_2232/0/1733360297763?e=1744243200&v=beta&t=q4UL8f-yQ7RxoqeDOcv-FpF_iUcFzPo-ajr64Hu53dE)

Table 3. Root metamorphic relations and their sub-relations for shipping cost calculation

## 6\. Transformation of Source Input

Altering the source test is known as a transformation (Figure 5). A transformation of the source test generates new tests by systematically altering the source input (original) according to the MR. These new tests are known as follow-up tests.

![](https://media.licdn.com/dms/image/v2/D4E12AQHzhSydWAkS9g/article-inline_image-shrink_1500_2232/article-inline_image-shrink_1500_2232/0/1733360322931?e=1744243200&v=beta&t=4yRHrC77SqGhbi7fXvxJMOETG4-mGCK1713pkUmQXz0)

Figure 5. Input transformations from source tests

To help guide us in identifying transformations, we can pose the question:

-   How can we modify (transform) the source test — including its input variables, data, conditions or factors — to support and uphold the specified output relationship without violating it?

## 7\. Outline of Metamorphic Test Creation

An outline I have created for the basic structure of a metamorphic test is illustrated in Table 4. For a practical application of these guidelines, see the use case described in Section 8 and 9.

![](https://media.licdn.com/dms/image/v2/D4D12AQEKBiULmjqQjA/article-inline_image-shrink_1500_2232/article-inline_image-shrink_1500_2232/0/1733530363856?e=1744243200&v=beta&t=lx3v37iywGNp_R0c8tZcFzSqfEVlhOX1CSBkCOJ73ZI)

Table 4. Test creation guidelines

## 8\. Use Case Study: Dynamic Advertisement Insertion

This use case is based on my experience testing IPTV systems. I have come to understand the complex and challenging nature of dynamic advertisement insertion systems as a direct result of testing various IPTV sub-systems. The system poses a challenge to properly verify the targeting and delivery of ads to the end-user. I will walk you through a hypothetical system (Figure 6) that showcases the intricacies of dynamic ad insertion on systems that could skip ads such as VOD services, gaming platforms and streaming services. In this example, ads are selected based on many factors including:

-   **Profile Evolution** User profiles that evolve over time based on viewing habits and interactions, location and demographic.
-   **Time-dependent Variability** Changes in user behavior and ad availability at different times of the day or during different events.
-   **Dynamic Content Management** Manage and update ad content in real-time.
-   **Complex Algorithms and Real-Time Bidding (RTB)** Sophisticated algorithms and real-time bidding processes determine which ads are shown based on current market conditions and user data.
-   **Machine Learning Models** Machine learning to predict and adapt to user preferences.

![](https://media.licdn.com/dms/image/v2/D4E12AQEiAqOIN43KUg/article-inline_image-shrink_1500_2232/article-inline_image-shrink_1500_2232/0/1733360408768?e=1744243200&v=beta&t=wV2h6F8XmwIkyRl9eYxylFEvpPajbTVVRVnOTFL5WNs)

Figure 6. Dynamic ad with user skip behavior

A description and flow of the components in Figure 6, beginning with the user profile, is as follows:

-   **User Profile**

User profile that evolves over time based on viewing habits and interactions, location, and demographics.

-   **Ad Selection Algorithm**

Uses RTB and ML models to decide which ads to show based on user profile data and real-time bidding results.

-   **Real-Time Bidding (RTB) Engine**

Processes real-time bids for ad placement, interacting with the ad selection algorithm.

-   **Ad Delivery System**

Delivers selected ads to the client device, managed dynamically based on content and user interaction.

-   **Client Device**

Where the user interacts with the ads, including actions like skipping ads.

-   **Profile Customization Engine**

Updates the user profile based on historical skip data to better match user preferences.

As part of my practice, I have always identified relationships within the systems I have worked on, even though earlier in my career metamorphic testing was not part of my toolkit. If I were to apply MT to evaluate an ad system’s (Figure 6) ability to adapt to historical end user behavior, I would use a subtractive relation. For this use case I will illustrate one test.

![](https://media.licdn.com/dms/image/v2/D4E12AQHlE_Bzq487hQ/article-inline_image-shrink_1500_2232/article-inline_image-shrink_1500_2232/0/1733360451890?e=1744243200&v=beta&t=tBr4LkGVVYuDlJDbclbQNBr7HnlZELSvNgsLhqp71k4)

In the context of this example, a subtractive relation would imply the omission of an ad category by the users’ historical skipping of the same ad category. From this I would derive the source and follow-up tests as illustrated in Table 5.

![](https://media.licdn.com/dms/image/v2/D4E12AQFTI36ahtTj7Q/article-inline_image-shrink_1500_2232/article-inline_image-shrink_1500_2232/0/1733360532981?e=1744243200&v=beta&t=_ysrXItxkmja9cc8qqpJk9AHm6IPnbQBy9wFd7WlOWE)

Table 5. Test case: Persistent omission of the same ad category

This use case study highlights the complexities involved in testing dynamic ad insertion systems. We can test sophisticated machine learning algorithms that are integral to these systems with the use of relations.

## 9\. Use Case Study - Assessing complex LLM outputs

Testing the output of large language models can be a challenge depending on the type of prompts we query it with. What if we prompt an LLM system with the following:

> What is the benefit of regular exercise?

What is the correct answer? Do we have a well-defined oracle? The validation process can become more complex. There's no single correct answer, but the follow-up output response should still make sense and convey similar information as the source output. Here is where the metamorphic relations can help guide us in identifying a problem for a semantic similarity test.

**Semantic similarity test idea**

![](https://media.licdn.com/dms/image/v2/D4E12AQHgLoPI7fAw7g/article-inline_image-shrink_1500_2232/article-inline_image-shrink_1500_2232/0/1733360678267?e=1744243200&v=beta&t=W-JlrkipCoRi5FohbbKqyyEjHRmQe-UWfpIv1qtFZzA)

Let’s use ChatGPT⁷ as an example:

___

**Source Test**

Prompt

What is the benefit of regular exercise?

ChatGPT response:

Regular exercise offers numerous benefits for both physical and mental health, including:

1.  **Improved Cardiovascular Health**: Regular physical activity strengthens the heart and improves circulation, which helps lower blood pressure, reduce cholesterol levels, and decrease the risk of heart disease.
2.  **Weight Management**: Exercise helps control weight by burning calories. Combining physical activity with a healthy diet is a more effective way to lose weight and maintain a healthy weight.
3.  **Muscle and Bone Strength**: Weight-bearing exercises, such as walking, running, and strength training, help build and maintain muscle mass and bone density, reducing the risk of osteoporosis and sarcopenia.
4.  **Mental Health Benefits**: Physical activity releases endorphins, which are natural mood lifters. It can help reduce symptoms of depression, anxiety, and stress, and improve overall mood and mental well-being.
5.  **Enhanced Sleep Quality**: Regular exercise can help you fall asleep faster and deepen your sleep. However, exercising too close to bedtime may interfere with sleep for some people.
6.  **Increased Energy Levels**: Regular physical activity improves muscle strength and boosts endurance. Exercise delivers oxygen and nutrients to your tissues and helps your cardiovascular system work more efficiently, resulting in more energy for daily tasks.
7.  **Better Cognitive Function**: Regular exercise has been shown to improve brain health, enhance memory, and increase cognitive function, which can help prevent cognitive decline and reduce the risk of developing neurodegenerative diseases such as Alzheimer's.
8.  **Improved Immune Function**: Regular physical activity can help flush bacteria out of the lungs and airways, increase circulation of white blood cells, and cause a temporary rise in body temperature, which may prevent bacteria from growing. These effects can contribute to a stronger immune system.
9.  **Social Interaction**: Engaging in group exercises or sports can provide opportunities for social interaction, helping to combat feelings of loneliness and isolation.
10.  **Longevity**: Studies have shown that regular exercise can increase life expectancy by reducing the risk of chronic diseases and improving overall health.

Incorporating regular exercise into your routine can significantly improve your quality of life and overall health.

**Follow-up Test**

Prompt

How does engaging in consistent physical activity benefit an individual?

ChatGPT response

Engaging in consistent physical activity provides numerous benefits to an individual, encompassing physical, mental, and emotional health. Here are some key advantages:

1.  **Physical Health:** **Improved Cardiovascular Health:** Regular exercise strengthens the heart, improves circulation, and reduces the risk of cardiovascular diseases. **Weight Management:** Physical activity helps control weight by burning calories and increasing metabolism. **Muscle and Bone Strength:** Exercise, especially weight-bearing activities, enhances muscle strength and bone density, reducing the risk of osteoporosis and frailty. **Enhanced Immune Function:** Regular activity can boost the immune system, making the body more resistant to infections and illnesses. **Better Respiratory Health:** Aerobic exercises improve lung capacity and efficiency.
2.  **Mental Health:** **Reduced Stress and Anxiety:** Physical activity triggers the release of endorphins, which help reduce stress and promote feelings of well-being. **Improved Mood:** Exercise can alleviate symptoms of depression and anxiety, leading to a more positive outlook on life. **Enhanced Cognitive Function:** Regular physical activity is linked to better memory, faster learning, and improved problem-solving skills. It also reduces the risk of cognitive decline and diseases like Alzheimer's.
3.  **Emotional Well-being:** **Increased Self-Esteem:** Achieving fitness goals and maintaining a regular exercise routine can boost self-confidence and self-worth. **Better Sleep:** Consistent physical activity can help regulate sleep patterns, leading to better quality sleep. **Social Interaction:** Participating in group sports or fitness classes can provide opportunities for socializing and building community.
4.  **Long-Term Health Benefits:** **Chronic Disease Prevention:** Regular physical activity can lower the risk of chronic diseases such as type 2 diabetes, hypertension, and certain cancers. **Longevity:** Active individuals tend to live longer and have a higher quality of life in their later years.

Overall, incorporating consistent physical activity into one's lifestyle is a powerful way to enhance overall health and well-being, promoting a more balanced and fulfilling life.

___

Because both outputs from the LLM are long, complex and not identical, a possible strategy to assess correctness between them is provide a mix of automated and human centric verification:

-   Semantic similarity
-   Keyword presence
-   Contradiction check
-   Heuristic correctness check
-   SME to provide expert assessment of responses

Figure 7 illustrates a strategy that can be used as a basis for automation to assess correctness for complex LLM responses.

![](https://media.licdn.com/dms/image/v2/D4E12AQF7VS_AKeZorA/article-inline_image-shrink_1000_1488/article-inline_image-shrink_1000_1488/0/1733360932928?e=1744243200&v=beta&t=XQ895eG84cRUF0sRpU7a0iIA5PHsl_PQ5vbksk7r6dc)

Figure 7. Strategy to assess correctness for complex LLM responses

The following table (Table 6) provides more details on validation checks for semantic similarity tests.⁸

![](https://media.licdn.com/dms/image/v2/D4E12AQEOu2m5SGZtjg/article-inline_image-shrink_1500_2232/article-inline_image-shrink_1500_2232/0/1733361034190?e=1744243200&v=beta&t=CaTDQMsn3YY0dBuJDiMp9kpCNtd0AYj0cbG8BRUQnrk)

Table 6. Validation checks

Now, all this can be tedious when we have a variety of prompts to test against many relations. Most of this process can be automated and will support human cognitive verification in terms of comprehension and context of outputs. Tool assisted verification can help us by devising robust checks that would alert us to a problem that can be further investigated. A high-level automation workflow is shown in Figure 8.

![](https://media.licdn.com/dms/image/v2/D4E12AQGuh1WYBZPpcA/article-inline_image-shrink_1000_1488/article-inline_image-shrink_1000_1488/0/1733361056571?e=1744243200&v=beta&t=haIVgnu_U_oqg3pBiEUxUbEw8byjh9NFJ39fPfoQ8p4)

Figure 8. Automation workflow for complex LLM

### 9.1 Tools and automation

Large language models do pose testing challenges that require human cognition to interpret the results. However, that doesn’t mean a portion of the testing can’t be automated to aid in identifying problems. Various tools can help streamline the process and can be utilized to automate metamorphic testing for large language models. Table 7 provides an overview of the types of tools available for automating metamorphic testing in LLMs.  This is not a complete list but more of a starting point as there are various strategies that can be implemented.

![](https://media.licdn.com/dms/image/v2/D4E12AQGeifRn-iRN0Q/article-inline_image-shrink_1500_2232/article-inline_image-shrink_1500_2232/0/1733361098045?e=1744243200&v=beta&t=1MW6PlZkLkDWpeFdFBIcWESn1CBCPvoXf9OnzzW05nQ)

Table 7. Categories of tools for implementing metamorphic testing in LLMs

## 10\. Conclusion

Software testing is a cognitive process and in today’s challenging landscape we look for methods that help us solve problems, particularly those that are non-deterministic in nature. The primary goal of metamorphic testing is to alleviate testing systems with large and complex oracles. It presents an alternate method to meet these challenges by emphasizing the importance of using relationships along with input transformations to reveal how the output is influenced.

Illustrating the practical application of metamorphic testing, this article explored detailed case studies, including the challenges faced with dynamic ad insertion and the complexities of testing the outputs of large language models. These examples effectively showcase how this method addresses the non-deterministic nature of such systems.

This article has outlined how this structured approach can be part of a comprehensive test strategy to address the complexities of testing modern software systems, and manage the challenges posed by AI-driven environments.

___

## Footnotes

¹ Dijkstra, Edsger W. "Notes on Structured Programming." _EWD249_, Section 3, "On the Reliability of Mechanisms," corollary at the end. 1970. Accessed June 9, 2024. [https://www.cs.utexas.edu/users/EWD/ewd02xx/EWD249.PDF](https://www.cs.utexas.edu/users/EWD/ewd02xx/EWD249.PDF)

² Kaner, Cem, and James Bach. Black Box Software Testing. November 2010. Accessed June 9, 2024. [http://www.testingeducation.org/BBST](http://www.testingeducation.org/BBST).

³ Bolton, Michael. Testing Without a Map: An Investigation of Oracles. DevelopSense, April 2009. Accessed June 9, 2024. [https://www.developsense.com/resource/Oracles.pdf](https://www.developsense.com/resource/Oracles.pdf).

⁴ Based on a conversation I had with Robert Sabourin in 2022.

⁵ Extensive details can be found in section 3 of Metamorphic Testing of RESTful Web APIs \[14\]

⁶ Machine learning relations are presented in A survey of Metamorphic Testing - page 10 \[13\]

⁷ OpenAI. "ChatGPT: An AI Language Model." Accessed June 1, 2024. [https://www.openai.com/chatgpt](https://www.openai.com/chatgpt).

⁸ Reimers, Nils, and Iryna Gurevych. "Sentence-Transformers." PyPI, accessed June 9, 2024. [https://pypi.org/project/sentence-transformers/](https://pypi.org/project/sentence-transformers/).

___

## References

\[1\] Chen, T. Y., S. C. Cheung, and S. M. Yiu. "Metamorphic Testing: A New Approach for Generating Next Test Cases." Technical Report HKUST-CS98-01, Department of Computer Science, The Hong Kong University of Science and Technology, 1998.

\[2\] Chen, T. Y., F.-C. Kuo, H. Liu, P. L. Poon, D. Towey, T. H. Tse, and Z. Q. Zhou. "Metamorphic Testing: A Review of Challenges and Opportunities." _ACM Computing Surveys_ 51, no. 1 (2018): 4:1–4:27. [https://doi.org/10.1145/3143561](https://doi.org/10.1145/3143561)

\[3\] Weyuker, Elaine J. "On Testing Nontestable Programs." Technical Report No. 025. New York University, October 1980. Accessed June 3rd, 2024. [http://ia311202.us.archive.org/1/items/ontestingnontest00weyu/ontestingnontest00weyu.pdf](http://ia311202.us.archive.org/1/items/ontestingnontest00weyu/ontestingnontest00weyu.pdf)

\[4\] Bach, James. "The Essence of Heuristics." Satisfice Blog, July 28, 2005. Accessed June 9, 2024. [https://www.satisfice.com/blog/archives/33](https://www.satisfice.com/blog/archives/33).

\[5\] Bach, Jonathan. "Session-Based Test Management." _Software Testing & Quality Engineering_ (November/December 2000): 32-37. [https://www.stqemagazine.com/](https://www.stqemagazine.com/)

\[6\] DeMillo, Richard A., Richard J. Lipton, and Fred G. Sayward. "Hints on Test Data Selection: Help for the Practicing Programmer." _IEEE Computer_ 11, no. 4 (April 1978): 34-41

\[7\] Dijkstra, Edsger, “Programming methodologies, their objectives and their nature.” 1969

\[8\] Goldreich, Oded, and Dana Ron. "Property Testing in Bounded Degree Graphs." _Algorithmica_ 32 (2004). [https://doi.org/10.1145/258533.258627](https://doi.org/10.1145/258533.258627)

\[9\] Kaner, Cem, and James Bach. Black Box Software Testing. November 2010. Accessed June 9, 2024. [http://www.testingeducation.org/BBST](http://www.testingeducation.org/BBST)

\[10\] Kaner, Cem, and James Bach. Lessons Learned in Software Testing. Wiley, 2001.

\[11\] Sabourin, Robert. 2024. _Charting the Course: Coming Up with Great Test  Ideas; Just in Time_. Notion Press.

\[12\] Sabourin, Robert and Colina, Mario "Becoming a Code Listener." _Tea Time with Testers_, November 2022. [https://teatimewithtesters.com/wp-content/uploads/2022/11/TTwT\_November\_2022.pdf](https://teatimewithtesters.com/wp-content/uploads/2022/11/TTwT_November_2022.pdf)

\[13\] Segura, Sergio, Gordon Fraser, Ana B. Sanchez, and Antonio Ruiz-Cortés. "A Survey on Metamorphic Testing." _IEEE Transactions on Software Engineering_ (2016). [https://doi.org/10.1109/TSE.2016.2532875](https://doi.org/10.1109/TSE.2016.2532875)

\[14\] Segura, Sergio, J. A. Parejo, J. Troya, and A. Ruiz-Cortés. "Metamorphic Testing of RESTful Web APIs." _IEEE Transactions on Software Engineering_ (2017). [https://doi.org/10.1109/TSE.2017.2764464](https://doi.org/10.1109/TSE.2017.2764464)

\[15\] Segura, Sergio, and Zhi Quan Zhou. "Metamorphic Testing 20 Years Later: A Hands-on Introduction." In _ICSE ’18 Companion: 40th International Conference on Software Engineering_, May 27-June 3, 2018, Gothenburg, Sweden, 538-539. New York, NY: ACM, 2018. [https://doi.org/10.1145/3183440.3183468](https://doi.org/10.1145/3183440.3183468)

\[16\] Weinberg, Gerald M. 2001. An Introduction to General Systems Thinking Silver anniversary ed. New York: Dorset House.

\[17\] Bolton, Michael. Testing Without a Map: An Investigation of Oracles. DevelopSense, April 2009. Accessed June 9, 2024. [https://www.developsense.com/resource/Oracles.pdf](https://www.developsense.com/resource/Oracles.pdf)

\[18\] Yang, Yingzhuo, Zenan Li, Huiyan Wang, Chang Xu, and Xiaoxing Ma. "Towards Effective Metamorphic Testing by Algorithm Stability for Linear Classification Programs." _Journal of Systems and Software_ (2021). [https://doi.org/10.1016/j.jss.2021.110655](https://doi.org/10.1016/j.jss.2021.110655)