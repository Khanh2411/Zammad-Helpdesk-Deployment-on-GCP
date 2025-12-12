# Overview
![](image/zammad_logo.png)  
Dự án này trình bày việc triển khai và cấu hình Zammad, một hệ thống hỗ trợ/quản lý yêu cầu mã nguồn mở, trên Google Cloud Platform (GCP) với môi trường sẵn sàng cho sản xuất.  
Mục tiêu là xây dựng một hệ thống hỗ trợ đầy đủ chức năng bao gồm quản lý yêu cầu, tích hợp email, tiện ích trò chuyện trực tuyến, quy tắc tự động hóa và bảo mật SSL.  

Dự án này bao gồm:  
- Cung cấp một máy ảo (VM) trên GCP và triển khai Zammad với Nginx.
- Bảo mật hệ thống bằng HTTPS (Let’s Encrypt).
- Thiết lập các kênh email (IMAP/SMTP) để tạo và trả lời yêu cầu hỗ trợ.
- Cấu hình tiện ích trò chuyện web và nhúng nó vào trang web.
- Triển khai Nhóm, Vai trò, Kích hoạt và SLA để mô phỏng quy trình hỗ trợ thực tế.
- Ghi chép cấu hình quản trị như lập lịch, giám sát và backup.  
Việc triển khai này mô phỏng cách thức hoạt động của các nhóm hỗ trợ CNTT hiện đại và làm nổi bật các kỹ năng quản trị Linux, cơ sở hạ tầng đám mây, mạng và tích hợp hệ thống.

# Cài đặt & cấu hình Zammad
## Thiết lập môi trường Zammad
Chọn VPC trên Google Cloud Platform
![](image/ChooseVPC.png)  

OS được chọn để cài đặt Ubuntu 22.04 LTS, 30 Gb ổ cứng.  
![](image/OS-VPC.png)  

Cho phép HTTP, HTTPS truy cập WebUI Zammad.  
![](image/Network-VPC.png)  

## Cài đặt Zammad
Cài đặt các gói cần thiết.  
![](image/instal-package.png)  

Cài đặt Elasticsearch
![](image/install-elastic1.png)  
![](image/install-elastic2.png)  

Cấu hình trong file /etc/elasticsearch/elasticsearch.yml  
![](image/edit-conf-elastic.png)  

# Email Channel
# Live Chat Channel
# Tích hợp Website
# Groups – Roles – Users – Permissions
# Backup & Restore
