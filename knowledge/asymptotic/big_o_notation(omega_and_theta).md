# Big-Omega (Ω) và Big-Theta (Θ) Notation

> Xem thêm: [Big-O Notation](big_o_notation.md)

---

## Nhắc Lại Nhanh

| Ký hiệu    | Ý nghĩa                     | Giới hạn           |
| ------------ | ----------------------------- | -------------------- |
| `O(g(n))`  | Không tệ hơn `g(n)`      | Trên (upper bound)  |
| `Ω(g(n))` | Không tốt hơn `g(n)`     | Dưới (lower bound) |
| `Θ(g(n))` | Xấp xỉ chính xác `g(n)` | Chặt (tight bound)  |

---

## Big-Omega Ω — Giới Hạn Dưới (Lower Bound)

### Định Nghĩa

`f(n) = Ω(g(n))` nghĩa là tồn tại hằng số `c > 0` và `n₀` sao cho:

```
f(n) ≥ c × g(n)   với mọi n ≥ n₀
```

Nói đơn giản: **g(n) là cận dưới của f(n)** — thuật toán luôn tốn ít nhất cỡ g(n) bước, không thể nhanh hơn được nữa.

> Big-Ω trả lời: *"Trong trường hợp tốt nhất, thuật toán tốn ít nhất bao nhiêu bước?"*

---

### Ví Dụ Minh Họa

```python
def sum_array(numbers):
    total = 0
    for i in range(len(numbers)):
        total = total + numbers[i]
    return total
```

Ta có `f(n) = 6n + 3`.

Thử `g(n) = n`, chọn `c = 6`:

```
n = 1  →  f(1) = 9    c·g(1) = 6    →  f(n) ≥ c·g(n) ✓
n = 5  →  f(5) = 33   c·g(5) = 30   →  f(n) ≥ c·g(n) ✓
n = 100 → f(100) = 603  c·g(100) = 600  →  f(n) ≥ c·g(n) ✓
```

Với mọi `n ≥ 1`, `6n + 3 ≥ 6n` luôn đúng.

→ **`f(n) = Ω(n)`**

---

### Ví Dụ Thực Tế

```python
def find_min(numbers):
    min_val = numbers[0]
    for item in numbers:       # Luôn phải duyệt hết mảng
        if item < min_val:
            min_val = item
    return min_val
```

Dù dữ liệu có được sắp xếp hay không, ta **bắt buộc phải duyệt qua mọi phần tử** để tìm giá trị nhỏ nhất.

→ `Ω(n)` — không thể làm nhanh hơn tuyến tính.

---

### Ví Dụ: Tìm Kiếm Trong Mảng Đã Sắp Xếp

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

- **Best case:** phần tử cần tìm nằm đúng ở giữa → 1 bước → `Ω(1)`
- **Worst case:** phải thu hẹp đến cùng → `O(log n)`

---

## Big-Theta Θ — Giới Hạn Chặt (Tight Bound)

### Định Nghĩa

`f(n) = Θ(g(n))` nghĩa là tồn tại hằng số `c₁, c₂ > 0` và `n₀` sao cho:

```
c₁ × g(n) ≤ f(n) ≤ c₂ × g(n)   với mọi n ≥ n₀
```

Tức là: **f(n) = O(g(n)) VÀ f(n) = Ω(g(n))** cùng lúc.

> Big-Θ trả lời: *"Thuật toán tăng trưởng chính xác theo tốc độ nào?"*

---

### Ví Dụ Minh Họa

Vẫn với `f(n) = 6n + 3`:

Chọn `g(n) = n`, `c₁ = 6`, `c₂ = 7`, `n₀ = 3`:

```
c₁ × g(n) ≤ f(n)  ≤  c₂ × g(n)
   6n      ≤ 6n+3 ≤     7n
```

Kiểm tra với `n = 3`:

```
18 ≤ 21 ≤ 21  ✓
```

Kiểm tra với `n = 10`:

```
60 ≤ 63 ≤ 70  ✓
```

→ **`f(n) = Θ(n)`** — thuật toán tăng trưởng chính xác tuyến tính.

---

### Ví Dụ Code

```python
def sum_array(numbers):
    total = 0
    for i in range(len(numbers)):   # Luôn duyệt đúng n lần
        total = total + numbers[i]
    return total
```

- Không có trường hợp nào duyệt ít hơn `n` lần → `Ω(n)`
- Không có trường hợp nào duyệt nhiều hơn `n` lần → `O(n)`
- Kết hợp lại → **`Θ(n)`**

---

## Khi Nào Dùng Cái Nào?

### Dùng Big-O khi:

Muốn đảm bảo thuật toán **không tệ hơn** một ngưỡng nhất định (worst case).

```python
# Quick Sort: O(n²) worst case, dù thường nhanh hơn
def quick_sort(arr):
    ...
```

### Dùng Big-Ω khi:

Muốn chứng minh thuật toán **không thể nhanh hơn** một giới hạn (best case / lower bound lý thuyết).

```python
# Bất kỳ thuật toán sắp xếp dựa trên so sánh nào cũng cần Ω(n log n)
# → không thể có thuật toán sort tổng quát nào nhanh hơn O(n log n)
```

### Dùng Big-Θ khi:

Biết chính xác thuật toán tăng trưởng thế nào ở **mọi trường hợp**.

```python
# Merge Sort: luôn là Θ(n log n) dù input thế nào
def merge_sort(arr):
    ...
```

---

## So Sánh Trực Quan

```
        f(n)
          │        ╔══════════ c₂·g(n)  ← Big-O (trần)
          │       ╔╝
          │      ╔╝
          │    ══╝  ←────────── f(n)    ← nằm trong khoảng Θ
          │   ╔╝
          │  ╔╝
          │ ╚══════════ c₁·g(n)         ← Big-Ω (sàn)
          └──────────────────────── n
                    n₀
```

Từ `n₀` trở đi, `f(n)` luôn nằm **kẹp giữa** `c₁·g(n)` và `c₂·g(n)` → đó là `Θ(g(n))`.

---

## Tóm Tắt

| Ký hiệu           | Điều kiện                       | Ý nghĩa thực tế                   |
| ------------------- | ---------------------------------- | ------------------------------------- |
| `f(n) = O(g(n))`  | `f(n) ≤ c·g(n)`                | g(n) là**trần** — worst case |
| `f(n) = Ω(g(n))` | `f(n) ≥ c·g(n)`                | g(n) là**sàn** — best case   |
| `f(n) = Θ(g(n))` | `c₁·g(n) ≤ f(n) ≤ c₂·g(n)` | g(n) là**khung chính xác**   |

Khi một thuật toán có `O` và `Ω` giống nhau → ta dùng `Θ` để diễn đạt gọn hơn.
