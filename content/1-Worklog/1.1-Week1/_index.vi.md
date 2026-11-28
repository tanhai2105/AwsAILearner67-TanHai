---
title: "Worklog Tuần 1"
date: 2026-09-14
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---


### Mục tiêu tuần 1:

* Bắt nhịp với chương trình First Cloud AI Journey (FCAJ): làm quen mọi người, nắm nội quy văn phòng và tạo tài khoản FCAJ Portal.
* Có cái nhìn ban đầu về AWS, biết thao tác trên Management Console, xem loạt video FCAJ Workforce Bootcamp 2026 và cài Kiro lên máy.
* Làm quen trang Cloud Journey và làm các lab Explore đầu tiên: Free Tier, AWS Budget, AWS Support và IAM.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Ngày đầu vào chương trình, làm quen với các bạn và anh chị trong FCAJ <br> - Đọc nội quy văn phòng AWS, ghi lại mấy chỗ chưa rõ để hỏi mentor <br> - Lướt trang Cloud Journey để xem các bài lab và tài liệu được sắp xếp thế nào | 14/09/2026 | 14/09/2026 | <https://hcm-rules.awsfcaj.com/instructions> <br> <https://cloudjourney.awsfcaj.com/> |
| 3 | - Đăng ký tài khoản trên FCAJ Portal, xem cách đặt lịch lên văn phòng và cách đăng ký đồng phục <br> - Mở playlist FCAJ Workforce Bootcamp 2026 trên kênh YouTube AWS Study Group, xem video mở đầu: Prologue - Know before you join First Cloud AI Journey | 15/09/2026 | 15/09/2026 | <https://hcm-rules.awsfcaj.com/instructions/portal> <br> <https://youtube.com/playlist?list=PLahN4TLWtox3TSYFbN1DNX7NZgTVXNj3x> <br> <https://youtu.be/Gz56QzLQ_Yo> |
| 4 | - Module 01-01 - Introduction to AWS: cloud là gì và hạ tầng AWS được bố trí trên thế giới ra sao <br> - Module 01-02 - Management Console: phân biệt root user với IAM user, các cách làm việc với AWS <br> - Module 01-03 - Gen AI on AWS - Kiro: giới thiệu agentic AI và công cụ Kiro <br> - **Thực hành:** cài Kiro IDE và Kiro CLI trên máy | 16/09/2026 | 16/09/2026 | <https://youtu.be/qVCF7UjYC5s> <br> <https://youtu.be/95quNuhvMT0> <br> <https://youtu.be/uAQCm4sm_1c> |
| 5 | - Module 01-04 - Cost optimization on AWS: những cách chính để giảm chi phí <br> - Bắt đầu Module 1 - Explore trên Cloud Journey <br> - Lab AWS Free Tier 2026: các gói Free Tier và cách tạo tài khoản AWS | 17/09/2026 | 17/09/2026 | <https://youtu.be/UIw8UxGZCHA> <br> <https://cloudjourney.awsfcaj.com/1-explore/> <br> <https://000001.awsfcaj.com/> |
| 6 | - Lab Cost Management with AWS Budget: các loại budget và cảnh báo chi phí <br> - Lab AWS Support Request: các gói support và cách mở một case <br> - Lab Access Management with AWS IAM: group, user, role và switch role | 18/09/2026 | 18/09/2026 | <https://000007.awsfcaj.com/> <br> <https://000009.awsfcaj.com/> <br> <https://000002.awsfcaj.com/> |


### Kết quả đạt được tuần 1:

* Đã nắm nội quy và những điều cần lưu ý khi lên văn phòng, mấy chỗ chưa hiểu thì đã hỏi lại mentor. Biết Cloud Journey sắp xếp ra sao nên tìm hướng dẫn lab và tài liệu cho từng chủ đề nhanh hơn.

* Có tài khoản FCAJ Portal, biết chỗ đặt lịch lên văn phòng và chỗ đăng ký đồng phục.

* Hiểu được các ý chính của Module 01:

  * Cloud là thuê tài nguyên khi cần và dùng bao nhiêu trả bấy nhiêu
  * AWS chia thành các Region, mỗi Region có nhiều Availability Zone, ngoài ra còn Edge Location đặt gần người dùng
  * Root user nên cất kỹ, làm việc hằng ngày thì dùng IAM user
  * Có thể dùng AWS qua Console, CLI hoặc SDK

* Hiểu sơ ý tưởng của Kiro: viết spec trước rồi mới sinh code (spec-driven development), có cả bản IDE lẫn CLI. Cài xong Kiro IDE và Kiro CLI, mở lên chạy bình thường.

* Hiểu rõ hơn chuyện tối ưu chi phí: chọn đúng cỡ tài nguyên, tắt cái không dùng, theo dõi hóa đơn sớm chứ không đợi cuối tháng. Tự tạo tài khoản AWS dùng Free Tier, phân biệt được Free plan, Paid plan và phần credit đi kèm.

* Đặt budget cho tài khoản bằng AWS Budget kèm ngưỡng cảnh báo, và làm thử với AWS Support: mỗi gói support có gì và các bước mở một request.

* Thực hành IAM:

  * Tạo group, thêm user vào rồi gán policy cho group chứ không gán lẻ từng user
  * Tạo role và switch role để lấy quyền tạm thời
  * Cố gắng chỉ cấp đúng quyền cần dùng (least privilege)
