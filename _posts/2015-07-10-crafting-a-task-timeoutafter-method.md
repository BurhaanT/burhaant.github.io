---
layout: post
title: Crafting a Task.TimeoutAfter Method
date: 2015-07-10 01:26
author: burhaan
comments: true
categories: [Uncategorized]
---
Imagine that you have a Task handed to you by a third party, and that you would like to force this Task to complete within a specified time period. However, you cannot alter the “natural” completion path and completion state of the Task, as that may cause problems with other consumers of the Task. So you need a way to obtain a copy or “proxy” of the Task that will either (A) complete within the specified time period, or (B) will complete with an indication that it had timed out.In this blog post, Joe Hoag how one might go about implementing a Task.TimeoutAfter method to support this scenario. 

<a  href='http://blogs.msdn.com/b/pfxteam/archive/2011/11/10/10235834.aspx'>Crafting a Task.TimeoutAfter Method - .NET Parallel Programming - Site Home - MSDN Blogs</a>.
