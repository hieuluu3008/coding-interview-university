# Phân Tích Thuật Toán - Ký Hiệu Tiệm Cận (Asymptotic Notation)

## Định Nghĩa

**Tiệm cận (Asymptotic Notation)** là tập hợp các ký hiệu dùng để ước lượng thời gian chạy của thuật toán thông qua **số chỉ thị (machine instructions)** hay số bước thực hiện, độc lập với các yếu tố như tốc độ máy hay ngôn ngữ lập trình.

Mục tiêu: so sánh hiệu năng giữa các thuật toán một cách tổng quát, không phụ thuộc vào phần cứng hay môi trường thực thi.

---

## Hai Khía Cạnh Quan Trọng

### 1. Độ Phức Tạp Theo Kích Thước Input

Thời gian chạy được biểu diễn như một hàm của kích thước dữ liệu nhập `n`.

Ví dụ:
```
F(n) = 9n² + 6n + 9
```

### 2. Tỷ Suất Gia Tăng (Rate of Growth)

Tập trung vào cách thức thời gian chạy **tăng như thế nào** khi kích thước input `n` tăng, thay vì giá trị tuyệt đối.

---

## Đơn Giản Hóa Biểu Thức

Khi phân tích, ta **bỏ qua các hệ số hằng số** và **các số hạng ít quan trọng hơn** (bậc thấp hơn), chỉ giữ lại số hạng chiếm ưu thế khi `n` lớn.

Ví dụ:
```
F(n) = 9n² + 6n + 9  →  O(n²)
```

Lý do: khi `n` rất lớn, `9n²` áp đảo hoàn toàn so với `6n` và `9`.

---

## Các Ký Hiệu Tiệm Cận

### 1. Big-O (O) — Giới Hạn Trên (Upper Bound)

Biểu diễn trường hợp **tệ nhất (worst case)**. Hàm `f(n) = O(g(n))` nghĩa là tồn tại hằng số `c > 0` và `n₀` sao cho:

```
f(n) ≤ c * g(n)  với mọi n ≥ n₀
```

Ví dụ: `O(n)`, `O(n²)`, `O(n log n)`, `O(1)`

> Dùng khi muốn đảm bảo thuật toán **không chạy lâu hơn** một ngưỡng nhất định.

---

### 2. Big-Omega (Ω) — Giới Hạn Dưới (Lower Bound)

Biểu diễn trường hợp **tốt nhất (best case)**. Hàm `f(n) = Ω(g(n))` nghĩa là tồn tại hằng số `c > 0` và `n₀` sao cho:

```
f(n) ≥ c * g(n)  với mọi n ≥ n₀
```

Ví dụ: `Ω(n)`, `Ω(log n)`

> Dùng khi muốn biết thuật toán **ít nhất phải tốn bao nhiêu thời gian**.

---

### 3. Big-Theta (Θ) — Giới Hạn Chặt (Tight Bound)

Biểu diễn **cả giới hạn trên lẫn giới hạn dưới**. Hàm `f(n) = Θ(g(n))` nghĩa là:

```
c₁ * g(n) ≤ f(n) ≤ c₂ * g(n)  với mọi n ≥ n₀
```

Tức là: `f(n) = O(g(n))` **và** `f(n) = Ω(g(n))`

Ví dụ: `Θ(n log n)`, `Θ(n²)`

> Dùng khi thời gian chạy được xác định **chính xác** theo bậc tăng trưởng.

---

## Bảng So Sánh Nhanh

| Ký hiệu | Ý nghĩa | Trường hợp |
|---------|---------|-----------|
| `O(g(n))` | Không tệ hơn `g(n)` | Worst case (tệ nhất) |
| `Ω(g(n))` | Không tốt hơn `g(n)` | Best case (tốt nhất) |
| `Θ(g(n))` | Xấp xỉ đúng bằng `g(n)` | Average / Tight bound |

---

## Thứ Tự Độ Phức Tạp Phổ Biến (từ tốt đến tệ)

```
O(1) < O(log n) < O(n) < O(n log n) < O(n²) < O(2ⁿ) < O(n!)
```

---

## Ví Dụ Thực Tế

| Thuật toán | Độ phức tạp |
|-----------|------------|
| Truy cập phần tử mảng | O(1) |
| Tìm kiếm nhị phân | O(log n) |
| Duyệt mảng | O(n) |
| Merge Sort, Heap Sort | O(n log n) |
| Bubble Sort, Selection Sort | O(n²) |
| Fibonacci đệ quy ngây thơ | O(2ⁿ) |

---

## Lưu Ý Khi Sử Dụng

- Trong thực tế, **Big-O** được dùng phổ biến nhất vì ta quan tâm đến worst case.
- Các ký hiệu này mô tả **tốc độ tăng trưởng**, không phải thời gian tuyệt đối.
- Hai thuật toán có cùng Big-O không có nghĩa là chạy nhanh như nhau — hằng số ẩn vẫn có vai trò với input nhỏ.

---

*Nguồn tham khảo: [blog.daynhauhoc.com](https://blog.daynhauhoc.com/phan-tich-thuat-toan-tiem-can-asymptotic-notation/)*
