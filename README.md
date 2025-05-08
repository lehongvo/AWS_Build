# Học AWS Cơ Bản

Chào mừng bạn đến với tài liệu học AWS!

## Mục tiêu
- Hiểu các dịch vụ cơ bản của AWS
- Biết cách tạo và quản lý tài nguyên trên AWS
- Ứng dụng AWS vào các dự án thực tế

## Các chủ đề sẽ học
- Giới thiệu về AWS và Cloud Computing
- AWS IAM (Identity and Access Management)
- Amazon EC2 (Elastic Compute Cloud)
- Amazon S3 (Simple Storage Service)
- AWS Lambda
- AWS CLI và SDK
- Quản lý chi phí và bảo mật trên AWS

## Hướng dẫn bắt đầu
1. Đăng ký tài khoản AWS tại https://aws.amazon.com/
2. Cài đặt AWS CLI:
   ```bash
   brew install awscli
   # hoặc
   curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
   sudo installer -pkg AWSCLIV2.pkg -target /
   ```
3. Cấu hình AWS CLI:
   ```bash
   aws configure
   ```
4. Tham khảo tài liệu chính thức: https://docs.aws.amazon.com/

## Hướng dẫn tạo và chạy 1 EC2 instance bằng AWS CLI

### 1. Tạo key pair để SSH vào EC2
```bash
yarn global add aws-cli # Nếu chưa cài aws-cli, dùng yarn
aws ec2 create-key-pair --key-name my-key-pair --query 'KeyMaterial' --output text > my-key-pair.pem
chmod 400 my-key-pair.pem
```

### 2. Tìm AMI ID cho Amazon Linux 2 (ở us-east-1)
```bash
aws ec2 describe-images \
  --owners amazon \
  --filters "Name=name,Values=amzn2-ami-hvm-*-x86_64-gp2" \
  --query 'Images[*].[ImageId,CreationDate]' \
  --output text | sort -k2 -r | head -n 1
```
- Lấy phần `ami-xxxxxxxxxxxxxxxxx` để dùng ở bước sau.

### 3. Tạo security group cho phép SSH (port 22)
```bash
aws ec2 create-security-group --group-name my-ec2-sg --description "Allow SSH"
```
- Lấy GroupId trả về, ví dụ: `sg-xxxxxxxxxxxxxxx`

Mở port 22:
```bash
aws ec2 authorize-security-group-ingress --group-name my-ec2-sg --protocol tcp --port 22 --cidr 0.0.0.0/0
```

### 4. Tạo EC2 instance
```bash
aws ec2 run-instances \
  --image-id ami-xxxxxxxxxxxxxxxxx \
  --count 1 \
  --instance-type t2.micro \
  --key-name my-key-pair \
  --security-groups my-ec2-sg
```
- Thay `ami-xxxxxxxxxxxxxxxxx` bằng AMI ID ở bước 2.

### 5. Lấy public IP của instance
```bash
aws ec2 describe-instances --filters "Name=instance-state-name,Values=running" --query "Reservations[*].Instances[*].[InstanceId,PublicIpAddress]" --output table
```
- Copy địa chỉ IP để SSH.

### 6. SSH vào EC2
```de
ssh -i my-key-pair.pem ec2-user@<Public-IP>
```
- `<Public-IP>` là IP vừa lấy ở bước trên.

### 7. Xóa (terminate) EC2 instance

Để xóa một EC2 instance, bạn cần biết InstanceId của instance muốn xóa. Có thể lấy InstanceId từ lệnh ở bước 5 hoặc từ AWS Console.

```bash
yarn global add aws-cli # Nếu chưa cài aws-cli, dùng yarn
aws ec2 terminate-instances --instance-ids <InstanceId>
```
- Thay `<InstanceId>` bằng ID của instance bạn muốn xóa, ví dụ: `i-0abcd1234efgh5678`.

Sau khi chạy lệnh này, instance sẽ chuyển sang trạng thái "shutting-down" và sau đó là "terminated". Bạn sẽ không bị tính phí cho instance đã bị terminate.

---
**Lưu ý:**
- Nếu gặp lỗi permission, security group, hoặc không tìm thấy AMI, hãy kiểm tra lại các bước hoặc gửi lỗi để được hỗ trợ.
- Nếu muốn tạo instance ở region khác, thêm tham số `--region <region>` vào các lệnh trên.

## Ghi chú
- Luôn kiểm tra chi phí khi sử dụng dịch vụ AWS.
- Xóa tài nguyên không sử dụng để tránh phát sinh phí.

---

*File này sẽ được cập nhật khi có thêm nội dung học tập mới.* # AWS_Build
