# CSE 373 – Lecture 2: Thuật toán, Counter-Example & Ký hiệu Tiệm cận

---

## 1. Triết lý thiết kế thuật toán

Phần lớn các thuật toán mà sinh viên thiết kế lần đầu tiên đều **sai**. Kỹ năng then chốt là:
1. Đề xuất một heuristic (chiến lược trực giác).
2. Tìm **counter-example** — ví dụ cụ thể cho thấy heuristic đó thất bại.
3. Lặp lại cho đến khi tìm được thuật toán đúng và hiệu quả.

**Nguyên tắc counter-example tốt:** Càng đơn giản càng thuyết phục. Không cần dùng số âm hay trường hợp đặc biệt nếu không cần thiết.

---

## 2. Bài toán Movie Star Scheduling (Interval Scheduling)

### Đề bài
Bạn là một diễn viên, được trả flat-fee cho mỗi bộ phim. Cho danh sách các phim với thời gian bắt đầu và kết thúc. **Không thể đóng hai phim chồng lịch.** Tìm tập con phim lớn nhất mà không có phim nào trùng lịch nhau.

### Các heuristic sai & counter-example

| Heuristic | Ý tưởng | Counter-example |
|---|---|---|
| **Shortest Job First** | Chọn phim ngắn nhất để còn thời gian | Phim ngắn nhất nằm giữa 2 phim khác → chọn nó chặn cả 2 phim đó |
| **Earliest Start Time** | Chọn phim bắt đầu sớm nhất | Phim đầu tiên là bộ phim "War and Peace" kéo dài cả năm → chặn tất cả các phim khác |
| **Exhaustive Search** | Thử mọi tập con (2ⁿ tập con) | Đúng nhưng **quá chậm** với n lớn |

### Thuật toán đúng: Earliest Completion Date

**Thuật toán:**
```
Sắp xếp các phim theo thời gian kết thúc (tăng dần)
Lặp:
    Chọn phim kết thúc sớm nhất
    Xóa tất cả phim chồng lịch với phim vừa chọn
```

**Proof by contradiction:**
- Giả sử tồn tại optimal solution *không chứa* phim kết thúc sớm nhất (gọi là job J).
- **Case 1:** Optimal solution chứa job K chồng lịch với J. Vì J kết thúc trước K, ta có thể *thay K bằng J* → vẫn có cùng số phim, thậm chí mở thêm slot (vì J kết thúc sớm hơn) → mâu thuẫn với "optimal".
- **Case 2:** Optimal solution không chứa J và không chứa bất kỳ job nào chồng lịch với J. Khi đó ta có thể *thêm J vào* → được nhiều phim hơn → mâu thuẫn với "optimal".

Vậy chọn phim kết thúc sớm nhất **không bao giờ sai**.

**Độ phức tạp:** O(n log n) — chủ yếu do bước sắp xếp.

---

## 3. Bài toán Knapsack (Integer Subset Sum)

### Đề bài
Cho tập số nguyên S và mục tiêu T. Tìm tập con của S mà tổng bằng đúng T.

*Ví dụ:* S = {1, 2, 5, 9, 10}, T = 22 → đáp án {1, 2, 9, 10}.

### Các heuristic sai & counter-example

| Heuristic | Counter-example |
|---|---|
| **Left-to-Right (First Fit)** | S = {1, 2, 3}, T = 4 → chọn 1, 2, không nhét được 3 → kết quả 3. Nhưng {1, 3} = 4 ✓ |
| **Smallest to Largest** | Cùng ví dụ trên (đã sorted). Đơn giản hơn: S = {1, 2}, T = 2 → chọn 1, không còn chỗ cho 2, nhưng {2} = 2 ✓ |
| **Largest to Smallest** | S = {2, 3, 4}, T = 5 → chọn 4, không còn chỗ cho 2 hay 3. Nhưng {2, 3} = 5 ✓ |

### Kết luận
Không có heuristic đơn giản nào đảm bảo đúng. Bài toán Knapsack ở dạng tổng quát là **NP-hard** — không có thuật toán polynomial được biết đến. Exhaustive search (2ⁿ tập con) là đúng nhưng quá chậm.

---

## 4. Mô hình RAM (Random Access Machine)

Để phân tích thuật toán **độc lập với máy tính**, dùng mô hình RAM:

| Giả định | Chi tiết |
|---|---|
| **Unit-time instructions** | Mỗi phép toán cơ bản (cộng, trừ, so sánh) = 1 bước |
| **Loops & subroutines** | Không phải 1 bước — phải đếm từng bước bên trong |
| **Memory access** | Mỗi lần đọc/ghi bộ nhớ = 1 bước |
| **Không quan tâm hardware** | Cache, disk latency bị bỏ qua (model có thể sai ~1000x nhưng đủ để đo order of magnitude) |

**Mục đích:** Xác định **order of magnitude** của running time — thuật toán tăng theo n như thế nào?

---

## 5. Ký hiệu Tiệm cận (Asymptotic Notation)

Hai khái niệm tách biệt:
1. **Loại complexity** đang phân tích (best case, worst case, average case)
2. **Ký hiệu** để mô tả (O, Ω, Θ)

**Tại sao dùng Worst Case?** Dễ tính toán hơn và cho **guaranteed bound** — biết chắc chắn thuật toán không bao giờ chậm hơn mức đó.

### 5.1 Big-O — Upper Bound (Chặn trên)

**Định nghĩa:**
f(n) = O(g(n)) ⟺ ∃ hằng số dương c và n₀ sao cho:
$$f(n) \leq c \cdot g(n) \quad \forall n > n_0$$

**Ví dụ:** f(n) = 3n² − 100n + 6

- Chọn c = 4, n₀ = 3: với mọi n > 3, ta có 4n² ≥ 3n² − 100n + 6
  → f(n) = **O(n²)** ✓
- Cũng đúng khi nói f(n) = O(n³) vì n³ là upper bound lớn hơn, nhưng **không tight**.

### 5.2 Big-Ω — Lower Bound (Chặn dưới)

**Định nghĩa:**
f(n) = Ω(g(n)) ⟺ ∃ hằng số dương c và n₀ sao cho:
$$f(n) \geq c \cdot g(n) \quad \forall n > n_0$$

**Ví dụ:** f(n) = 3n² − 100n + 6

- Chọn c = 1: với n đủ lớn, n² ≤ 3n² − 100n + 6
  → f(n) = **Ω(n²)** ✓
- f(n) = Ω(n) cũng đúng (lower bound lỏng hơn)
- f(n) **≠** Ω(n³): không có hằng số c > 0 nào để c·n³ ≤ n² với mọi n lớn

### 5.3 Big-Θ — Tight Bound (Chặn khít)

**Định nghĩa:**
f(n) = Θ(g(n)) ⟺ ∃ hằng số dương c₁, c₂ và n₀ sao cho:
$$c_2 \cdot g(n) \leq f(n) \leq c_1 \cdot g(n) \quad \forall n > n_0$$

Tức là f(n) vừa là O(g(n)) vừa là Ω(g(n)).

**Ví dụ:** f(n) = 3n² − 100n + 6 = **Θ(n²)**
- Upper: c₁ = 4 (Big-O)
- Lower: c₂ = 1 (Big-Ω)
- Cả hai dùng cùng hàm n² → Θ(n²) ✓

### Tóm tắt bảng ký hiệu

| Ký hiệu | Ý nghĩa | Tương tự |
|---|---|---|
| O(g(n)) | f ≤ c·g (upper bound) | ≤ |
| Ω(g(n)) | f ≥ c·g (lower bound) | ≥ |
| Θ(g(n)) | c₂·g ≤ f ≤ c₁·g (tight) | = |

---

## 6. Quy tắc đơn giản hóa Asymptotic

1. **Bỏ hằng số nhân:** 3n² và 100n² đều là Θ(n²)
2. **Chỉ giữ số hạng bậc cao nhất:** 3n² − 100n + 6 → n² chiếm ưu thế
3. **n₀ loại bỏ small cases:** Không quan tâm behavior khi n nhỏ — chỉ cần đúng "eventually" (khi n > n₀)

### Thứ tự thống trị (Dominance Ordering)

$$1 \ll \log n \ll n \ll n \log n \ll n^2 \ll n^3 \ll 2^n \ll n!$$

- Exponential **luôn** thắng polynomial, bất kể hằng số
- Ví dụ: 0.01·n³ cuối cùng sẽ lớn hơn 1,000,000·n² khi n đủ lớn

---

## 7. Key Takeaways

- **Counter-examples là công cụ thiết kế:** Hầu hết heuristic đầu tiên đều sai. Chứng minh sai = bước tiến, không phải thất bại.
- **Đơn giản > phức tạp trong counter-example:** Ví dụ nhỏ nhất thuyết phục nhất.
- **Earliest Completion Date** là thuật toán greedy đúng cho Interval Scheduling — có thể chứng minh bằng proof by contradiction.
- **Knapsack** không có heuristic greedy nào đúng — đây là dấu hiệu của bài toán khó.
- **Big-Θ là mục tiêu:** Khi có cả upper và lower bound cùng loại → hiểu đầy đủ behavior.
- **Phân tích thuật toán ≠ benchmark thực tế** — RAM model bỏ qua hằng số để tập trung vào growth rate.

---

## Nguồn

- **CSE 373 Analysis of Algorithms, Lecture 2, Fall 2020** – Prof. Skiena
  NotebookLM: *Thuật toán và Độ phức tạp: Bài toán Xếp lịch và Knapsack*
