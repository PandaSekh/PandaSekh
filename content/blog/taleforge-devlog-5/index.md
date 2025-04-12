---
title: "TaleForge Dev Log #5: Postmortem of an almost-SaaS"
date: 2025-04-12
categories: [AI, Startup, DevLog]
description: "What is TaleForge going to be?"
series: [TaleForge DevLog]
---

It has been a while since the last post. I took this time to review TaleForge's beta usage and to think what to do moving forward. Shall I create a company and move forward, or should I rethink everything and change the course of action?

This post will be quite verbose, so here's a TL;DR:  
*I'll stop all things related to TaleForge as a product to sell. I'll focus on changing its code into something one can run on its own device. This is not the ideal solution for most, but it's the right choice for me right now.*

{{< toc >}}

## How the beta went

Not as I expected. At the moment I have 30 beta users, which I thought was a good number, or at least enough to gather feedback.  
Unfortunately, I didn’t consider that many of them might not actually be interested in the product, nor in giving feedback. The usage of TaleForge has been low and the feedback I got, while really helpful, was not sufficient.

I write this not to blame the users, but to give better context on why I took some decisions.  
For example, from this lack of usage what I get is: the app isn't actually useful or engaging. Users don't have any reason to come back to the app.

At the moment TaleForge can only analyze notes and generate characters, locations and story sheets. This is useful, and I got great feedback on that, but then? Users don't have so that many new notes every day, so there’s no reason to return daily. And that becomes a problem when it comes to pricing: how can someone justify spending money on something that they don't use daily?  
(Now that I wrote this, it sounds stupid as not everything I own is used everyday, but I'm referring to subscriptions here.)

From this point of view the beta went great as I was able to understand this before it was too late.

## Product vs Tech

Working on the "tech" part of TaleForge was enjoyable. I had fun thinking and implementing the analyses and, when there was a scalability issue, it was a great challenge to come up with the best solution given the constraints I had.

However, the "product" side was less enjoyable. In this case with product I mean everything that wasn't directly related to the technical part itself. Things like the homepage, landing page, FAQ, "marketing", analytics, GDPR, market research and so on.  
This is not something I'm used to do, so it was a great learning experience, but something that I should've done with more calm. Between all of these - and more - "product" things to consider and the technical part, I found myself thinking too much of TaleForge. From the time I woke up until I went to sleep (I talked about this in other dev logs too).

If I wanted to bring TaleForge to market, I had to answer a few key questions: What do the users want? How will they use the product? Will they actually be interested in paying?  
Given the huge cost of starting a company in Italy, where I live, these were all things I had to consider before proceeding with the company formation.

However, without a company, I can't sell the product. So I was in a limbo, where either I spend a lot of money, form a company and try to get some subs while I improve the product, or I improve the product in a rush trying to go to market before I finish the money.

In the end, none of these paths are ones I want to follow. I have a full time job already taking most of my time and mental energy, I can't keep giving all my free time to this project.

Moving forward, I'll focus on the technical side of TaleForge, adding new features and improving it.

## What is TaleForge?

The other day I had to stop and ask myself, "What is TaleForge?". Why did I start it?  
The answer is quite simple: I needed it. I have a lot of notes on paper, in digital notes and in audio clips. I needed a way to organize them all, so I created TaleForge.

I soon noticed that there was potential, so I pivoted and tried to create a SaaS product. In the weeks that passed working on TaleForge, I think I lost sight on the reason I built it, and so the vision of the future was lost.  
At one point I didn't know what TaleForge was going to be.

Having taken the decision to work on TaleForge as a product for myself, I have a better vision on what I want to do and when.

## The Future of TaleForge

My aim with TaleForge is to build a suite of tools to help fiction writers.  
This is not set in stone, but at the moment is what I'm aiming for.

## Next Steps

The first step is for me to take back time for my other hobbies: writing, reading, gardening, etc.  
I'll take a break from coding in my free time.

The beta will be stopped, but the public website will be kept online.

Next, I have a high-level plan on what to work on to move to the next phase, here's a short version:

- Separate the frontend between the webapp and the landing page. The landing page will be public, but the webapp must be hosted manually
- Migrate the backend to a monolithic server
- Containerize everything for ease of deployment
- Keep publishing devlogs, but make them more technical. This is something I noticed previous posts lacked

This will take a bit of time, as now I'll reduce my focus on TaleForge.

## Open Source

Another big decision I took is to make TaleForge open source.  
This is almost mandatory for self-hostable project, but the decision to open source the code was taken a while back.

The reasons are multiple, but the core concept is that if the code is open everyone can investigate and make sure that the data is safe.

As TaleForge uses AI, I can't provide a full E2E encryption, as the data will eventually be sent to third parties (this is something I'd like to avoid, but it's not feasible at the moment).  
Users should "just believe me" that I won't read their data.

When I decided to implement encryption at rest, I knew I wanted to also open source the code to reassure users that their data was safe.

## What I learned

A lot. On top of my head I think the main points are:

- Don't work too much on a project or burnout will get you
- Launch an MVP as fast as possible
- Leverage AI tools to reduce workload (I did a lot of research using ChatGPT 4.5, money well spent)

And probably much more things that will come handy in the future.

## 💸 Expenses vs. Gains

Running total of TaleForge expenses:

| Item           | Cost ($) |
|----------------|----------|
| Previous total | $128     |
| Nothing new    | $0       |
| **Total**      | **-$128**|

## What’s Next

- **Close the beta**
- **Relax**
- **Work on the code as stated above**
- **Open source the code**

## Closing Notes

**In short**: I’m stepping away from the “startup” part of TaleForge.  
It’ll become a personal, open-source tool for writers.

{{< comments >}}
