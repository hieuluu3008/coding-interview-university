# Big-O Notation

## Định Nghĩa

**Big-O Notation** là một ký pháp toán học dùng để mô tả **hiệu suất của thuật toán** — cụ thể là cách thời gian thực thi hoặc bộ nhớ sử dụng tăng lên khi kích thước dữ liệu đầu vào (`n`) lớn hơn.

Big-O **không đo thời gian tuyệt đối** (giây, millisecond) mà đo **tốc độ tăng trưởng** của thuật toán.

> Câu hỏi Big-O trả lời: *"Nếu dữ liệu tăng gấp đôi, thuật toán chậm đi bao nhiêu lần?"*

---

## Ví Dụ Minh Họa Định Nghĩa

### Ví dụ 1 — Số bước không phụ thuộc vào dữ liệu: O(1)

```python
def get_last_element(numbers):
    return numbers[len(numbers) - 1]
```

Các bước thực hiện:

1. Lấy `len(numbers)` — độ dài mảng
2. Thực hiện phép tính `len(numbers) - 1`
3. Lấy giá trị tại index vừa tính được
4. Return kết quả

Dù mảng có 10 hay 10 triệu phần tử, thuật toán vẫn luôn thực hiện đúng **4 bước**. Biểu diễn dưới dạng hàm số:

```
f(n) = 4
```

→ Số bước **không phụ thuộc** vào `n`, đây là **O(1)**.

---

### Ví dụ 2 — Số bước tăng theo dữ liệu: O(n)

```python
def sum_array(numbers):
    total = 0                        # bước 1: khởi tạo biến total
    for i in range(len(numbers)):    # bước 2: khởi tạo i = 0
        total = total + numbers[i]   # 4 bước lặp lại mỗi vòng
    return total                     # bước cuối: return
```

Phân tích số bước:

- **Ngoài vòng lặp:** 3 bước (khởi tạo `total`, khởi tạo `i`, return)
- **Trong mỗi vòng lặp:** 6 bước
  1. Lấy `len(numbers)`
  2. So sánh `i` với `len(numbers)`
  3. Lấy `numbers[i]`
  4. Cộng `total + numbers[i]`
  5. Gán kết quả vào `total`
  6. Tăng `i` lên 1

Với `n` phần tử → có `6n` bước trong vòng lặp + 3 bước ngoài:

```
f(n) = 6n + 3
```

---

### Từ f(n) đến Big-O — Tại Sao Bỏ Hằng Số?

Ở ví dụ 2, ta có `f(n) = 6n + 3`. Bây giờ đặt `g(n) = n`.

Với `n₀ = 3` và `c₀ = 7`:

```
f(n) = 6(3) + 3 = 21
c₀ × g(n) = 7 × 3 = 21
```

Với mọi `n ≥ 3`, ta luôn có:

```
f(n) ≤ c₀ × g(n)
tức là: 6n + 3 ≤ 7n
```

→ `g(n) = n` là **cận trên** của `f(n)`, nên ta viết: **f(n) = O(g(n)) = O(n)**

> Khi n đủ lớn, giá trị của hàm **f(n)** luôn nhỏ hơn hoặc bằng giá trị của hàm **g(n)** nhân với một hằng số  **c0** .

**Kết luận:** Hệ số `6` và hằng số `3` không quan trọng khi `n` lớn — Big-O chỉ giữ lại **bậc tăng trưởng** chính là `n`.

---

## Tại Sao Cần Big-O?

- So sánh hiệu năng giữa các thuật toán một cách khách quan.
- Dự đoán thuật toán có hoạt động được với dữ liệu lớn không.
- Lựa chọn giải pháp tối ưu khi thiết kế hệ thống.

---

## Các Loại Độ Phức Tạp (Từ Tốt → Tệ)

### O(1) — Hằng số (Constant)

Thời gian thực thi **không đổi** bất kể kích thước đầu vào.

```python
def get_first(arr):
    return arr[0]  # Luôn chỉ 1 bước dù arr có 10 hay 10 triệu phần tử
```

**Ví dụ thực tế:** Truy cập phần tử mảng theo index, tra cứu hash map.

---

### O(log n) — Logarit (Logarithmic)

Mỗi bước **loại bỏ một nửa** số phần tử còn lại.

```python
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```

**Ví dụ thực tế:** Tìm kiếm nhị phân, cây nhị phân tìm kiếm (Binary Search Trees).

> 1 tỷ phần tử chỉ cần ~30 bước!

---

### O(n) — Tuyến Tính (Linear)

Thời gian tăng **tỉ lệ thuận** với kích thước đầu vào.

```python
def find(arr, target):
    for item in arr:      # Duyệt qua từng phần tử
        if item == target:
            return True
    return False
```

**Ví dụ thực tế:** Tìm kiếm tuần tự, tính tổng mảng, đọc file.

---

### O(n log n) — Tuyến Tính Logarit (Linearithmic)

Thường xuất hiện ở các **thuật toán sắp xếp hiệu quả**.

```python
# Merge Sort — O(n log n)
def merge_sort(arr):
    if len(arr) <= 1:
        return arr
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    return merge(left, right)
```

**Ví dụ thực tế:** Merge Sort, Heap Sort, Quick Sort (trung bình).

---

### O(n²) — Bậc Hai (Quadratic)

Thường xuất hiện khi có **vòng lặp lồng nhau**.

```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):          # Vòng ngoài
        for j in range(n - i - 1):  # Vòng trong
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
```

**Ví dụ thực tế:** Bubble Sort, Selection Sort, so sánh mọi cặp phần tử.

> Dữ liệu tăng gấp đôi → thời gian tăng **gấp 4 lần**.

---

### O(2ⁿ) — Hàm Mũ (Exponential)

Thời gian **nhân đôi** với mỗi phần tử thêm vào.

```python
def fibonacci(n):
    if n <= 1:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)  # Gọi đệ quy 2 nhánh
```

**Ví dụ thực tế:** Bài toán đệ quy không tối ưu, brute-force tổ hợp.

> Với n=50, số bước lên đến hàng nghìn tỷ — **không dùng được thực tế**.

---

### O(n!) — Giai Thừa (Factorial)

Độ phức tạp tệ nhất, tăng cực nhanh.

```python
from itertools import permutations

def all_permutations(arr):
    return list(permutations(arr))  # n! hoán vị
```

**Ví dụ thực tế:** Bài toán người bán hàng (TSP) brute-force, sinh toán hoán vị.

> n=20 → 2,4 tỷ tỷ bước. **Hoàn toàn không khả thi.**

---

## Bảng So Sánh

| Ký hiệu      | Tên             | n = 10    | n = 100 | n = 1000  |
| -------------- | ---------------- | --------- | ------- | --------- |
| `O(1)`       | Hằng số        | 1         | 1       | 1         |
| `O(log n)`   | Logarit          | 3         | 7       | 10        |
| `O(n)`       | Tuyến tính     | 10        | 100     | 1.000     |
| `O(n log n)` | Tuyến tính log | 33        | 664     | 9.966     |
| `O(n²)`     | Bậc hai         | 100       | 10.000  | 1.000.000 |
| `O(2ⁿ)`     | Hàm mũ         | 1.024     | ~10³⁰ | ∞        |
| `O(n!)`      | Giai thừa       | 3.628.800 | ∞      | ∞        |

---

## Quy Tắc Tính Big-O

### 1. Bỏ hằng số

```
O(3n)   → O(n)
O(500)  → O(1)
O(5n²)  → O(n²)
```

### 2. Bỏ số hạng bậc thấp

```
O(n² + n)     → O(n²)
O(n + log n)  → O(n)
```

### 3. Vòng lặp lồng nhau → nhân

```python
for i in range(n):       # O(n)
    for j in range(n):   # O(n)
        ...              # Tổng: O(n * n) = O(n²)
```

### 4. Vòng lặp tuần tự → cộng rồi lấy bậc cao nhất

```python
for i in range(n):   # O(n)
    ...

for j in range(n):   # O(n)
    ...

# Tổng: O(n + n) = O(2n) → O(n)
```

---

## Lưu Ý Quan Trọng

- Big-O mô tả **worst case** (trường hợp xấu nhất) theo mặc định.
- Hai thuật toán cùng Big-O không có nghĩa chạy nhanh như nhau — hằng số ẩn vẫn có ảnh hưởng với input nhỏ.
- Trong thực tế, `O(n log n)` thường là **ngưỡng tốt** mà ta hướng đến khi sắp xếp/tìm kiếm.
- Tránh `O(n²)` trở lên khi xử lý dữ liệu lớn (> 10.000 phần tử).

---

*Nguồn tham khảo: [topdev.vn](https://topdev.vn/blog/big-o-notation-la-gi/)*
