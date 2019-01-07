---
layout: post
title: The Basics of Task Parallelism via C#
date: 2014-10-29 06:44
author: burhaan
comments: true
categories: [Uncategorized]
---
The trend towards going parallel means that .NET Framework developers should learn about the Task Parallel Library (TPL). But in general terms, data parallelism uses input data to some operation as the means to partition it into smaller pieces. The data is divvied up among the available hardware processors in order to achieve parallelism. It is then often followed by replicating and executing some independent operation across these partitions. It is also typically the same operation that is applied concurrently to the elements in the dataset.

Task parallelism takes the fact that the program is already decomposed into individual parts - statements, methods, and so on - that can be run in parallel. More to the point, task parallelism views a problem as a stream of instructions that can be broken into sequences called tasks that can execute simultaneously. For the computation to be efficient, the operations that make up the task should be largely independent of the operations taking place inside other tasks. The data-decomposition view focuses on the data required by the tasks and how it can be decomposed into distinct chunks. The computation associated with the data chunks will only be efficient if the data chunks can be operated upon relatively independently. While these two are obviously inter-dependent when deciding to go parallel, they can best be learned if both views are separated. A powerful reference about tasks and compute-bound asynchronous operations is Jeffrey Richter's book, "CLR via C#, 3rd Edition". It is a good read.

In this brief article, we will focus on some of the characteristics of the <code>System.Threading.Tasks</code> <code>Task</code> object.

<a href="http://www.codeproject.com/Articles/189374/The-Basics-of-Task-Parallelism-via-C">http://www.codeproject.com/Articles/189374/The-Basics-of-Task-Parallelism-via-C</a>

&nbsp;

<img class="alignnone" src="http://dj9okeyxktdvd.cloudfront.net/App_Themes/CodeProject/Img/logo250x135.gif" alt="" width="250" height="135" />

&nbsp;
<p class="wdgpo_gplus_attachment wdgpo_gplus_article_attachment"></p>
