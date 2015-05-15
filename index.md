---
layout: default
title: About
---

# Report Card Generation from Robot Mobile Manipulation Activities #

## Introduction ##

The main goal of the project is to provide robotics research environment with succinct report of manipulation activities performed by a robot. To this end, an automated *Report Card* generation tool will be developed as an extension of widely used CRAM system.  
The tool will be easy to use, accessible for wide community and highly customisable in a way that generated *Report Cards* can be tailored to personal needs by displaying relevant to the experiment statistics. Such *Report Card* will consist of basic robot performance measures that are easy to view, read and interpret, therefore, time-consuming raw data analysis that needed additional statistical knowledge won't be necessary.

## Project Goals ##
The project aims to deliver a tool capable of automatic *Report Cards* generation from robot manipulation task logs. The tool will be integrated with widely used among robotics research communities CRAM package therefore allow for straightforward report generation without need of additional tools.  
The program will take as an input XML-like logs from robot manipulation tasks and extract relevant information from them for future analysis. Then, user will be allowed to choose types of statistics to be included in the *Report Card* form available range of performance measures or write one owns code - inject custom `R` code - to be used as data analysis framework.  
As each task performed by a robot is highly structured and exhibits elements of hierarchy producing *Report Cards* with different level of detail based on user needs will be of great value. Moreover, the tool would provide several predefined 'styles' (flavours) of the *Report Card* and custom mode where user can specify needed output. Finally, producing succinct plain English description of the data analysis results in preamble of document will give great insight into the document before more detailed results follow i.e. Narrative Analysis.

User will be delivered ready to view PDF file that will be helpful with debugging robot behaviour, spot behavioural anomalies, analyse at glance robot performance in time and space, view successfulness of basic tasks, etc..  
I expect to include into the *Report Card* amongst others:

1. Narrative Analysis.
2. Time-line analysis.
3. Proportion of each of the basic moves.
4. Path analysis.
5. Successfulness of performed tasks.
6. Abnormalities detection, etc.

## Implementation ##
As `Prolog` introduces power of first-order logical rules it gives greater power than widely used propositional logic employed by vast majority of machine learning algorithms. Moreover, it has great capabilities when dealing with natural language processing and pattern matching: in this particular application log analysis. It's capabilities can be extended even further when integrated with [`R` statistical package][real], therefore, producing plots and statistical analysis/hypothesis testing for the *Report Card* will be straightforward.  
The *Report Cards* will be generated using `LaTeX` and automated via `Java` program. Use of these technologies will enhance the look and feel of the report by allowing multiple 'flavours' and facilitate multi-platform use.

## Time-line ##
I reckon the following time-line to complete proposed project (13 weeks of GSoC):

1. Familiarising with logs format, parsing and knowledge representation. (~2 weeks)
2. Designing and tuning `Prolog` rules. Defining and implementing hierarchy of detail. (~2 week)
3. Deciding on information to be placed in *Report Cards* and basic 'flavours'. Extracting relevant information. Performing data analysis and producing plots. (~2 weeks)
4. Designing statistical tests to be run on extracted database to identify potential anomalies to be included in the *Report Card*. Generating relevant plots. (~1 week)
5. Generating simple 'Narrative Analysis' to be placed in preamble of document. (~2 weeks)
6. Designing LaTeX document and its structure. Generating LaTeX document. (~2 week)
7. Integrating with CRAM.(~1 week)
8. Cleaning the code. Filling documentation. Writing final report. (~1 week)

## About me ##
I'm final year undergraduate MEng Mathematics & Computer Science student.
I have advanced `Prolog` skills including Inductive Logic Programming frameworks and `Prolog`<->R interface: during Artificial intelligence and Logic Programming course I developed BlackJack game engine and different playing strategies using best of both worlds (`Prolog` and `R`).  
Furthermore, I have experience with real-time (and static) log analysis gained during my internship at CERN - I designed and implemented self-tuning hardware monitoring application in Java and ESPER: complex event processing library.  
Moreover, I'm experienced LaTeX user: I use it to write all the reports and I have designed `LaTeX` 'problem sheet' generator for one of the courses that I was tutoring; the program was generating different input variables and corresponding solutions for the problems by `LaTeX`<->`R` integration.  
Furthermore, I'm advanced `GIT` user, I have great programming skills mainly in `Prolog`, `Java` and `Python` (I have used `Prolog` interfaces within both later languages), nevertheless, I'm competent with many other languages. I'm also familiar with basics of ROS and robotics gained during graduate level course.

Finally, I gained extensive statistical and probability theory knowledge during graduate level courses taken in the Mathematics Department which will be essential while analysing the robot's log data. I have also completed number of research internships within machine learning group over the summer therefore I'm familiar with vast number of machine learning and artificial intelligence concepts needed for successful completion of proposed project.

I find the concept of generating *Report Cards* really fascinating as in a way it complements my current Master's Project and PhD that I'm starting next year.  
Currently I'm developing `Prolog` framework with use of Inductive Logic Programming (in particular [Aleph][aleph]) constituting Activity Recognition Model for Smart Houses (spatio-temporal data) applications. The aim of the project is to produce human readable model of data - in form of `Prolog` predicates - which recognises human activity based on house sensors output data.

Moreover, I received a studentship to start a PhD in next academic year with [SPHERE Project][sphere] at the University of Bristol, UK. I will be researching a 'Narrative Analysis' tools for Smart Houses. The main concept behind the project is to extend my previous work and produce description of held activity in plain English so that medical personnel can easily analyse the current state of patient without time consuming data analysis.

I believe that robots performing manipulation tasks are inevitable step in development of Smart Houses. Undertaking proposed GSoC project would grant me first-hand experience of interpreting robot data and give me a great insight for my future research. Both: my current work and GSoC project present various similarities, especially when treating the robot as a monitored person.  
After Google Summer of Code I'd love to continue my work on the system as I believe that my future research can greatly benefit the *Report Card* system.

[real]: http://www.swi-prolog.org/pack/list?p=real
[aleph]: http://www.cs.ox.ac.uk/activities/machlearn/Aleph/aleph.html
[sphere]: http://www.irc-sphere.ac.uk

<!--
<img src="/images/shakespeare.png" align="right"/> class="right"
-->
