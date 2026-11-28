---
title: "Worklog Tuần 10"
date: 2026-11-16
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---


### Mục tiêu tuần 10:

* Làm xong Experience 2 (bảo mật Windows trên AWS) trong tài khoản lab chung và viết lại thành bản nháp blog.
* Bắt đầu chủ đề mới của nhóm "Cost Guard for Windows Labs" và hoàn thành phần của mình: foundation, Layer 1 (tắt máy theo lịch), tính giá, workshop 5.1–5.4 và sửa proposal.
* Chuyển phần mình làm sang CloudFormation template chung của nhóm.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Experience 2 trong tài khoản lab chung (us-east-1, tài nguyên có tiền tố `exp2-`): <br>&emsp; + Đặt budget, tạo IAM role cho instance, SNS topic kèm subscription email <br>&emsp; + Recipe EC2 Image Builder có component STIG và pipeline (lần build đầu lỗi vì loại instance build không thuộc Free Tier, lần hai ra được AMI) <br>&emsp; + Bật AWS Config với rule `required-tags` và `approved-amis-by-id` <br>&emsp; + Rule EventBridge đẩy kết quả NON_COMPLIANT của Config sang SNS, test bằng 2 EC2 | 16/11/2026 | 17/11/2026 | <https://docs.aws.amazon.com/imagebuilder/latest/userguide/what-is-image-builder.html> <br> <https://docs.aws.amazon.com/config/latest/developerguide/WhatIsConfig.html> |
| 3 | - Làm tiếp Experience 2: <br>&emsp; + Vào instance bằng Session Manager để kiểm tra IMDSv2 <br>&emsp; + Amazon Inspector bị chặn ở gói Free nên thay bằng scan của SSM Patch Manager <br>&emsp; + RDP qua Fleet Manager <br>&emsp; + Viết CloudFormation template và IAM policy least privilege <br>&emsp; + Dọn sạch tài nguyên <br>- Viết 3 bản nháp blog | 17/11/2026 | 17/11/2026 | <https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager.html> <br> <https://docs.aws.amazon.com/systems-manager/latest/userguide/fleet-manager.html> <br> <https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html> |
| 4 | - Nhóm đổi chủ đề sang "Cost Guard for Windows Labs" (3 lớp serverless tự tắt Windows EC2 để giữ credit) <br>- Nhận phần của mình: foundation, Layer 1 (tắt theo lịch), tính giá, workshop 5.1–5.4, sửa proposal | 18/11/2026 | 18/11/2026 | <https://tanhai2105.github.io/AwsAILearner67-TanHai/vi/5-workshop/5.1-workshop-overview/> |
| 5 | - Deploy stack foundation (IAM role, SNS, DynamoDB, Parameter Store) và instance Windows EC2 cho lab <br>- Deploy Layer 1: EventBridge Scheduler lúc 22:00 Asia/Ho_Chi_Minh gọi Lambda ScheduleHandler <br>- Test tắt máy theo lịch | 19/11/2026 | 19/11/2026 | <https://docs.aws.amazon.com/scheduler/latest/UserGuide/what-is-scheduler.html> <br> <https://docs.aws.amazon.com/lambda/latest/dg/welcome.html> <br> <https://tanhai2105.github.io/AwsAILearner67-TanHai/vi/5-workshop/5.4-layer1-schedule/> |
| 6 | - Chuyển sang template chung `cost-guard.yaml` của nhóm <br>- Đổi instance sang t3.small, EBS từ gp2 sang gp3, tính lại giá bằng AWS Pricing Calculator <br>- Kiểm tra lịch 22:00 của stack mới <br>- Đẩy phần sửa và ảnh chụp lên repo nhóm, sửa sơ đồ kiến trúc | 20/11/2026 | 22/11/2026 | <https://calculator.aws/> <br> <https://tanhai2105.github.io/AwsAILearner67-TanHai/vi/5-workshop/5.3-foundation/> |


### Kết quả đạt được tuần 10:

* Làm trọn Experience 2 và dọn hết tài nguyên `exp2-` sau đó:

  * Image Builder cho mình một bài học thực tế: lần build đầu hỏng chỉ vì loại instance build không thuộc Free Tier, đổi lại thì lần hai ra được AMI đã hardening theo STIG
  * AWS Config (`required-tags`, `approved-amis-by-id`) cộng rule EventBridge đã đẩy được kết quả NON_COMPLIANT sang SNS, test với 2 EC2 cho kết quả đúng
  * Kiểm tra IMDSv2 qua Session Manager và vào máy bằng Fleet Manager RDP, không cần mở cổng RDP ra internet
  * Inspector không dùng được ở gói Free nên mình chuyển sang scan bằng SSM Patch Manager
  * Gói toàn bộ cấu hình vào một CloudFormation template kèm IAM policy least privilege

* Viết 3 bản nháp blog: Image Builder trên Free Tier; cảnh báo bằng AWS Config + EventBridge + SNS; quản lý Windows bằng SSM mà không mở RDP, theo least privilege.

* Deploy xong phần Cost Guard của mình: stack foundation (IAM role, SNS, DynamoDB, Parameter Store), instance Windows EC2 cho lab và Layer 1 (EventBridge Scheduler lúc 22:00 Asia/Ho_Chi_Minh → Lambda ScheduleHandler); test tắt máy theo lịch chạy đúng.

* Ngày 22/11 chuyển sang `cost-guard.yaml` chung: instance giờ là t3.small, ổ gp3 thay cho gp2, giá mới là $7.12/tháng cho 120 giờ lab so với $31.04 nếu chạy 24/7, lịch 22:00 của stack mới đã kiểm tra. Phần sửa, ảnh chụp và sơ đồ kiến trúc đã sửa đều nằm trên repo nhóm.
