---
layout: post
title: How to read and interpret the SQL Server log
date: 2015-12-02 02:55
author: burhaan
comments: true
categories: [Uncategorized]
---
The SQL Server transaction log contains the history of every action that modified anything in the database. Reading the log is often the last resort when investigating how certain changes occurred. It is one of the main forensic tools at your disposal when trying to identify the author of an unwanted change. Understanding the log and digging through it for information is pretty hard core and definitely not for the faint of heart. And the fact that the output of ::fn_dblog can easily go into millions of rows does not help either. But I’ll try to give some simple practical examples that can go a long way into helping sort through all the information and dig out what you’re interested in.

via <a href='http://rusanu.com/2014/03/10/how-to-read-and-interpret-the-sql-server-log/' target='_blank'>How to read and interpret the SQL Server log</a>.
