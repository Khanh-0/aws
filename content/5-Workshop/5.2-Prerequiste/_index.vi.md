---

title : "Các bước chuẩn bị"
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

#### IAM permissions

## Các quyền cần thêm

* AdministratorAccess
* AmazonBedrockFullAccess
* AWSCodeBuildAdminAccess
* AWSCodeBuildDeveloperAccess
* BedrockAgentCoreFullAccess

## Tạo user và gán quyền

1. Vào **IAM** → **Users** → chọn **Create user**.
2. Thêm các quyền đã liệt kê ở trên.
3. Hoàn tất tạo user và lưu Access Key nếu cần dùng SDK.
![iam](/aws/static/images/5-Workshop/5.2-Prerequisite/iam.png)

#### Tải AWS CLI

Tải AWS CLI:
[**AWS CLI Link**](https://s3.amazonaws.com/aws-cli/AWSCLI64PY3.msi)

Sau đó cài đặt theo hướng dẫn.

![CLI](/aws/static/images/5-Workshop/5.2-Prerequisite/cli.png)



## Cấu hình UV management

#### 1. Vì sao dùng UV?

UV nhanh, nhẹ, quản lý môi trường tốt hơn pip.



#### 2. Cài đặt UV trên Windows

Chạy lệnh:

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```


Thêm UV vào PATH:

```powershell
$env:Path = "C:\Users\leamo\.local\bin;$env:Path"
```
![uvpath](/aws/static/images/5-Workshop/5.2-Prerequisite/uv.png)

> Khởi động lại máy để nhận PATH mới.



#### 3. Khởi tạo môi trường UV

Trong thư mục dự án:

```bash
uv init
```

![runuv](/aws/static/images/5-Workshop/5.2-Prerequisite/uvinit.png)

Sau đó chọn environment trong VS Code.


## Kết nối máy với AWS CLI

Vào lại IAM để tạo Access Key.



#### Tạo Access Key và cấu hình AWS CLI

1. Trong user: **Security credentials** → **Create access key**
2. Chọn loại **Command Line Interface (CLI)**



#### Cấu hình AWS CLI

Chạy lệnh:

```bash
aws configure
```

Nhập lần lượt:

* **AWS Access Key ID**
* **AWS Secret Access Key**
* **Default region name** (demo: `ap-southeast-1`)
* **Default output format**

  ```bash
  json
  ```

![conectCLI](/aws/static/images/5-Workshop/5.2-Prerequisite/conectcli.png)



## Khởi chạy AWS CLI AgentCore

Chạy:

```bash
uv run which agentcore
```
sau khi chạy sẽ tải các thư viện cần thiết cho AWS Agentcore về máy

![run agentcore](/aws/static/images/5-Workshop/5.2-Prerequisite/whichagent.png)

## Tạo Groq API
bạn vào trang [Groq](https://console.groq.com/keys) và tạo api như hình.Đây là các tool bên ngoài bổ trợ cho Rag của liên kết thông qua AWS Agentcore
![groqapi](/aws/static/images/5-Workshop/5.2-Prerequisite/groqapi.png)


