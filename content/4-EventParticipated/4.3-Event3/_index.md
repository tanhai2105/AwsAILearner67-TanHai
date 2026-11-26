---
title: "From Prototype to Production with Amazon Bedrock AgentCore"
date: 2026-11-26
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

**Date:** Saturday 14/11/2026 · **Location:** AWS office, Level 26 Bitexco, Ho Chi Minh City · **Role:** Attendee

### Event objectives

An FCAJ session for interns on taking an AI agent from a prototype to production on AWS with Amazon Bedrock AgentCore, so that it is secure, reliable and scalable from the start.

### Highlights

- Why academic AI and enterprise AI are different problems.
- How AgentCore controls who can call an agent and what the agent can call.
- Three takeaways for running agents in production.

### What I learned

- The AgentCore talk opened with "Academic vs Enterprise". Academic AI aims for accuracy on a clean, fixed dataset for a few people. Enterprise AI aims for business value on real, changing data for many users, running all the time.
- AgentCore keeps inbound and outbound access apart: who may call the agent is one question, and what the agent may call is another, and each gets its own control.
- The talk ended with three takeaways that I wrote down: build security and scale into the foundation, not as a patch; agents only create value when they reach real systems and live data; if you can't measure it, you can't trust it, so log everything and set targets first.

### Applying it to my work / the Cost Guard project

The third takeaway is close to how Cost Guard works. Every layer writes what it did to the DynamoDB table (`schedule_stop`, `threshold_stop`, `budget_stop`) and logs to CloudWatch, and the target is written down before the test: the meter must stop the instance within one 5-minute interval. When I re-tested the stack, I compared the real reaction time with that target instead of only checking that the instance stopped. The first takeaway also matches our choice to give each Lambda its own role from the first template, instead of adding permissions later.

### Check-in photos

![Check-in selfie during the session From Prototype to Production](../../images/4-EventParticipated/event3-checkin.jpg)

![Agenda of the AgentCore session](../../images/4-EventParticipated/event3-agenda.jpg)

![Slide: Academic vs Enterprise AI](../../images/4-EventParticipated/event3-academic-vs-enterprise.jpg)

![Slide: AgentCore separates inbound from outbound](../../images/4-EventParticipated/event3-agentcore.jpg)

![Slide: three takeaways](../../images/4-EventParticipated/event3-takeaways.jpg)

![With friends at the meet-up](../../images/4-EventParticipated/event3-group.jpg)
