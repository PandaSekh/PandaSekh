
---
title: "TaleForge Dev Log #4: Beta, Bureaucracy, and Burnout Prevention"
date: 2025-03-30
categories: [AI, Startup, DevLog]
description: "The beta is live! Thoughts on early feedback, Italian bureaucracy, and reworking the heart of TaleForge."
series: [TaleForge DevLog]
---

This series was always meant to be about the **journey from idea to product**, not just the code that powers it. And right now, I’ve hit one of those defining moments in any product path: **the beta is live**.

{{< toc >}}

## 🚀 Beta Launched!

Since the last post, I didn’t add any new flashy features. Instead, I focused on **refining what was already there**: fixing edge cases, cleaning up usability, polishing UI flow. I also finished the **public website**, launched it, and started looking for beta testers through Reddit.

**About 20 people signed up**. Way more than I expected. I’ve already started receiving some feedback, which of course, has triggered the usual anxiety spiral.

## ⚖️ Bureaucracy & Break-even Math

Now that TaleForge is in beta, I’m facing a big question: **do I actually want to turn this into a real product?**

Living in Italy means **bureaucracy is a nightmare**: a tangled mess of taxes, paperwork, and people whose job is to help you understand how much to pay in taxes. If I want to be compliant and make TaleForge an actual product people can pay for, I’ll probably need to spend **at least 1200 €** just to get started legally.

So I’ve been running some numbers. Based on a rough pricing model, I’d need **about 130 paying customers** just to break even.

Will I get 130 paying customers?

I don’t know. That’s what the beta is for, to understand whether people find TaleForge useful enough to justify a price tag. The feedback I gather now will shape everything moving forward.

## 🧠 The Note Analysis Nightmare

While I’m monitoring beta usage and fixing bugs as they appear, one issue is taking up a *lot* of mental space: **the note analysis pipeline**.

It’s currently not scalable. At all. I had to limit the number of characters of uploaded notes and that, being the core feature, is not great.

It’s been haunting me. I go to sleep thinking about solutions. I wake up thinking about solutions. It’s a mess, it's the only thing I can think of. I even discussed solutions with my girlfriend during a dinner out (thankfully she's a developer herself, so she found the discussion interesting). Over the past few days, I’ve come up with 3 or 4 different approaches—two of which I even built out and tested—but none of them really stuck.

But today, while waiting for the coffee to brew, I think I’ve finally landed on something that hits a balance: **good enough for beta**, simple enough to not require additional infrastructure, and maintainable enough for me to sleep at night. I'll work on it next.

Also, I did consider moving everything to AWS. It’s tempting: scalability, flexibility, not being locked by custom tech, proprietary tech like I am right now. But, as a solo “entrepreneur”, it would mean diving headfirst into more cost, more complexity, more risk. Not worth it, yet.

## 💸 Expenses vs. Gains

Here’s the updated running total of TaleForge expenses:

| Item                      | Cost ($)     |
|---------------------------|----------------|
| Previous total              | $128          |
| Nothing, thankfully         | $0         |
| **Total**                 | **-$128**   |

**Gains:** $0 (but the beta is finally live, and that’s worth something.)

## What’s Next

- **Track beta usage**
- **Keep fixing bugs**
- **Finalize the new note analysis pipeline**
- **Keep listening**
- **Try not to panic**

This is where things start getting real. I’m both terrified and excited to see where it goes.

Thanks for following along 🛠️

{{< comments >}}
