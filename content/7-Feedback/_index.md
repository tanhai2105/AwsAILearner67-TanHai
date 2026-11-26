---
title: "Sharing and Feedback"
date: 2026-11-26
weight: 7
chapter: false
pre: " <b> 7. </b> "
---

> These are my thoughts after almost three months in First Cloud AI Journey, with a few suggestions that I hope help the program for the next cohorts.

### Overall Evaluation

**1. Working Environment**

The events at the AWS office on Level 26 of Bitexco made me feel I was working in a real environment, meeting speakers from companies and other interns. The rest of the time my team worked online through WhatsApp and GitHub, splitting tasks and setting deadlines ourselves.

**2. Support from Mentor / Team Admin**

The documents, the workshop template and the scoring rules were clear, so the team knew what to submit and how it would be graded. When choosing our topic, I used those scoring rules to compare four ideas before we decided.

**3. Relevance of Work to Academic Major**

I study Software Engineering. At school my code runs on my own machine; here my Lambda code ran on AWS, and I had to think about IAM permissions, cost, and cleaning up resources afterwards. School does not cover much of this, so I learned a lot.

**4. Learning & Skill Development Opportunities**

- Writing infrastructure with CloudFormation, and learning that "deployed" is not the same as "working correctly": in my final re-test I found that the code running on AWS differed from the template.
- Estimating cost with the AWS Pricing Calculator: the same Windows `t3.small` costs about $7.12 a month for 120 lab hours, and about $31.04 if left running 24/7. That gap is the reason Cost Guard exists.
- Teamwork through Git: branches, reviews, and not merging before the team lead agrees.

**5. Company Culture & Team Spirit**

We were a team of three, each with a clear part, and we helped each other when needed: one made the demo, one put the site together, and I re-tested and cleaned up. When I found a problem in a teammate's part, I posted it in the group with evidence, and it was fixed in a constructive way.

**6. Internship Policies / Benefits**

Flexible hours, and free access to AWS and community events (Cloud & AI Day, Student Community Day, Buildrathon). That is a big plus for students.

---

### Additional Questions

- **What did you find most satisfying during your internship?**

  Re-testing all three layers of Cost Guard on AWS and seeing the system stop the instance as designed: Layer 2 stopped it 3 min 54 s after the threshold was lowered, Layer 3 in 7 seconds. It was the first time I saw something I wrote run for real in the cloud with a measured result.

- **What do you think the company should improve for future interns?**

  Give each team its own lab account with a budget already set. My team shared one member's account and one IAM user. Sending a password through chat and managing permissions that way is not safe, and several services (for example Amazon Inspector) are blocked on the Free plan.

- **Would you recommend this internship to a friend? Why?**

  Yes. The program makes you build a real project from proposal, deployment and testing to cleanup, not just study theory. The events also let me meet people who work with the cloud every day.

---

### Suggestions & Expectations

- Give each team its own lab account as above, or at least show from the first week how to give each member their own IAM permissions.
- Keep important announcements (deadlines, templates, which parts are team or personal) on one fixed page, because messages in the group chat are easy to miss.
- I hope the program keeps the in-person meet-ups like the events at Bitexco, because that is where I learned the most from other people.
