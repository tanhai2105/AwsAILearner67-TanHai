---
title: "Worklog Tuần 4"
date: 2026-09-28
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---


### Mục tiêu tuần 4:

* Làm việc trên terminal với AWS CloudShell và AWS CLI.
* Hiểu Amazon S3 lưu dữ liệu ra sao và Amazon CloudFront phân phối thế nào.
* Host trang tĩnh trên S3, đặt CloudFront phía trước và tập quản lý object.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Kiến thức cơ bản về S3: <br>&emsp; + Bucket và object <br>&emsp; + Storage class <br>&emsp; + Versioning và replication <br> - **Thực hành:** <br>&emsp; + Tạo bucket <br>&emsp; + Đưa file website mẫu lên bucket | 28/09/2026 | 28/09/2026 | <https://000057.awsfcaj.com/> |
| 3 | - **Thực hành:** <br>&emsp; + Bật static website hosting, chọn index.html làm trang chính <br>&emsp; + Tắt Block public access, đặt ACL public cho object <br>&emsp; + Mở site bằng website endpoint và ghi lại thời gian tải | 29/09/2026 | 29/09/2026 | <https://000057.awsfcaj.com/> |
| 4 | - Tìm hiểu Amazon CloudFront <br> - **Thực hành:** <br>&emsp; + Bật lại Block all public access cho bucket <br>&emsp; + Tạo CloudFront distribution lấy bucket làm origin, sửa bucket policy <br>&emsp; + Mở site qua CloudFront, so thời gian tải với endpoint của S3 | 30/09/2026 | 30/09/2026 | <https://000057.awsfcaj.com/> |
| 5 | - Tổng quan AWS CloudShell <br> - **Thực hành:** <br>&emsp; + Mở môi trường CloudShell <br>&emsp; + Gõ thử pwd, ls -la, mkdir, touch, vi, cat <br>&emsp; + Dùng AWS CLI xem phiên bản, liệt kê bucket S3 và instance EC2 <br>&emsp; + Đưa file lên CloudShell và tải file về máy <br>&emsp; + Xóa môi trường | 01/10/2026 | 01/10/2026 | <https://000049.awsfcaj.com/> |
| 6 | - Quản lý object trong S3: <br>&emsp; + Versioning <br>&emsp; + Di chuyển object <br>&emsp; + Replication sang Region khác <br> - **Thực hành:** <br>&emsp; + Bật versioning, sửa index.html rồi tải lên lại, xem danh sách phiên bản <br>&emsp; + Chuyển object sang bucket thứ hai <br>&emsp; + Tạo replication rule sang bucket ở N. Virginia <br>&emsp; + Dọn tài nguyên lab | 02/10/2026 | 02/10/2026 | <https://000057.awsfcaj.com/> |


### Kết quả đạt được tuần 4:

* Thấy CloudShell khá tiện: mở trình duyệt là có terminal, AWS CLI đăng nhập sẵn. Mình tập lệnh Linux cơ bản trên đó, dùng CLI liệt kê bucket với instance, và chuyển file qua lại.

* Hiểu S3 lưu object trong bucket, còn storage class quyết định giá và tốc độ truy cập. Đã tạo bucket và đưa file website lên.

* Đưa được website tĩnh lên chạy từ S3. Muốn truy cập được thì phải tắt Block public access và mở object bằng ACL, mà cách này chỉ chạy HTTP qua website endpoint.

* Sau đó sửa lại cho an toàn: bật lại chặn public và cho CloudFront đọc bucket bằng bucket policy. Qua CloudFront trang tải nhanh hơn, xem được edge location (PoP) nào trả kết quả.

* Tập quản lý object:

  * Bật versioning rồi thì index.html bị ghi đè vẫn lấy lại được bản cũ
  * Chuyển object giữa các bucket
  * Replication rule tự chép object mới sang bucket ở Region khác, coi như có bản dự phòng
