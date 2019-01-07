---
layout: post
title: How To Create SSH Keys With PuTTY to Connect to a VPS | DigitalOcean
date: 2016-05-11 07:34
author: burhaan
comments: true
categories: [Uncategorized]
---
If your headless, or remote, VPS is visible over the Internet, you should use public key authentication instead of passwords, if at all possible. This is because SSH keys provide a more secure way of logging in compared to using a password alone. While a password can eventually be cracked with a brute-force attack, SSH keys are nearly impossible to decipher by brute force alone. With public key authentication, every computer has (i) a public and (ii) a private "key" (two mathematically-linked algorithms that are effectively impossible to crack).

Today, OpenSSH is the default SSH implementation on Unix-like systems such as Linux and OS X. Key-based authentication is the most secure of several modes of authentication usable with OpenSSH, such as plain passwords and Kerberos tickets. Other authentication methods are only used in very specific situations. SSH can use either "RSA" (Rivest-Shamir-Adleman) or "DSA" ("Digital Signature Algorithm") keys. Both of these were considered state-of-the-art algorithms when SSH was invented, but DSA has come to be seen as less secure in recent years. RSA is the only recommended choice for new keys, so this tutorial uses "RSA key" and "SSH key" interchangeably.

Full Article: <a href='https://www.digitalocean.com/community/tutorials/how-to-create-ssh-keys-with-putty-to-connect-to-a-vps' target='_blank'>How To Create SSH Keys With PuTTY to Connect to a VPS</a>.
