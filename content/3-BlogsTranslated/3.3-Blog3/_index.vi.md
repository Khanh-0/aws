---
title: "Cách Strangeworks sử dụng Amazon Braket để khám phá vấn đề xếp hàng hóa lên máy bay"
date: 2025-09-02
weight: 3
chapter: false
pre: "<b>3. </b>"
---

{{% notice info %}}
Bài viết gốc được đăng trên [AWS Quantum Technologies Blog](https://aws.amazon.com/blogs/quantum-computing/)
{{% /notice %}}

Máy tính lượng tử hứa hẹn đem lại bước ngoặt cho việc tính toán các bài toán ở nhiều ngành công nghiệp, mặc dù vẫn còn là câu hỏi mở là công nghệ này sẽ hữu ích ra sao trong thực tế. Trong bài blog này, nhóm từ Strangeworks, một đối tác AWS, đánh giá các triển khai khác nhau của thuật toán **QAOA (Quantum Approximate Optimization Algorithm)** đối với vấn đề xếp hàng hóa lên máy bay do Airbus đặt ra như một phần của [Quantum Mobility Challenge](https://qcc.thequantuminsider.com/) năm trước.

## Tổng quan về vấn đề

Vấn đề xếp hàng hóa máy bay mà Airbus đưa ra là dạng bài toán tối ưu kiểu **bin packing** (đóng hộp) xảy ra trong nhiều ngành - du lịch, sản xuất, logistics. Những bài toán kiểu này rất khó giải vì số lượng các khả năng tăng theo cấp số nhân khi biến đổi nhiều hơn, dẫn đến không gian bài toán rất lớn ngay cả với các trường hợp đơn giản.

### Tại sao sử dụng máy tính lượng tử?

Máy tính lượng tử có thể phù hợp để giải các bài toán tối ưu này, có tiềm năng tăng tốc so với giải pháp cổ điển (classical). Tuy nhiên, khi phần cứng lượng tử hiện tại vẫn chưa vượt trội so với các công nghệ cổ điển, các phương pháp heuristic kết hợp xử lý lượng tử và cổ điển (hybrid) đang được xem xét nhiều.

{{% notice note %}}
Hiện tại phần cứng lượng tử kiểu "gate-based" chưa vượt qua được công nghệ cổ điển tốt nhất, nhưng phương pháp benchmark trong bài này đạt được kết quả chính xác cho các bài toán lên đến **80 qubit/biến**.
{{% /notice %}}

## Các thuật toán QAOA được sử dụng

Thuật toán QAOA thuộc loại **hybrid quantum-classical**, nghĩa là nó sử dụng cả phần tính toán lượng tử và phần tính toán cổ điển, được tin là phù hợp trong thời đại NISQ (noisy intermediate-scale quantum). Nhóm Strangeworks dựa trên hai thuật toán QAOA có sẵn:

1. **QAOA tiêu chuẩn (Standard QAOA)**: Có trong nhiều thư viện mã nguồn mở bao gồm [Braket algorithm library](https://aws.amazon.com/blogs/quantum-computing/introducing-the-amazon-braket-algorithm-library/)

2. **Relax-and-Round QAOA**: Một biến thể được phát triển bởi đội của Rigetti Computing

3. **StrangeworksQAOA**: Biến thể riêng với cải tiến trong phần xử lý cổ điển, vượt trội so với QAOA chuẩn trong bài toán này

## Sử dụng Braket Hybrid Jobs

Kết hợp tài nguyên lượng tử và cổ điển thông qua các thuật toán hybrid như QAOA là con đường để sử dụng máy tính lượng tử hiện có cùng với luồng công việc cổ điển. Nhóm dùng [Amazon Braket Hybrid Jobs](https://aws.amazon.com/blogs/aws/introducing-amazon-braket-hybrid-jobs-set-up-monitor-and-efficiently-run-hybrid-quantum-classical-workloads/), cho phép toàn bộ workflow QAOA được gửi như một job duy nhất.

### Lợi ích của Braket Hybrid Jobs

- **Giảm thời gian chờ**: Thuật toán chỉ phải chờ trong hàng đợi Braket một lần, thay vì chờ mỗi lần gửi mạch lượng tử riêng lẻ

- **Cached circuit compilation**: Cho phép các mạch kế tiếp nếu có cấu trúc giống mạch trước đó dùng lại phần biên dịch trước đó, tiết kiệm thời gian và chi phí

### Tích hợp với nền tảng Strangeworks

Người dùng có thể truy cập Braket và các tính năng này qua [platform Strangeworks Compute](https://strangeworks.com/). Nền tảng này có hệ thống quản lý công việc trực tuyến và SDK để tải xuống, cho phép người dùng truy cập StrangeworksQAOA, Braket Hybrid Jobs cũng như các thiết bị lượng tử, lượng tử cảm hứng ("quantum-inspired") và các solver cổ điển.

## Phương pháp benchmark

Để benchmark StrangeworksQAOA, nhóm xét bài toán tối ưu xếp hàng hóa lên máy bay, được hình thành ban đầu như một phần của thử thách tính toán lượng tử của Airbus. Bài toán này cố gắng tối đa hóa khối lượng hàng được xếp lên máy bay, đồng thời đảm bảo rằng số lượng container (n) có thể vừa vào số chỗ có sẵn (N).

### Các trường hợp kiểm tra

**Bảng 1: Các giá trị xét cho số container (n) và số chỗ có sẵn trên máy bay (N)**

| Containers (n) | Spaces (N) | Số biến/qubits |
|---------------|-----------|----------------|
| 5 | 3 | 26 |
| 6 | 4 | 38 |
| 6 | 5 | 46 |
| 7 | 5 | 52 |
| 7 | 6 | 61 |
| 8 | 6 | 68 |
| 8 | 7 | 78 |

### Kết quả so sánh

![Hình 1: So sánh các thuật toán QAOA](/images/Blog/Blog_3/bl3_1.png)

*Hình 1: So sánh máy lượng tử Rigetti Ankaa-3 chạy thuật toán StrangeworksQAOA (vòng tròn liền) với Standard QAOA (ô vuông liền), cả hai dùng framework Braket Hybrid Job. Ngoài ra, còn có kết quả từ QRR của Rigetti (ô vuông gạch) và StrangeworksQRR (vòng tròn gạch).*

{{% notice tip %}}
Kết quả được lấy trung bình trên 8 lần chạy. Thanh lỗi thống kê nhỏ so với bề rộng đường biểu diễn cho thấy kết quả ổn định với bài toán được xét.
{{% /notice %}}

### Phân tích gradient

**Bảng 2: Gradient của đường khớp cho mỗi thuật toán**

| Thuật toán | Gradient |
|-----------|----------|
| Gurobi | -1.25 |
| StrangeworksQAOA | 3.47 |
| Standard QAOA | n.a |
| Strangeworks QRR QAOA | -0.16 |
| Rigetti QRR QAOA | 1.06 |

{{% notice warning %}}
Standard QAOA tăng theo cấp số (exponential) nên không thể có một fit tuyến tính hợp lý cho trường hợp đó.
{{% /notice %}}

Phân tích gradient cho thấy phiên bản Strangeworks của QRR có gradient gần nhất với kết quả chính xác (Gurobi), nghĩa là phiên bản này sẽ tiếp tục có sai số nhỏ nhất khi kích thước hệ thống tăng.

## Chi tiết thuật toán StrangeworksQAOA

Thuật toán QAOA là một thuật toán hybrid cổ điển-lượng tử lặp trong một vòng giữa phần tính toán lượng tử và xử lý cổ điển.

![Hình 2: QAOA workflow](/images/Blog/Blog_3/bl3_2.png)

*Hình 2: QAOA workflow - (a) thiết lập mạch với tham số biến phân cổ điển và áp dụng lên qubit; (b) đo ra bitstring; (c) lặp nhiều lần để tạo phân bố xác suất; (d) tính chi phí và dùng thuật toán tối ưu hoá cổ điển để sinh mạch mới với tham số cập nhật*

### Quy trình hoạt động

1. **Tham số hoá mạch lượng tử**: Bắt đầu với tập tham số cổ điển θ. Nhóm sử dụng **Real Amplitude Quantum Circuit** thay vì ansatz QAOA tiêu chuẩn do hạn chế độ sâu mạch

2. **Chạy trên QPU**: Mạch được chạy trên QPU Rigetti Ankaa-3, đo trạng thái qubit (0 hoặc 1), lặp nhiều lần để thu được phân bố xác suất

3. **Tính hàm chi phí**: Phân bố xác suất được đưa vào hàm cổ điển để tính chi phí

4. **Tối ưu hoá**: Sử dụng phương pháp **COBYLA** từ gói SciPy để cập nhật tham số biến phân

### Điểm khác biệt của StrangeworksQAOA

Trong **Standard QAOA**, đáp án thường được chọn từ bitstring có xác suất cao nhất. Tuy nhiên, với hệ lớn, phân bố xác suất trở nên phẳng, mỗi trạng thái chỉ được đo một lần.

Trong **StrangeworksQAOA**, nhóm:
- Theo dõi giá trị chi phí C_q nhỏ nhất trong suốt các vòng lặp
- Báo cáo trạng thái cơ sở có chi phí tối thiểu là lời giải
- Việc này hợp lý vì lời giải cho bài toán xếp hàng hóa là một đáp án cổ điển đơn

### Công thức tính chi phí

Chi phí cho trạng thái cơ sở thứ q:

![Công thức chi phí](/images/Blog/Blog_3/bl3_3.png)

Tổng chi phí trung bình:

![Công thức tổng chi phí](/images/Blog/Blog_3/bl3_4.png)

Trong đó:
- q_i ∈ {0,1} là giá trị bit thứ i trong bitstring thứ q
- J_{i,j} là các hệ số coupling của QUBO/đồ thị vấn đề
- N_q là số lần trạng thái cơ sở được đo

## Kết luận và triển vọng

Phân tích cho thấy giá trị của việc tối ưu hoá các thuật toán lượng tử cho ứng dụng cụ thể. Các phát hiện chính:

- **StrangeworksQAOA** bền vững hơn trước các bất cập của thuật toán lai lượng-cổ điển
- Kết hợp giữa nền tảng Strangeworks, StrangeworksQAOA, Braket Hybrid Jobs và thiết bị Rigetti Ankaa-3 thể hiện con đường thực tế để giải các bài toán tối ưu
- Phương pháp tinh chỉnh kỹ càng có thể đạt kết quả tốt hơn mà không tốn thêm chi phí

{{% notice success %}}
Độ tin cậy và khả năng tái lập của kết quả trên bài toán xếp hàng hóa gợi mở một hướng đi khả quan cho việc đạt **lợi thế lượng tử thực tiễn** (practical quantum advantage).
{{% /notice %}}

### Bắt đầu sử dụng

Để khám phá các thuật toán này cho thách thức tối ưu của bạn:
- Truy cập [nền tảng Strangeworks](https://strangeworks.com/)
- Xem [tài liệu về QAOA](https://docs.strangeworks.com/algorithms/qaoa)
- Bắt đầu với Amazon Braket

## Tài liệu tham khảo

1. Romero, S., Osaba, E., Villar-Rodriguez, E., Oregi, I. & Ban, Y. *Hybrid approach for solving real-world bin packing problem instances using quantum annealers.* Sci Rep 13, 11777 (2023).

2. Farhi, E., Goldstone, J. & Gutmann, S. *A Quantum Approximate Optimization Algorithm.* arXiv:1411.4028 (2014).

3. Dupont, M. & Sundar, B. *Extending relax-and-round combinatorial optimization solvers with quantum correlations.* PhysRevA.109.012429 (2024).

4. [Airbus Quantum Computing Challenge](https://www.airbus.com/sites/g/files/jlcbta136/files/2021-10/Airbus-QuantumComputing-Challenge-PS5.pdf)

5. Pilon, G., Gugole, N. & Massarenti, N. *Aircraft Loading Optimization -- QUBO models under multiple constraints.* arXiv:2102.09621 (2021).

6. Guerreschi, G. G. *Solving Quadratic Unconstrained Binary Optimization with divide-and-conquer and quantum algorithms.* arXiv:2101.07813v1 (2021).

7. [Gurobi Optimization](https://www.gurobi.com/)

8. [QAOA Circuit Ansatz](https://qiskit.org/documentation/stubs/qiskit.circuit.library.RealAmplitudes.html)

9. [Real Amplitude Quantum Circuit Ansatz](https://qiskit.org/documentation/stubs/qiskit.circuit.library.RealAmplitudes.html)

10. [Braket QAOA Example](https://github.com/amazon-braket/amazon-braket-examples/blob/main/examples/pennylane/2_Graph_optimization_with_QAOA/2_Graph_optimization_with_QAOA.ipynb)

## Về các tác giả

### Stuart Flannigan
Senior Quantum Software Engineer tại Strangeworks, có nền tảng phân tích các thí nghiệm mô phỏng lượng tử. Từ khi gia nhập Strangeworks 3 năm trước, anh đã làm việc để kết hợp các ý tưởng lý thuyết của hệ thống lượng tử vào quy trình làm việc cổ điển và AI, giúp công nghệ này dễ tiếp cận hơn với ngành công nghiệp.

### Andrew J. Ochoa

Chief Scientist tại Strangeworks, lãnh đạo các sáng kiến nghiên cứu và phát triển. Có bằng Tiến sĩ Vật lý từ Texas A&M University và MBA từ UT McCombs. Đã có nhiều công trình xuất bản về tính toán lượng tử và làm việc tại Strangeworks từ năm 2018.

### Michael Brett
![Michael Brett](/images/Blog/Blog_3/blavt1.png)
Principal Specialist for Quantum Computing trong nhóm High Performance Computing tại AWS. Lãnh đạo các nỗ lực phát triển kinh doanh và tiếp thị toàn cầu cho Amazon Braket. Trước đây là SVP for Applications tại Rigetti Computing và CEO của QxBranch.

### Charunethran Panchalam Govindarajan
![Charunethran Panchalam Govindarajan](/images/Blog/Blog_3/blavt2.png)
Sr. Product Marketing Manager tại AWS, tập trung vào High-Performance Computing và Quantum Technologies. Có bằng Thạc sĩ Kỹ thuật Điện từ Stanford University. Ngoài công việc, thích vẽ và các cuộc trò chuyện triết học.

---

*Bài viết được đăng vào 02 THÁNG 9, 2025 trong [Amazon Braket](https://aws.amazon.com/blogs/quantum-computing/category/quantum-technologies/amazon-braket/), [Quantum Technologies](https://aws.amazon.com/blogs/quantum-computing/category/quantum-technologies/)*