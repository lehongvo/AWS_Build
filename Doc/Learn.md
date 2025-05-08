# EC2 - Nội dung cần học và câu hỏi kiểm tra kiến thức

## 1. Nội dung cần học về EC2

1. Khái niệm EC2 là gì?
   - **Amazon EC2 (Elastic Compute Cloud)** là một dịch vụ điện toán đám mây thuộc AWS, cung cấp khả năng thuê máy chủ ảo (instance) với tài nguyên linh hoạt, giúp người dùng triển khai, quản lý và mở rộng ứng dụng một cách dễ dàng, tiết kiệm chi phí. EC2 cho phép bạn khởi tạo, cấu hình, dừng, khởi động lại hoặc hủy các máy chủ ảo theo nhu cầu sử dụng thực tế.
2. Các loại instance (Instance Types)
   - EC2 cung cấp nhiều loại instance phù hợp với các mục đích khác nhau, ví dụ:
     - **General Purpose (T, M series):** Cân bằng giữa CPU, RAM, và network.
     - **Compute Optimized (C series):** Tối ưu cho xử lý tính toán.
     - **Memory Optimized (R, X series):** Tối ưu cho ứng dụng cần nhiều RAM.
     - **Storage Optimized (I, D series):** Tối ưu cho lưu trữ tốc độ cao.
     - **Accelerated Computing (P, G series):** Tối ưu cho AI/ML, GPU.
3. Cách tạo và cấu hình một EC2 instance
   - Để tạo một EC2 instance, bạn cần:
     1. Chọn AMI (Amazon Machine Image) phù hợp.
     2. Chọn loại instance.
     3. Cấu hình network (VPC, subnet).
     4. Thiết lập key pair để truy cập SSH.
     5. Cấu hình security group (firewall).
     6. Gán storage (EBS).
     7. Khởi tạo instance và truy cập qua SSH hoặc RDP.
4. AMI (Amazon Machine Image) là gì?
   - **AMI** là một template chứa thông tin cấu hình (hệ điều hành, phần mềm, cài đặt) dùng để khởi tạo EC2 instance. Bạn có thể sử dụng AMI do AWS cung cấp, do cộng đồng chia sẻ, hoặc tự tạo AMI riêng để chuẩn hóa môi trường triển khai.
5. Key Pair và bảo mật truy cập SSH
   - **Key Pair** gồm public key (lưu trên AWS) và private key (lưu trên máy bạn). Khi tạo EC2, bạn phải chọn hoặc tạo key pair để truy cập instance qua SSH (Linux) hoặc RDP (Windows). Không chia sẻ private key và nên sử dụng key pair khác nhau cho các môi trường khác nhau.
6. Security Groups và cách cấu hình firewall
   - **Security Group** là firewall ảo kiểm soát inbound/outbound traffic cho EC2. Bạn có thể cấu hình rule cho phép hoặc chặn truy cập theo IP, port, protocol. Nên chỉ mở các port cần thiết (ví dụ: 22 cho SSH, 80/443 cho web).
7. Elastic IP là gì? Khác gì với Public IP?
   - **Elastic IP** là địa chỉ IPv4 tĩnh, có thể gán cho bất kỳ EC2 instance nào trong tài khoản AWS. Khác với Public IP (được gán động và thay đổi khi stop/start instance), Elastic IP giữ nguyên cho đến khi bạn release, giúp duy trì địa chỉ IP cố định cho dịch vụ.
8. EBS (Elastic Block Store) và các loại storage khác
   - **EBS** là dịch vụ lưu trữ block, dùng làm ổ cứng cho EC2. Có thể mở rộng, snapshot, attach/detach giữa các instance. Ngoài ra còn có Instance Store (lưu trữ tạm thời, mất dữ liệu khi stop/terminate) và EFS (Elastic File System) cho lưu trữ file chia sẻ.
9. Lifecycle của một EC2 instance (Start, Stop, Terminate, Reboot)
   - **Start:** Khởi động instance.
   - **Stop:** Dừng instance, giữ lại dữ liệu EBS.
   - **Terminate:** Xóa instance, dữ liệu EBS có thể bị xóa tùy cấu hình.
   - **Reboot:** Khởi động lại instance mà không thay đổi trạng thái lưu trữ.
10. Cách scale EC2 (Auto Scaling Group, Launch Template)
   - **Auto Scaling Group (ASG)** tự động tăng/giảm số lượng EC2 instance dựa trên nhu cầu thực tế (CPU, network, schedule...). **Launch Template** giúp chuẩn hóa cấu hình khi tạo mới instance trong ASG.
11. Pricing models: On-Demand, Reserved, Spot, Savings Plans
   - **On-Demand:** Trả tiền theo giờ/giây sử dụng, không cam kết.
   - **Reserved Instance:** Cam kết sử dụng 1-3 năm, giá rẻ hơn.
   - **Spot Instance:** Đấu giá, giá rẻ nhưng có thể bị thu hồi bất cứ lúc nào.
   - **Savings Plans:** Cam kết chi tiêu tối thiểu, linh hoạt loại instance.
12. Monitoring với CloudWatch
   - **CloudWatch** giám sát tài nguyên EC2 (CPU, RAM, disk, network...), gửi cảnh báo khi vượt ngưỡng, hỗ trợ log, dashboard, tự động scale.
13. Cách backup và restore EC2 (AMI, Snapshot)
   - **AMI:** Backup toàn bộ instance (OS, data, config).
   - **Snapshot:** Backup từng volume EBS, có thể dùng để restore hoặc tạo volume mới.
14. User Data và Metadata
   - **User Data:** Script chạy khi instance khởi tạo, dùng để cài đặt phần mềm, cấu hình tự động.
   - **Metadata:** Thông tin về instance (ID, IP, role...) truy cập qua http://169.254.169.254.
15. Network: VPC, Subnet, ENI, Placement Group
   - **VPC:** Mạng ảo riêng biệt trong AWS.
   - **Subnet:** Phân vùng nhỏ trong VPC.
   - **ENI (Elastic Network Interface):** Card mạng ảo gán cho instance.
   - **Placement Group:** Tối ưu hiệu năng network giữa các instance.
16. Cách kết nối EC2 với các dịch vụ AWS khác (S3, RDS, Lambda, v.v.)
   - EC2 có thể kết nối với các dịch vụ khác qua network, IAM role, VPC endpoint, hoặc SDK/API. Ví dụ: gắn IAM role để EC2 truy cập S3 mà không cần key, kết nối RDS qua private subnet.
17. Best practices khi sử dụng EC2
   - Sử dụng key pair và security group hợp lý.
   - Không dùng root account để vận hành.
   - Sử dụng IAM role thay vì access key.
   - Theo dõi chi phí và sử dụng CloudWatch để giám sát.
   - Thường xuyên backup và kiểm tra restore.
   - Cập nhật hệ điều hành và phần mềm thường xuyên.
   - Chỉ mở port cần thiết, hạn chế truy cập từ internet.

---

## 2. Câu hỏi trắc nghiệm kiểm tra kiến thức EC2

1. EC2 là viết tắt của cụm từ nào?
   - a) Elastic Compute Cloud
   - b) Elastic Cloud Compute
   - c) Elastic Container Cloud
   - d) Elastic Compute Cluster

2. Để truy cập vào EC2 instance qua SSH, bạn cần gì?
   - a) Username và password
   - b) Key Pair (private key)
   - c) IAM Role
   - d) MFA Token

3. Security Group trong EC2 dùng để làm gì?
   - a) Lưu trữ dữ liệu
   - b) Quản lý truy cập mạng (firewall)
   - c) Tạo snapshot
   - d) Quản lý billing

4. AMI là gì?
   - a) Một loại instance
   - b) Ảnh hệ điều hành dùng để launch instance
   - c) Một loại storage
   - d) Một loại network

5. Elastic IP khác gì với Public IP thông thường?
   - a) Elastic IP là tĩnh, Public IP là động
   - b) Elastic IP chỉ dùng cho S3
   - c) Elastic IP chỉ dùng cho Lambda
   - d) Không có sự khác biệt

6. Để tự động tăng/giảm số lượng EC2 instance theo tải, bạn dùng dịch vụ nào?
   - a) CloudWatch
   - b) Auto Scaling Group
   - c) S3
   - d) IAM

7. Loại pricing nào phù hợp cho workload chạy liên tục, lâu dài?
   - a) On-Demand
   - b) Reserved Instance
   - c) Spot Instance
   - d) Free Tier

8. Để giám sát CPU, RAM, Disk của EC2, bạn dùng dịch vụ nào?
   - a) CloudTrail
   - b) CloudWatch
   - c) S3
   - d) IAM
