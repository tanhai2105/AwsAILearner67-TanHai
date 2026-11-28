---
title: "Week 11 Worklog"
date: 2026-11-23
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---


### Week 11 Objectives:

* Polish the Cost Guard workshop: screenshots, check lines, Vietnamese wording, alert email and architecture diagram.
* Re-test all 3 layers end to end on AWS and review the latest code change.
* Prepare the team files for FCAJ Submission.
* Write my personal sections and do the final check of the site before submitting.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 2 | - Add AWS Budgets screenshots to 5.6, and the missing screenshots plus a "Check" line under every image of 5.3–5.7 <br>- Rewrite the Vietnamese wording of the workshop <br>- Change the alert email to the team email (two stack updates, because the budget has a fixed name) <br>- Redraw the architecture diagram in draw.io with the official AWS icon package (07/2026) | 11/23/2026 | 11/23/2026 | <https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html> <br> <https://tanhai2105.github.io/AwsAILearner67-TanHai/5-workshop/5.6-layer3-backstop/> |
| 3 | - Review a teammate's change to the Cost Meter Lambda (cost computed from the instance LaunchTime) <br>- Start re-testing all 3 layers end to end on AWS | 11/24/2026 | 11/25/2026 | <https://tanhai2105.github.io/AwsAILearner67-TanHai/5-workshop/5.5-layer2-costmeter/> |
| 4 | - Finish the end-to-end test, compare the deployed code with the template and post the review <br>- Draft the team summary .md and the architecture .xml for FCAJ Submission | 11/25/2026 | 11/25/2026 | <https://tanhai2105.github.io/AwsAILearner67-TanHai/5-workshop/5.5-layer2-costmeter/> <br> <https://hcm-rules.awsfcaj.com/instructions/fcaj-submission> |
| 5 | - Write the personal sections: events, self-evaluation, feedback | 11/26/2026 | 11/26/2026 | <https://hcm-rules.awsfcaj.com/project/content> |
| 6 | - Final check of the whole site <br>- Rewrite my worklog following the team lead's feedback so my personal sections do not overlap with my teammates' | 11/27/2026 | 11/27/2026 | <https://hcm-rules.awsfcaj.com/project/content> |


### Week 11 Achievements:

* Workshop sections 5.3–5.7 now have every screenshot they need with a "Check" line under each image, and 5.6 includes the AWS Budgets screenshots; the Vietnamese text reads more naturally.

* Alerts now go to the team email. It took two stack updates because the budget uses a fixed name, which is something to remember when designing templates.

* The architecture diagram is redrawn in draw.io with the official AWS icon package (07/2026).

* Reviewed the Cost Meter Lambda change that computes cost from the instance LaunchTime, then re-tested all 3 layers on AWS:

  * Layer 2 stopped the instance 3 min 54 s after I lowered the threshold
  * Layer 3 stopped it in 7 s
  * The deployed code was not the same as the template; I wrote this up in the review I posted

* Drafted the team summary .md and the architecture .xml for FCAJ Submission.

* Finished the personal sections and checked the site again. Submitting the workshop link on the FCAJ Portal moves to week 12.
