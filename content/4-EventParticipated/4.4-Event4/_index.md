---
title: "AWS Student Community Day Vietnam 2026"
date: 2026-11-26
weight: 4
chapter: false
pre: " <b> 4.4. </b> "
---

**Date:** Saturday 21/11/2026, 08:30–11:30 · **Location:** AWS office, Level 26 Bitexco, Ho Chi Minh City · **Role:** Attendee

![Event agenda](../../images/4-EventParticipated/event4-agenda.jpg)

### Event objectives

A morning of Cloud and AI talks run by students, organized by four AWS Student Builder Groups (HUFLIT, HCMUTE, PTIT HCM, SGU) within FCAJ. The aim was to hear from experts, let student groups present their work, and connect builders from different schools.

### Speakers

- Expert talks: "Journeying with Young Builders" and "Enterprise Cloud Environment Security"
- Speakers from the Student Builder Groups of HCMUTE, SGU, HUFLIT and PTIT

### Highlights

| Time | Session |
|---|---|
| 08:30–09:00 | Check-in |
| 09:00–09:10 | Opening |
| 09:10–09:20 | Journeying with Young Builders |
| 09:20–09:40 | Enterprise Cloud Environment Security |
| 09:40–11:00 | Talks by the four Student Builder Groups (20 minutes each) |
| 11:00–11:20 | Quiz Quest and Word Quest mini games |
| 11:20–11:30 | Closing |

### What I learned

- The security talk ("Network security on the Cloud") split protection into layers: the edge (DDoS protection, website protection, control of Internet access), the network (zones, routing, segmentation policies), the application (Security Groups, container network policies, identity), and inspection of the traffic that matters at the network and application layers.
- The Student Builder Groups closed with three ways to take part: Learn, Build and Connect.
- The quiz showed me there are still many AWS services I could not explain if asked.

### Applying it to my work / the Cost Guard project

I went back and looked at the IAM roles in our CloudFormation template. Each Lambda has its own role, and the stop function may only stop instances, not terminate them. The layered view of the talk also fits our lab machine: its security group has no inbound rule, so nothing from the Internet reaches it, and IMDSv2 is required. For the final demo I want to keep the student-talk format of about 20 minutes: one architecture diagram, the three layers in order, then a live stop.

### Check-in photos

![Check-in selfie in front of the Student Community Day screen](../../images/4-EventParticipated/event4-checkin.jpg)

![The talk on network security on the Cloud](../../images/4-EventParticipated/event4-security-talk.jpg)

![Slide: protect the system in several layers](../../images/4-EventParticipated/event4-security-layers.jpg)

![The Student Builder Groups on stage](../../images/4-EventParticipated/event4-sbg-stage.jpg)

![With friends in the hall](../../images/4-EventParticipated/event4-friends.jpg)
