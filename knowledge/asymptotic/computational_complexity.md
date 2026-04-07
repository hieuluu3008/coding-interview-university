# Độ Phức Tạp Tính Toán (Computational Complexity)

---

## 1. Ký pháp Asymptotic (Asymptotic Notation)

Độ phức tạp tính toán dùng ký pháp toán học để mô tả tốc độ tăng trưởng của hàm số khi kích thước đầu vào $N$ tiến đến vô cùng.

| Ký hiệu | Tên | Ý nghĩa |
|---------|-----|---------|
| $O(g(N))$ | Big-O | **Cận trên** — $f(N)$ không bao giờ vượt quá $c \cdot g(N)$ với mọi $N > N_0$ |
| $\Omega(g(N))$ | Omega | **Cận dưới** — $f(N)$ tăng trưởng ít nhất nhanh bằng $g(N)$ |
| $\Theta(g(N))$ | Theta | **Cận chặt** — vừa là $O(g(N))$ vừa là $\Omega(g(N))$, cùng tốc độ tăng trưởng |

**Big-O** là ký hiệu phổ biến nhất — nó đảm bảo thuật toán sẽ không chậm hơn một mức nhất định.

---

## 2. Các lớp độ phức tạp phổ biến

Xếp theo thứ tự từ nhanh đến chậm:

| Độ phức tạp | Tên | Ví dụ |
|------------|-----|-------|
| $O(1)$ | Hằng số | Truy cập phần tử mảng theo index |
| $O(\log N)$ | Logarit | Binary search — cơ số log không quan trọng trong Big-O |
| $O(N)$ | Tuyến tính | Duyệt qua một danh sách |
| $O(N \log N)$ | Tuyến tính logarit | MergeSort, HeapSort |
| $O(N^2)$ | Bình phương | Vòng lặp lồng nhau, MinSort/BubbleSort |
| $O(N^k)$ | Đa thức | $k$ là hằng số |
| $O(2^N)$ | Lũy thừa | Backtracking, vét cạn |
| $O(N!)$ | Giai thừa | Liệt kê hoán vị — không xử lý được với $N > 11$ trong 8 giây |

> **Giới hạn thực tế (competitive programming):** Với 8 giây, $O(N^2)$ xử lý tối đa $N \approx 10.000$.

---

## 3. Phân tích trường hợp Tốt nhất / Trung bình / Xấu nhất

- **Worst case (xấu nhất):** Trọng tâm chính — cung cấp **hạn định trên** chắc chắn cho mọi đầu vào kích thước $N$.
- **Best case / Average case:** Ít dùng hơn; thường dùng để so sánh thuật toán trên "hầu hết đầu vào".
- **Tối ưu tiệm cận:** Thuật toán tối ưu khi đạt cận dưới lý thuyết của bài toán.
  Ví dụ: MergeSort đạt $\Theta(N \log N)$ — tối ưu cho sắp xếp dựa trên so sánh.

---

## 4. Cách phân tích độ phức tạp

### 4.1 Mã không đệ quy

- **Quy tắc ngón tay cái:** Ước tính số lần tối đa mỗi vòng lặp thực hiện.
- **Phép cộng:** Các vòng lặp nối tiếp → cộng biên.
- **Phép nhân:** Các vòng lặp lồng nhau → nhân biên.
- **Bỏ qua thành phần bậc thấp:** $1.5N^2 - 0.5N = O(N^2)$ — số hạng $-0.5N$ không ảnh hưởng khi $N$ lớn.

### 4.2 Mã đệ quy (Chia để trị)

Phương trình đệ quy dạng: $f(N) = a \cdot f(N/b) + p(N)$

Có 3 phương pháp giải:

1. **Substitution method (thay thế):** Đoán cận trên rồi chứng minh bằng quy nạp toán học.

2. **Recursion tree (cây đệ quy):** Vẽ sơ đồ các lần gọi hàm → tính tổng công việc tại mỗi tầng → cộng tất cả tầng.

3. **Master Theorem (Định lý Thợ):** Công cụ chính thức với 3 trường hợp:
   - **Case 1:** Công việc ở nút **lá** chiếm ưu thế.
   - **Case 2:** Công việc **trải đều** các tầng → kết quả thường là $p(N) \log N$.
   - **Case 3:** Công việc ở nút **gốc** chiếm ưu thế.

### 4.3 Độ phức tạp không gian (Space Complexity)

Lượng bộ nhớ thuật toán sử dụng so với kích thước đầu vào.
Ví dụ: thuật toán lưu mảng $N$ phần tử → $\Theta(N)$ space.

---

## 5. Các lưu ý quan trọng

- **Kích thước đầu vào $N$:** Có thể là số phần tử mảng, số đỉnh/cạnh đồ thị, hoặc số bit biểu diễn một số.
- **Hằng số ẩn:** Big-O bỏ qua hằng số ($10N^2$ và $10^7 N^2$ đều là $O(N^2)$), nhưng nếu hằng số quá lớn cần xem xét trong thực tế.
- **Tính độc lập phần cứng:** Big-O dự đoán hiệu suất mà không cần biết tốc độ máy cụ thể.
  Ví dụ: đầu vào tăng gấp đôi → thuật toán $O(N^2)$ chạy chậm gấp **4 lần**.
- **Cơ số logarit:** Không quan trọng trong Big-O vì $\log_a N$ và $\log_b N$ chỉ khác nhau bởi một hằng số.

---

## Nguồn

- [Computational Complexity part one](https://www.topcoder.com/thrive/articles/Computational%20Complexity%20part%20one)
- [Computational Complexity part two](https://www.topcoder.com/thrive/articles/Computational%20Complexity%20part%20two)
