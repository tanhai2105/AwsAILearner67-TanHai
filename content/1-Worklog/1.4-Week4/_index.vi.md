---
title: "Worklog Tuần 4"
date: 2026-10-05
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---


### Mục tiêu tuần 4:

* Cùng nhóm bàn ý tưởng cho proposal và workshop.
* Học Amazon RDS và dựng trọn một database MySQL trên đó.
* Đọc hai blog AWS: Fleet Manager để truy cập Windows và CloudWatch Omni để theo dõi AI agent.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Đọc và ghi chú bài blog giới thiệu Fleet Manager của AWS Systems Manager | 05/10/2026 | 05/10/2026 | <https://aws.amazon.com/vi/blogs/mt/console-based-access-to-windows-instances-using-aws-systems-manager-fleet-manager/> |
| 3 | - Đọc và ghi chú bài blog ra mắt Amazon CloudWatch Omni | 06/10/2026 | 06/10/2026 | <https://aws.amazon.com/vi/blogs/aws/introducing-amazon-cloudwatch-omni-ai-powered-observability-for-generative-ai-and-agentic-workloads/> |
| 4 | - Đọc tài liệu RDS: <br>&emsp; + Các database engine RDS hỗ trợ <br>&emsp; + Multi-AZ và Read Replica <br>&emsp; + Backup tự động và snapshot <br>&emsp; + Bảo mật bằng mã hóa, VPC và IAM | 07/10/2026 | 07/10/2026 | <https://000005.awsfcaj.com/> |
| 5 | - **Thực hành:** <br>&emsp; + VPC, Security Group và DB subnet group <br>&emsp; + Một instance RDS MySQL <br>&emsp; + EC2 kết nối tới RDS bằng MySQL client <br>&emsp; + Tạo database, tạo bảng, thêm dữ liệu <br>&emsp; + Triển khai app dùng RDS <br>&emsp; + Snapshot và restore <br>&emsp; + Dọn dẹp | 08/10/2026 | 08/10/2026 | <https://000005.awsfcaj.com/> |
| 6 | - Ngồi cùng nhóm brainstorm chủ đề cho proposal và workshop | 09/10/2026 | 09/10/2026 | <https://tanhai2105.github.io/AwsAILearner67-TanHai/vi/2-proposal/> |


### Kết quả đạt được tuần 4:

* Rút ra từ blog Fleet Manager:

  * RDP vào máy Windows ngay trên trình duyệt, đi qua Session Manager
  * Không phải mở cổng RDP, không cần public IP hay bastion host, nên ít chỗ bị tấn công hơn
  * Đăng nhập bằng key pair EC2 hoặc tài khoản Windows, xem được tối đa 4 máy cùng lúc
  * Máy cần có SSM Agent, instance profile có quyền Systems Manager và đường kết nối tới SSM endpoint

* Rút ra từ blog CloudWatch Omni:

  * AI agent mỗi lần chạy có thể đi một hướng khác, nên phải trace từng lần gọi LLM và gọi tool mới tìm ra bước bị lỗi
  * Latency hay tỷ lệ lỗi không nói được câu trả lời tốt hay dở, Omni có sẵn 17 evaluator như coherence, helpfulness, faithfulness để chấm
  * Playground, Experiments và Prompt Management giúp so prompt, so cấu hình và quay về bản cũ
  * Dùng trong VS Code, Kiro hoặc bản web, dựa trên OpenInference và ADOT, hỗ trợ LangChain, CrewAI, Strands và nhiều framework khác

* RDS lo giúp mấy việc lặp đi lặp lại của database (vá lỗi, backup, failover). Mình nắm được các engine nó hỗ trợ, Multi-AZ giữ bản standby ở AZ khác ra sao, và mã hóa, VPC, IAM bảo vệ database thế nào.

* Làm hết lab RDS:

  * Instance MySQL nằm ở private subnet do DB subnet group chọn, nên chỉ EC2 trong VPC mới vào được
  * Từ EC2 dùng MySQL client tạo database, tạo bảng và thêm vài dòng dữ liệu
  * Chạy một ứng dụng dùng database đó
  * Chụp snapshot rồi restore ra instance mới, kiểm tra thấy dữ liệu vẫn còn

* Nhóm đã có vài ý tưởng ban đầu cho proposal và workshop để tiếp tục phát triển.
