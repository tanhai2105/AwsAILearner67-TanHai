---
title: "Triển khai nền tảng"
date: 2026-11-18
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

## Triển khai nền tảng

Ở bước này, bạn deploy một CloudFormation stack để tạo máy lab và những thành phần mà ba lớp dùng chung: cấu hình, dữ liệu, thông báo và IAM role. Phần giải thích và kiểm thử từng lớp nằm ở 5.4, 5.5 và 5.6.

### Stack tạo những gì

- **Máy lab:** một instance Windows Server 2022 `t3.small` với ổ gốc gp3 30 GB, gắn tag `Project=CostGuard`. Nếu bạn truyền `ExistingInstanceId`, stack sẽ bảo vệ instance đó thay vì tạo máy mới.
- **Parameter Store:** `/costguard/instance-id`, `/costguard/price-per-hour`, `/costguard/cost-threshold` và `/costguard/budget-limit`.
- **DynamoDB:** bảng `CostGuardSessions` ở chế độ on-demand, khóa phân vùng `instance_id` và khóa sắp xếp `timestamp`.
- **SNS:** các topic `costguard-alerts`, `costguard-errors` và `costguard-budgets`. Email của bạn đăng ký hai topic đầu.
- **IAM:** một role cho từng hàm Lambda, cộng một role cho EventBridge Scheduler.
- **Lambda, lịch, Budget, alarm:** được tạo ở đây và mô tả ở 5.4–5.6.

### Tham số

- `AlertEmail` (bắt buộc): hộp thư nhận cảnh báo.
- `ExistingInstanceId`: để trống để tạo máy lab mới.
- `PricePerHour`: mặc định 0.0392. Đây là đơn giá theo giờ của instance trong Pricing Calculator.
- `CostThreshold`: mặc định 14, khoảng 70% ngân sách.
- `BudgetLimit`: mặc định 20.
- `StopCron` và `StopScheduleTimezone`: mặc định `cron(0 22 * * ? *)` và `Asia/Ho_Chi_Minh`.
- `CostMeterRateMinutes`: mặc định 5.

### Bước 1 – Tạo stack

**Console**

1. Mở **CloudFormation** ở `us-east-1` và chọn **Create stack → With new resources**.
2. Chọn **Upload a template file** và chọn `cost-guard.yaml`.
3. Đặt tên stack là `CostGuard`.
4. Nhập email của bạn vào `AlertEmail`. Giữ nguyên các tham số khác.
5. Ở trang cuối, tick **I acknowledge that AWS CloudFormation might create IAM resources**, rồi chọn **Submit**.

Sau khi stack tạo xong, mở tab **Parameters** của stack để đối chiếu giá trị đã nhập.

![Tab Parameters của stack CostGuard](../../../images/5-Workshop/5.3-foundation/62-cg-stack-parameters.png)

**Kiểm tra:** `PricePerHour` là `0.0392`, `CostThreshold` là `14`, `BudgetLimit` là `20`, `StopCron` là `cron(0 22 * * ? *)` và `StopScheduleTimezone` là `Asia/Ho_Chi_Minh`. `AlertEmail` là email của bạn (đã che trong ảnh).

**AWS CLI**

```bash
aws cloudformation deploy \
  --region us-east-1 \
  --stack-name CostGuard \
  --template-file cost-guard.yaml \
  --capabilities CAPABILITY_IAM \
  --parameter-overrides AlertEmail=you@example.com
```

### Bước 2 – Xác nhận đăng ký email

1. Mở hộp thư. Bạn nhận hai thư từ Amazon SNS, một cho `costguard-alerts` và một cho `costguard-errors`.
2. Bấm **Confirm subscription** trong từng thư. Địa chỉ nào chưa xác nhận thì SNS không gửi gì tới.

![Hai đăng ký SNS của costguard-alerts và costguard-errors ở trạng thái Confirmed](../../../images/5-Workshop/5.3-foundation/49-sns-subscriptions-confirmed.png)

**Kiểm tra:** cả hai đăng ký của `costguard-alerts` và `costguard-errors` có Status **Confirmed**, không còn **Pending confirmation**.

### Bước 3 – Kiểm tra kết quả

1. Khi trạng thái stack là **CREATE_COMPLETE**, mở tab **Outputs**. `ProtectedInstanceId` là instance mà Cost Guard bảo vệ.
2. Kiểm tra từng tài nguyên trong console tương ứng:
   - **EC2:** instance `CostGuard-Lab` đang chạy.
   - **Systems Manager → Parameter Store:** có bốn tham số `/costguard/`.
   - **DynamoDB:** bảng `CostGuardSessions` ở trạng thái active.
   - **SNS:** có ba topic, và hai đăng ký đầu hiện **Confirmed**.
   - **Lambda:** có `CostGuard-ScheduleHandler`, `CostGuard-CostMeter` và `CostGuard-BudgetBackstop`.
   - **EventBridge → Scheduler:** `costguard-daily-stop` và `costguard-cost-meter` đang bật.

![Stack CostGuard: 29 tài nguyên ở trạng thái CREATE_COMPLETE](../../../images/5-Workshop/5.3-foundation/28-cg-stack-create-complete.png)

**Kiểm tra:** stack ở trạng thái **CREATE_COMPLETE** và không có tài nguyên nào ở trạng thái `CREATE_FAILED`.

![Máy lab t3.small đang chạy trong console EC2](../../../images/5-Workshop/5.3-foundation/54-ec2-t3small-running.png)

**Kiểm tra:** Instance type là `t3.small` và Instance state là **Running**.

**Kết quả trả về:** đủ mọi tài nguyên và máy đang chạy. Máy đã bắt đầu tính tiền, nên chuyển sang 5.4 luôn, đừng để lâu.
