---
title: "First Cloud AI Journey – Energetic Meet Up"
date: 2026-11-26
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

**Date:** Saturday 24/10/2026, 09:00–12:00 · **Location:** AWS office, Level 26 Bitexco, Ho Chi Minh City · **Role:** Attendee

### Event objectives

A three-hour FCAJ meet-up for interns about taking AI and cloud systems from an idea to something that runs for real: voice AI, sharing resources across accounts, security testing, running AI agents in production, and keeping an agent working around the clock.

### Speakers

- Kiet Tran (Revve AI): building a Vietnamese voice-AI contact center on AWS
- Long Huynh (Styl Solution): AWS Resource Access Manager
- Thinh Nguyen (Styl Solution): security testing with AWS Security Agent
- Xuan An (leader of the AWS Student Builder Group HCMUTE): Amazon Bedrock AgentCore
- Khoa Kieu (ACB) and Nghi Danh (AWS Community Builder): Kiro Crew

### Highlights

| Session | Topic |
|---|---|
| 1 | Hear, Think, Speak: a voice-AI contact center on AWS, with a phone-screening demo |
| 2 | AWS Resource Access Manager: share resources between accounts instead of copying them |
| 3 | AWS Security Agent: review infrastructure code and run automated tests before release |
| 4 | Amazon Bedrock AgentCore: from prototype to production |
| 5 | Kiro Crew: an AI agent that works 24/7, with limits on context, memory and permissions |

### What I learned

- The AgentCore talk opened with "Academic vs Enterprise". Academic AI aims for accuracy on a clean, fixed dataset for a few people. Enterprise AI aims for business value on real, changing data for many users, running all the time.
- AgentCore keeps inbound and outbound access apart: who may call the agent is one question, and what the agent may call is another, and each gets its own control.
- The talk ended with three takeaways that I wrote down: build security and scale into the foundation, not as a patch; agents only create value when they reach real systems and live data; if you can't measure it, you can't trust it, so log everything and set targets first.
- The voice-AI session showed that latency decides the experience: every extra step (speech to text, model, text to speech) adds delay the listener can feel.

### Applying it to my work / the Cost Guard project

The third takeaway is close to how Cost Guard works. Every layer writes what it did to the DynamoDB table (`schedule_stop`, `threshold_stop`, `budget_stop`) and logs to CloudWatch, and the target is written down before the test: the meter must stop the instance within one 5-minute interval. When I re-tested the stack, I compared the real reaction time with that target instead of only checking that the instance stopped. The first takeaway also matches our choice to give each Lambda its own role from the first template, instead of adding permissions later.

### Check-in photos

![Check-in selfie during the session From Prototype to Production](../../images/4-EventParticipated/event3-checkin.jpg)

![Agenda of the AgentCore session](../../images/4-EventParticipated/event3-agenda.jpg)

![Slide: Academic vs Enterprise AI](../../images/4-EventParticipated/event3-academic-vs-enterprise.jpg)

![Slide: AgentCore separates inbound from outbound](../../images/4-EventParticipated/event3-agentcore.jpg)

![Slide: three takeaways](../../images/4-EventParticipated/event3-takeaways.jpg)

![With friends at the meet-up](../../images/4-EventParticipated/event3-group.jpg)
