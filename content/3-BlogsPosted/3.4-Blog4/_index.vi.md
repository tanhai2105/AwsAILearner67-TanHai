---
title: "Blog 4"
date: 2026-11-26
weight: 4
chapter: false
pre: " <b> 3.4. </b> "
---

### TỰ TẮT EC2 LÚC 22:00 GIỜ VIỆT NAM BẰNG EVENTBRIDGE SCHEDULER + LAMBDA (VÀ MẤY LỖI DỄ DÍNH)

**Đăng tại:** AWS Study Group (Facebook)

Đây là phần mình làm trong dự án Cost Guard của nhóm: Lớp 1, tự tắt máy Windows lab mỗi tối. Nghe đơn giản, nhưng có vài chỗ làm sai là máy tắt sai giờ hoặc không tắt luôn. Mình ghi lại cách làm và những điều học được.

![Lớp 1 – Schedule Guard: EventBridge Scheduler gọi Lambda ScheduleHandler lúc 22:00, hàm đọc instance ID từ Parameter Store, tắt máy EC2 và ghi sự kiện vào DynamoDB](../../../images/3-BlogsPosted/3.4-Blog4/layer1-schedule.png)

**Luồng chạy:** EventBridge Scheduler (22:00, `Asia/Ho_Chi_Minh`) → Lambda `CostGuard-ScheduleHandler` → đọc instance ID từ Parameter Store → máy đang chạy thì gọi `StopInstances`, không thì bỏ qua → ghi sự kiện vào DynamoDB → nếu lỗi thì gửi email qua SNS `costguard-errors`.

**Các điểm chính:**

- **Múi giờ: dùng Scheduler, đừng dùng rule cron kiểu cũ.** Cron của rule EventBridge kiểu cũ tính theo UTC. Muốn tắt lúc 22:00 giờ Việt Nam (UTC+7) thì phải viết 15:00 UTC, rất dễ quên và lệch 7 tiếng. EventBridge Scheduler cho chọn múi giờ ngay trên lịch. Nhóm mình dùng biểu thức `cron(0 22 * * ? *)`, time zone `Asia/Ho_Chi_Minh`, và tắt Flexible time window để chạy đúng 22:00.
- **Hàm Lambda nên "chạy lại bao nhiêu lần cũng được".** Hàm đọc trạng thái máy trước khi tắt: máy running thì gọi `StopInstances` và ghi `schedule_stop`; máy đã tắt thì không làm gì, chỉ ghi `schedule_noop`. Mỗi lần chạy đều ghi một dòng vào DynamoDB kèm trạng thái trước đó, nên test bao nhiêu lần cũng được, và nhìn bảng là biết tối nào máy thật sự bị tắt.
- **Quyền vừa đủ (least privilege).** Role của hàm chỉ được `StopInstances` trên đúng ARN của một máy lab. `DescribeInstances` không giới hạn theo resource được nên để `*`, nhưng quyền này chỉ đọc. Tài khoản lab dùng chung 3 người, giới hạn ARN giúp hàm không tắt nhầm máy của bạn khác. Scheduler cũng cần một role riêng (`scheduler.amazonaws.com`) chỉ có quyền `lambda:InvokeFunction` trên đúng hàm cần gọi; thiếu role này thì lịch vẫn hiện Enabled nhưng hàm không bao giờ được gọi.
- **Lỗi thì phải biết.** Khi có lỗi, hàm gửi email qua SNS `costguard-errors` rồi raise lại. Raise để metric Errors của Lambda tăng, CloudWatch alarm mới bắt được; nuốt lỗi thì alarm không bao giờ kêu. Nhóm đã test bằng cách đổi tạm `/costguard/instance-id` thành một ID không tồn tại: khoảng 6 phút sau alarm `CostGuard-CostMeter-Errors` chuyển sang In alarm, trả lại ID thật thì alarm về OK.
- **Test không cần chờ tới 22:00.** Gọi thẳng hàm với payload giống lịch gửi: `{"action": "stop"}`. Lần test ngày 25/11, máy chuyển từ running sang stopping sau khoảng 3 giây, hàm trả về `{"event": "schedule_stop", "state_before": "running"}`. Gọi lần hai khi máy đã tắt thì kết quả phải là `schedule_noop`. Sau 22:00 mở tab Monitoring của hàm hoặc bảng DynamoDB để chắc lịch thật đã chạy.

**Kết luận và cảm nghĩ:**

Cần giờ địa phương thì dùng EventBridge Scheduler kèm time zone. Hàm nên kiểm tra trạng thái trước khi làm và ghi lại mọi lần chạy. Quyền tắt máy giới hạn theo ARN, và lỗi phải raise để alarm thấy. Phần này nhỏ nhưng là lớp bảo vệ chắc nhất của Cost Guard, vì nó không phụ thuộc vào dữ liệu billing.

Workshop đầy đủ của nhóm: [nguyenducbinh-aws.github.io/AwsAILearner67](https://nguyenducbinh-aws.github.io/AwsAILearner67/)

**Tham khảo:**

- [EventBridge Scheduler: schedule types và time zone](https://docs.aws.amazon.com/scheduler/latest/UserGuide/schedule-types.html)
- [EC2 StopInstances](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_StopInstances.html)
