---
title: "Blog 4"
date: 2026-11-26
weight: 4
chapter: false
pre: " <b> 3.4. </b> "
---

### AMAZON EVENTBRIDGE SCHEDULER: LÊN LỊCH CHẠY TÁC VỤ TRÊN AWS MÀ KHÔNG CẦN SERVER

**Đăng tại:** AWS Study Group (Facebook)

Mình đang thực tập tại AWS và dành thời gian đọc tài liệu về Amazon EventBridge Scheduler. Đây là dịch vụ để hẹn giờ chạy một việc nào đó (gọi Lambda, gửi tin SNS, dừng EC2...) mà không phải tự dựng server hay cron. Bài này là bản tóm tắt để bạn nào mới học AWS nắm nhanh bức tranh tổng thể.

**Các điểm chính cần nắm:**

- **EventBridge Scheduler là gì:** dịch vụ lập lịch serverless, cho phép tạo, chạy và quản lý tác vụ từ một nơi duy nhất. Theo tài liệu AWS, một lịch có thể gọi hơn 270 dịch vụ AWS và hơn 6.000 API, và dịch vụ chịu được hàng triệu lịch mà bạn không phải tự quản lý hạ tầng. AWS khuyến nghị dùng Scheduler thay cho rule lên lịch của EventBridge (scheduled rules), vì Scheduler mở rộng tốt hơn và hỗ trợ nhiều API đích hơn.
- **3 kiểu lịch:**
  - **Rate:** chạy đều theo chu kỳ, ví dụ `rate(5 minutes)`. Đơn vị là minutes, hours hoặc days.
  - **Cron:** chạy vào thời điểm cụ thể, gồm 6 trường (phút, giờ, ngày trong tháng, tháng, thứ trong tuần, năm). Ví dụ `cron(0 22 * * ? *)` là 22:00 mỗi ngày. Không dùng `*` ở cả "ngày trong tháng" và "thứ trong tuần", một bên phải là `?`.
  - **One-time:** chỉ chạy một lần, viết dạng `at(yyyy-mm-ddThh:mm:ss)`. Lịch một lần vẫn tính vào hạn mức tài khoản sau khi chạy xong, nên AWS khuyên xóa đi khi không cần nữa.
  - Mọi kiểu lịch đều có độ chính xác 60 giây: đặt 1:00 thì target được gọi trong khoảng 1:00:00 đến 1:00:59 (khi không bật flexible time window).
- **Múi giờ và giờ mùa hè:** với lịch cron và one-time, bạn chọn được múi giờ theo danh sách IANA, ví dụ `Asia/Ho_Chi_Minh`. Đây là điểm khác lớn so với rule kiểu cũ vốn tính theo UTC. Scheduler còn tự xử lý giờ mùa hè: khi đồng hồ chuyển tiến, lần chạy rơi vào giờ không tồn tại sẽ bị bỏ qua; khi đồng hồ lùi, lịch chỉ chạy một lần. Múi giờ không có giờ mùa hè (như Việt Nam) hoặc UTC thì không bị điều chỉnh.
- **Flexible time window và retry:** flexible time window cho phép rải lần chạy trong một khoảng thời gian thay vì đúng một thời điểm, phù hợp việc không cần chính xác tuyệt đối; tắt (Off) nếu cần chạy đúng giờ. Về retry, Scheduler đảm bảo gửi tác vụ "ít nhất một lần" (at-least-once). Bạn đặt được số lần thử lại và thời gian giữ lại tối đa cho lần gọi lỗi; các lần thử có độ trễ để target kịp sẵn sàng. Vì có thể gọi nhiều hơn một lần, hàm đích nên viết sao cho chạy lại không gây hại.
- **Target và quyền:** *templated target* là các mẫu dựng sẵn cho SQS, SNS, Lambda, EventBridge; *universal target* gọi bất kỳ API nào trong số hơn 270 dịch vụ. Scheduler cần một **execution role** để thay bạn gọi API đích: tạo lịch trên console thì AWS tự dựng role theo target đã chọn, còn dùng CLI, SDK hay CloudFormation thì bạn phải có sẵn role với đúng quyền cần thiết.
- **Hạn mức:** mặc định mỗi Region có tối đa 10.000.000 lịch (có thể nâng lên), cùng các giới hạn về số request tạo lịch và số lần gọi mỗi giây. Nên xem trong Service Quotas trước khi dùng ở quy mô lớn.

**Kết luận và cảm nghĩ:**

EventBridge Scheduler thay thế tốt cho việc tự dựng cron: có múi giờ, có retry, có nhiều kiểu lịch và gọi được rất nhiều dịch vụ. Điều mình thấy hữu ích nhất là chọn múi giờ ngay trên lịch và việc phải cấp execution role đúng quyền. Mình sẽ chọn rate cho việc lặp đều, cron khi cần giờ cụ thể, và luôn viết target chạy lại được vì Scheduler gửi "ít nhất một lần".

**Tham khảo:**

- [Amazon EventBridge Scheduler là gì](https://docs.aws.amazon.com/scheduler/latest/UserGuide/what-is-scheduler.html)
- [Các kiểu lịch, múi giờ và giờ mùa hè](https://docs.aws.amazon.com/scheduler/latest/UserGuide/schedule-types.html)
- [Hạn mức của EventBridge Scheduler](https://docs.aws.amazon.com/scheduler/latest/UserGuide/scheduler-quotas.html)
