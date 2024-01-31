---
slug: journey-with-dell-poweredge-r720
title: My Journey with the Dell PowerEdge R720 Server
date: 2024-01-31T10:00:00.000Z
excerpt: An exploration of setting up a Dell PowerEdge R720 server, learning new technologies, and envisioning future tech projects.
coverImage: /images/posts/dell-poweredge-setup.png
tags:
  - Technology
  - Networking
  - Linux
  - Cybersecurity
---

## Introduction

Recently, I bought a **Dell PowerEdge R720 server** because I wanted a more powerful computer than the one I had and I didn't want to have to pay for cloud computing. So hopefully for the long term cost I got a beefy server for at home. It quickly was a challenge, especially considering its placement in a room without direct internet access. This led me to learn how to split coaxial cable and extend my home network to other parts of my house.

### Learning Network Management

A cool thing I learned about was **DHCP (Dynamic Host Configuration Protocol)** and why I may want to use a static IP reservation for my server. I learned how to set actually static IP reservation through my router. This ensures that whenever the server comes online, it always uses the IP address I've reserved, providing consistency in network management.

### Learning about New Technologies

The adventure introduced me to several new concepts and technologies:

<details>
<summary><b>iDRAC (Integrated Dell Remote Access Controller)</b></summary>

Imagine you have a really cool robot in your house that can do lots of things like turn lights on and off, check if the doors are locked, and even fix some simple problems – all without you having to be there. You could be at school or a friend's house and still control this robot with your phone. Pretty neat, right?

Now, let's think of a computer server – it's like a super-powerful computer that companies use to run websites, store a lot of data, or do complex tasks. iDRAC is like that robot, but for these servers. It's a special tool that lets people control and monitor their servers from anywhere, even if they're not in the same building or city as the server.

With iDRAC, you can turn the server on or off, check if everything is working fine, and even fix some problems, just like how you could control the robot in your house. It's really useful for people who manage a lot of servers because they can make sure everything is running smoothly without having to be right there in front of the server.

So, iDRAC is like having a remote control and a set of super eyes for your server, making it easier to manage and keep everything working well, no matter where you are.
</details>

<details>
<summary><b>RAID Configuration</b></summary>

Imagine you have a bunch of school notebooks. Each notebook represents a hard drive in your computer. Now, let's say you're taking notes for a really important class, and you don't want to lose them. Here's where RAID comes in – it's like a clever way of using your notebooks (or hard drives) to make sure your notes (or data) are safe and easy to get to.

RAID 0: Think of this like spreading your notes across several notebooks, a little bit in each. This way, you can write and read faster because you're using multiple notebooks at once. But if you lose one notebook, you lose some of your notes. This is fast but not safe.

RAID 1: This is like writing the exact same notes in two notebooks. If you lose one, no problem – you have a complete copy in the other one. It's safer because you have a backup, but it's like buying two notebooks for every class, so it costs more.

RAID 5: This is more complex. Imagine you write your notes in several notebooks but also include special 'summary pages' in each notebook. These summaries help you rebuild your notes if one notebook goes missing. It's a balance between being fast, not using too many notebooks, and still keeping your notes safe.

RAID 10: This is like a mix of RAID 0 and RAID 1. You're spreading your notes out for speed, but also keeping duplicate copies for safety. It's like having both speed and a backup plan, but you need a lot of notebooks.

In the computer world, RAID helps manage data in hard drives, either to make things faster, safer, or both. Just like how you might use your notebooks differently depending on what's most important for your class – speed or safety.
</details>

<details>
<summary><b>VMware ESXi</b></summary>

Imagine your computer is like a big house. Normally, this house has one family living in it – that's like having one operating system (like Windows or macOS) on your computer. Now, what if you could magically divide this house into several smaller, separate apartments, each with its own family living independently? This is what VMware ESXi does, but for computers.

VMware ESXi is like a special kind of magic that can take one physical computer (the big house) and split it into several virtual computers (the apartments). Each of these virtual computers can run its own operating system and applications, just like each family in an apartment can live its own life, independently of the others.

This is super useful because it means you can use one physical computer to do many different tasks at the same time, just like if you had several computers. It's like having a game room, a study room, and a movie room all in one house, but they don't interfere with each other.

For big companies, this is really cool because they can use one powerful computer to do lots of different jobs, saving money and space. It's like having one big building with different businesses inside, each doing its own thing, but all under one roof. That's the magic of VMware ESXi – turning one computer into many!
</details>

<details>
<summary><b>Linux Distributions Workstations vs Server</b></summary>

Linux distributions are tailored for specific uses. Workstation versions like Ubuntu Desktop focus on user-friendly interfaces and applications for everyday use. Server versions like Ubuntu Server are optimized for tasks like web hosting and database management, often foregoing a graphical interface for efficiency. I experimented with Ubuntu and Fedora, but I'm particularly intrigued by Pop!_OS. What's your favorite Linux distribution, and why?
</details>

### Future Project Ideas

I'm excited about several upcoming projects:

1. **Running a Wazuh node:**
2. **Setting up a Cryptocurrency Node:**
3. **Using Large Language Models for SQL Queries:**

I would love to hear your thoughts on other project ideas!

### Looking Ahead: Integrating an Nvidia Tesla GPU

I'd like to enhance the server with an **Nvidia Tesla GPU** for advanced language processing tasks. If you have experiences or advice on integrating GPUs into server setups, please share!

## Conclusion

I hope you enjoyed reading about my journey with the Dell PowerEdge R720 server. I'm excited to continue learning about new technologies and exploring future projects. If you have any questions or comments, please feel free to reach out!
