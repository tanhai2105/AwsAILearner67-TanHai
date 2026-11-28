---
title: "Worklog Tuần 3"
date: 2026-09-28
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---


### Mục tiêu tuần 3:

* Làm việc trên terminal với AWS CloudShell và AWS CLI.
* Hiểu Amazon S3 lưu dữ liệu ra sao và Amazon CloudFront phân phối thế nào.
* Host trang tĩnh trên S3, đặt CloudFront phía trước và tập quản lý object.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Học cơ bản về S3: bucket với object, các storage class, versioning và replication <br> - **Thực hành:** <br>&emsp; + Tạo một bucket <br>&emsp; + Tải file website mẫu lên | 28/09/2026 | 28/09/2026 | <https://000057.awsfcaj.com/> |
| 3 | - **Thực hành:** <br>&emsp; + Bật static website hosting với index.html làm trang chủ <br>&emsp; + Tắt Block public access và đặt object ở chế độ public bằng ACL <br>&emsp; + Truy cập site qua website endpoint, ghi lại thời gian tải | 29/09/2026 | 29/09/2026 | <https://000057.awsfcaj.com/> |
| 4 | - Đọc về Amazon CloudFront <br> - **Thực hành:** <br>&emsp; + Bật lại Block all public access cho bucket <br>&emsp; + Tạo CloudFront distribution với bucket là origin và cập nhật bucket policy <br>&emsp; + Mở site qua CloudFront, so thời gian tải với website endpoint của S3 | 30/09/2026 | 30/09/2026 | <https://000057.awsfcaj.com/> |
| 5 | - Xem tổng quan AWS CloudShell <br> - **Thực hành:** <br>&emsp; + Mở CloudShell và chạy thử vài lệnh Linux (pwd, ls -la, mkdir, touch, vi, cat) <br>&emsp; + Dùng AWS CLI: xem version, liệt kê bucket S3 và EC2 instance <br>&emsp; + Upload file lên CloudShell rồi download về máy <br>&emsp; + Xóa môi trường CloudShell | 01/10/2026 | 01/10/2026 | <https://000049.awsfcaj.com/> |
| 6 | - Quản lý object trên S3: versioning, di chuyển object, replication sang Region khác <br> - **Thực hành:** <br>&emsp; + Bật versioning, sửa index.html và upload lại, xem các phiên bản <br>&emsp; + Đưa object sang bucket thứ hai <br>&emsp; + Tạo replication rule sang bucket ở N. Virginia <br>&emsp; + Dọn tài nguyên lab | 02/10/2026 | 02/10/2026 | <https://000057.awsfcaj.com/> |


### Kết quả đạt được tuần 3:

* Nắm được S3 chứa object trong bucket và storage class ảnh hưởng tới giá lẫn tốc độ truy cập. Mình đã tạo bucket và tải file của website lên.

* Cho website tĩnh chạy từ S3. Để mở được thì phải tắt Block public access và cấp quyền cho object bằng ACL; kiểu này chỉ chạy HTTP trên website endpoint.

* Sau đó xử lý cho an toàn hơn: bật lại chặn truy cập public, để CloudFront đọc bucket thông qua bucket policy. Trang tải nhanh hơn khi đi qua CloudFront, và mình xem được edge location (PoP) nào phục vụ request.

* CloudShell khá tiện vì mở trình duyệt là có sẵn terminal với AWS CLI đã đăng nhập. Mình chạy thử các lệnh Linux cơ bản, dùng CLI để liệt kê bucket và instance, và upload/download file.

* Thực hành quản lý object trong S3:

  * Khi đã bật versioning, file index.html bị ghi đè vẫn khôi phục được về phiên bản trước
  * Đưa object từ bucket này sang bucket khác
  * Replication rule tự sao chép object mới sang bucket ở Region khác, tương đương một bản sao lưu
