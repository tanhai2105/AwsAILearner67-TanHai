---
title: "AWS Cloud & AI Day Vietnam 2026 (Watch Party)"
date: 2026-11-26
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

**Date:** Tuesday 29/09/2026, 08:30–17:00 · **Location:** AWS office, Level 26 Bitexco, Ho Chi Minh City (live stream from the main stage in Hanoi) · **Role:** Attendee

### Event objectives

AWS's largest Cloud and AI event in Vietnam for the year. The HCMC office ran a Watch Party so people in the South could follow the keynotes live and meet the local community. The main theme was Agentic AI, along with security, testing and AI governance.

### Speakers

- Dr. Werner Vogels — CTO, Amazon.com
- Dr. Jens Lottner — CEO, Techcombank
- Mai Le — Director of Product Management, Katalon
- Võ Xuân Minh Tuấn — Senior Solution Architect, AWS
- Representatives of the Ministry of Science and Technology, AWS and partners

### Highlights

- **Invisible Engineering (Werner Vogels):** the history behind Dynamo, Nitro, and Lambda with Firecracker and SnapStart.
- **Agentic AI at Techcombank:** AI should make staff stronger, with ROI, guardrails and monitoring in place.
- **Testing in the GenAI era (Katalon):** code is written faster than it is checked; AI does the work, people set the limits.
- **AI security agents and AI governance with Amazon Q:** reviewing designs, scanning code, controlling "shadow AI".
- **Customer stories:** Vinasoy, Agris, AMI Group and several banks.

### What I learned

- Serverless is not magic. The Lambda story showed how much engineering sits under a service I call with a few lines of code.
- Several talks said the same thing from different sides: when work is automated, people must still decide the limits and check the result.
- Testing and security belong in every step, not only at the end.

### Applying it to my work / the Cost Guard project

Cost Guard is automation that turns off someone's machine, so the "people set the limits" idea mattered to me. The stop time, the cost thresholds and the email alerts are all settings a person controls through Parameter Store and SNS, not values hidden in code. The testing message is why I later did a full end-to-end re-test of the stack: I started the instance, waited for each layer to fire, and checked that the alert email arrived. I did not trust that a layer worked just because it deployed.

### Check-in photos

![Check-in selfie at the Watch Party in the AWS office](../../images/4-EventParticipated/event2-checkin.jpg)

![The Watch Party room with the live stream from Hanoi](../../images/4-EventParticipated/event2-room.jpg)

![Closing screen of AWS Cloud and AI Day Hanoi with the feedback survey](../../images/4-EventParticipated/event2-survey.jpg)
