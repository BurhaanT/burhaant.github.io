---
layout: post
title: Setting up WSO2 API MANAGER on Ubuntu - The easy way
date: 2014-10-23 17:23
author: burhaan
comments: true
categories: [Uncategorized]
---
<a href="http://burhaantargett.co.za/wp-content/uploads/2014/10/api-manager-logo.png"><img class="size-full wp-image-14 aligncenter" src="http://burhaantargett.co.za/wp-content/uploads/2014/10/api-manager-logo.png" alt="api-manager-logo" width="249" height="44" /></a>

&nbsp;

So, after much battling getting WSO2 API Manager up and running, I finally decided to sit down and just note how to get this running.

If you're not familiar with WSO2 API Manager, here's an excerpt from their site:

<em>WSO2 API Manager is a complete solution for designing and publishing APIs, creating and managing a developer community, and for scalably routing API traffic. The WSO2 API Manager is 100% open source. Designed for easy customization, it is extensively pluggable to integrate with existing infrastructure in your enterprise.</em>

Anyway, here's the steps, simply:

1. Download the WSO2 zip file from their site.
2. Install unzip ( sudo apt-get install unzip )
3. Unzip that file to some directory (e.g. /wso2)
4. Install Java using the following commands:
- sudo apt-get install python-software-properties
- sudo add-apt-repository ppa:webupd8team/java
- sudo apt-get update
- sudo apt-get install oracle-java8-installer
5. Check Java version is installed correctly: java -version
6. Set environment variables:
- export JAVA_HOME=/usr
- export PATH=${JAVA_HOME}/bin:${PATH}
7. In the bin folder of wherever WSO2 was extracted, run ./wso2server.sh
<p class="p1">Note, that when logging in for the first time it is recommended to change the admin password, but if you do, You have to modify the <b>AuthManager</b> section in the api-manager.xml with the new admin password.</p>
