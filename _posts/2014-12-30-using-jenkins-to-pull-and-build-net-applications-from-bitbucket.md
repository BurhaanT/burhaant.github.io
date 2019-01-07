---
layout: post
title: Using Jenkins to pull and build .Net applications from BitBucket
date: 2014-12-30 08:36
author: admin
comments: true
categories: [Uncategorized]
---
<ol>
	<li>Download and install the latest, native package, of the Jenkins Windows Installer from <a href="http://jenkins-ci.org" target="_blank">http://jenkins-ci.org</a> (version used for this instruction: 1.595).</li>
	<li>Open up the Jenkins dashboard (localhost:8080).</li>
	<li>Go to "Manage Jenkins &gt; Manage Plugins" and select the following plugins to install:
<ul>
	<li>Git Client</li>
	<li>Git Server</li>
	<li>Git Plugin</li>
	<li>BitBucket Plugin</li>
	<li>MSBuild Plugin</li>
</ul>
</li>
	<li>Install Git Extensions from <a href="http://sourceforge.net/projects/gitextensions" target="_blank">http://sourceforge.net/projects/gitextensions</a> - Use all defaults.</li>
	<li>Install Git Bash from <a href="http://git-scm.com/download/win" target="_blank">http://git-scm.com/download/win</a> - During the installation select "Use Git from the Windows command Prompt" on the "Adjusting your Path environment page".</li>
	<li>I would suggest that you restart the machine at this point, this forces a restart of Jenkins (obviously). I suppose you could just restart Jenkins but hey....</li>
	<li>When Jenkins is installed on Windows, it runs the service a the local service account. This user will need to have .ssh setup to include the known_hosts and id_rsa files (See here on how to do this: https://gist.github.com/tehfoo/3708738#generate-rsa-key-pair-on-your-jenkins-server)</li>
	<li>Once these files have ben created, copy the entire .ssh directory to C:\Windows\SysWOW64\config\systemprofile. If you chose to run the service as a specific user, put the .ssh folder in the profile of that user (i.e. C:\Users\TheUser).</li>
	<li>Under global configuration for Jenkins, make sure to set the path to git as <em>&lt;InstallDirectory&gt;\cmd\git.exe </em>(NOT just git.exe)</li>
	<li>Go to the Jenkins dashboard and select "New Item".</li>
	<li>Put in a name that suits you and select "FreeStyle Project". Click Ok.</li>
	<li>On the configure page, under "Source Code Management", select "Git" and put in the SSH url of your BitBucket repo.</li>
	<li>Set the credentials to "none".</li>
	<li>Schedule your build ("Build Now") and your should see that succeed.</li>
</ol>
On to setting up the build:
<ol>
	<li>Firstly download NuGet.exe from <a href="https://nuget.codeplex.com/releases " target="_blank">https://nuget.codeplex.com/releases </a>copy the exe to somewhere you'll find it.</li>
	<li>Install the latest version of the .Net framework and locate MSBuild.exe (take a look in the "framework" folder).</li>
	<li>Setup MSBuild on "Manage Jenkins &gt; Configure System" by specifying a name and the full path to the MSBuild (noted in point 2).</li>
	<li>The purpose of NuGet here is to restore packages for the project and a bunch of parameters can be passed to nuget indicating package sources and the like. Personally, I prefer for all this to be setup in a config file instead. Take a look at the <a href="http://docs.nuget.org/docs/reference/command-line-referenc" target="_blank">Nuget Command Line Reference</a> if you'd like to use the command line options, but you can copy my settings (below), adjust for your need and store in a file named &lt;Something&gt;.config (remove the proxy settings if you don't need those). This config file can then be passed in using -ConfigFile.  A note that passwords are hashed and will need to be set using the command line.
<blockquote style="margin: 0 0 0 40px; border: none; padding: 0px;">
<pre>&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;configuration&gt;
 &lt;packageSources&gt;
 &lt;add key="Alternative package source" value="http://packages.nuget.org/v1/FeedService.svc/" /&gt;
 &lt;add key="NuGet official package source" value="https://nuget.org/api/v2/" /&gt;
 &lt;/packageSources&gt;
 &lt;activePackageSource&gt;
 &lt;add key="All" value="(Aggregate source)" /&gt;
 &lt;/activePackageSource&gt;
 &lt;packageRestore&gt;
 &lt;!-- Allow NuGet to download missing packages --&gt;
 &lt;add key="enabled" value="True" /&gt;

 &lt;!-- Automatically check for missing packages during build in Visual Studio --&gt;
 &lt;add key="automatic" value="True" /&gt;
 &lt;/packageRestore&gt;
 &lt;config&gt;
 &lt;add key="HTTP_PROXY" value="mycorpproxy" /&gt;
 &lt;add key="HTTP_PROXY.USER" value="domain\user" /&gt;
 &lt;add key="HTTP_PROXY.PASSWORD" value="the password as set by the command line option" /&gt;
 &lt;/config&gt;
&lt;/configuration&gt;</pre>
</blockquote>
</li>
	<li>On the build configuration of the project in Jenkins, add the following steps under "Build" (They should be in this order as well):
<ul>
	<li>"Execute Windows batch command" : <em>&lt;NuGet.exe Path&gt;</em>\NuGet.exe restore <em>&lt;name of sln/ csproj file of your project&gt;</em> -ConfigFile <em>&lt;Config Location&gt;</em>\<em>&lt;The config file from point 4&gt;</em></li>
	<li>"Build a Visual Studio project or solution using MSBuild":
<ul>
	<li>MSBuild Version: <em>Select the name you setup in point 3</em>.</li>
	<li>MSBuild Build File: <em>The name of your sln/csproj file</em>.</li>
	<li>Command Line Arguments: <a href="http://msdn.microsoft.com/en-us/library/ms164311.aspx" target="_blank">http://msdn.microsoft.com/en-us/library/ms164311.aspx</a></li>
</ul>
</li>
</ul>
</li>
</ol>
And that's it. If you've followed all the steps correctly, you'll have a Jenkins instance setup and ready to build .Net applications. Click Build Now and off you go....

&nbsp;

See these sites for reference:
<ul>
	<li><a href="https://wiki.jenkins-ci.org/display/JENKINS/Git+Plugin" target="_blank"><em>https://wiki.jenkins-ci.org/display/JENKINS/Git+Plugin</em></a></li>
	<li><a href="http://computercamp-cdwilson-us.tumblr.com/post/48589650930/jenkins-git-clone-via-ssh-on-windows-7-x64" target="_blank"><em>http://computercamp-cdwilson-us.tumblr.com/post/48589650930/jenkins-git-clone-via-ssh-on-windows-7-x64</em></a></li>
	<li><a href="https://gist.github.com/tehfoo/3708738" target="_blank"><em>https://gist.github.com/tehfoo/3708738</em></a></li>
	<li><a href="https://adrianliew.wordpress.com/2013/09/04/setting-up-jenkins-as-a-ci-server-for-net-3-54-04-5-projects-using-bitbucket-1-of-2/" target="_blank">https://adrianliew.wordpress.com/2013/09/04/setting-up-jenkins-as-a-ci-server-for-net-3-54-04-5-projects-using-bitbucket-1-of-2/</a></li>
	<li><a href="http://www.infoq.com/articles/MSBuild-1" target="_blank">http://www.infoq.com/articles/MSBuild-1</a></li>
	<li><a href="http://docs.nuget.org/docs/reference/command-line-reference" target="_blank">http://docs.nuget.org/docs/reference/command-line-reference</a></li>
	<li><a href="http://docs.nuget.org/docs/reference/nuget-config-settings" target="_blank">http://docs.nuget.org/docs/reference/nuget-config-settings</a></li>
	<li><a href="http://msdn.microsoft.com/en-us/library/ms164311.aspx" target="_blank">http://msdn.microsoft.com/en-us/library/ms164311.aspx</a></li>
	<li><a href="http://blog.davidebbo.com/2011/03/using-nuget-without-committing-packages.html" target="_blank">http://blog.davidebbo.com/2011/03/using-nuget-without-committing-packages.html</a></li>
	<li><a href="http://marcofranssen.nl/ci-with-jenkins-msbuild-nuget-and-git-part-2/" target="_blank">http://marcofranssen.nl/ci-with-jenkins-msbuild-nuget-and-git-part-2/</a></li>
	<li><a href="http://automatetheplanet.com/2014/08/31/integrate-jenkins-msbuild-nuget/" target="_blank">http://automatetheplanet.com/2014/08/31/integrate-jenkins-msbuild-nuget/</a></li>
</ul>
