---

title: "Dọn Dẹp"
weight: 5
chapter: false
pre: " <b> 5.5 </b> "
---

#### Overview

Trong phần này, chúng ta sẽ xoá những gì đã khởi tạo trong workshop lần này



### Xóa một Agent trong AgentCore Runtime

1. Truy cập vào **Amazon Bedrock AgentCore** trên AWS Console.
2. Trong menu bên trái, chọn **Build → Runtime**.
3. Tại danh sách **Runtime resources**, bạn sẽ thấy các agent đã tạo, ví dụ: `agent_demo_2` hoặc `agent_with_memory`.
4. Chọn agent bạn muốn xóa bằng cách tick vào radio button ở cột **Name**.
5. Nhấn nút **Delete** ở góc phải.
6. Xác nhận thao tác xóa khi AWS yêu cầu. Agent sẽ bị xóa vĩnh viễn khỏi danh sách Runtime resources.




![Xóa agent trong Amazon Bedrock AgentCore](/images/5-Workshop/5.5-Clean/clean.png)


### Xóa một Memory trong AgentCore

Sau khi xóa agent trong Runtime, bạn có thể quản lý hoặc xóa **Memory** mà agent đó sử dụng:

1. Trong menu bên trái, chọn **Build → Memory**.
2. Tại danh sách các Memory resources, chọn memory bạn muốn xóa.
3. Nhấn nút **Delete** ở góc phải.
4. Xác nhận thao tác xóa khi AWS yêu cầu. Memory sẽ bị xóa khỏi hệ thống.






![Xóa Memory trong Amazon Bedrock AgentCore](/images/5-Workshop/5.5-Clean/clean_memory.png)
