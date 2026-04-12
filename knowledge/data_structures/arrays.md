# Arrays (Mảng)

## 1. Định nghĩa / Bản chất

Array là một tập hợp các phần tử được lưu trữ trong các ô nhớ **liền kề nhau** trên bộ nhớ. Hình dung như một hộp đĩa DVD — mỗi đĩa là một phần tử, các đĩa xếp sát nhau theo thứ tự.

Đặc điểm cốt lõi:
- **Kích thước cố định** — phải khai báo dung lượng tại thời điểm tạo
- **Lưu trữ liên tục** — các phần tử nằm trong các ô nhớ kề nhau
- **Kiểu dữ liệu đồng nhất** — thường chỉ chứa một loại dữ liệu
- **Tốn kém khi mở rộng** — không thể tăng kích thước động

---

## 2. Ví dụ minh họa

```python
import ctypes

# Mảng cố định kích thước với ctypes (true static array)
DVDBox = (ctypes.py_object * 100)()

# Truy cập phần tử tại index 50 — O(1)
finding_nemo = DVDBox[50]

# Thêm phần tử vào vị trí bất kỳ — O(1) nếu cuối, O(n) nếu giữa/đầu
DVDBox[7] = "The Matrix"

# Xóa phần tử (gán None) — O(1) nếu cuối, O(n) nếu giữa/đầu
DVDBox[7] = None

# Linear search — O(n)
target = "The Matrix"
for i in range(100):
    if DVDBox[i] == target:
        print(f"Found at index {i}")
        break

# Binary search (yêu cầu mảng đã sắp xếp) — O(log n)
import bisect
sorted_arr = [1, 3, 5, 7, 9]
idx = bisect.bisect_left(sorted_arr, 5)  # trả về index 2
```

---

## 3. Các thao tác và độ phức tạp

| Thao tác | Vị trí | Time Complexity | Ghi chú |
|----------|--------|-----------------|---------|
| Access   | Bất kỳ | $O(1)$          | Truy cập trực tiếp qua index |
| Insert   | Cuối   | $O(1)$          | Gán thẳng vào ô trống |
| Insert   | Đầu/Giữa | $O(n)$       | Phải dịch chuyển các phần tử sau |
| Delete   | Cuối   | $O(1)$          | Gán null hoặc giảm size |
| Delete   | Đầu/Giữa | $O(n)$       | Phải dịch chuyển để lấp chỗ trống |
| Search (Linear) | — | $O(n)$   | Duyệt tuần tự từ đầu đến cuối |
| Search (Binary) | — | $O(\log n)$ | Yêu cầu mảng đã được sắp xếp |

---

## 4. Cơ chế truy cập O(1)

Vì các phần tử nằm liên tiếp trong bộ nhớ, địa chỉ của phần tử thứ `i` tính được trực tiếp:

$$\text{address}[i] = \text{base\_address} + i \times \text{element\_size}$$

Đây là lý do access luôn là $O(1)$ bất kể kích thước mảng.

---

## 5. Ưu và nhược điểm

**Ưu điểm:**
- Truy cập ngẫu nhiên cực nhanh: $O(1)$
- Cache-friendly — dữ liệu liên tục trong bộ nhớ, ít cache miss
- Đơn giản, ít overhead về bộ nhớ

**Nhược điểm:**
- Kích thước cố định, không linh hoạt
- Insert/Delete ở đầu hoặc giữa tốn $O(n)$
- Lãng phí bộ nhớ nếu khai báo quá lớn

---

## 6. Các loại Array

### 6.1 Linear Array (Mảng một chiều)

Dạng cơ bản nhất — các phần tử xếp thành **một hàng**, truy cập bằng 1 index.

```python
arr = [10, 20, 30, 40, 50]
arr[2]  # → 30
```

Trong bộ nhớ:
```
index:  0    1    2    3    4
      [10] [20] [30] [40] [50]
addr:  100  101  102  103  104
```

---

### 6.2 Multi-dimensional Array (Mảng nhiều chiều)

Là mảng của các mảng — thường gặp nhất là **2D array** (ma trận hàng × cột), truy cập bằng 2 index `[row][col]`.

```python
matrix = [
    [1, 2, 3],   # hàng 0
    [4, 5, 6],   # hàng 1
    [7, 8, 9],   # hàng 2
]
matrix[1][2]  # → 6  (hàng 1, cột 2)
```

Bộ nhớ vật lý vẫn **1 chiều** — 2D array được trải phẳng theo **row-major order**:

```
[1, 2, 3, 4, 5, 6, 7, 8, 9]
 ←hàng 0→ ←hàng 1→ ←hàng 2→
```

Địa chỉ phần tử `[i][j]` trong ma trận `m×n`:

$$\text{address}[i][j] = \text{base} + (i \times n + j) \times \text{size}$$

---

### 6.3 Dynamic Array (Mảng động)

Array **tự động tăng kích thước** khi đầy — giải quyết nhược điểm kích thước cố định. Python `list`, Java `ArrayList`, C++ `vector` đều là dynamic array.

**Cơ chế resize:**
1. Khởi tạo với capacity nhỏ (vd: 4)
2. Khi đầy → cấp phát mảng mới **gấp đôi** capacity
3. Copy toàn bộ phần tử sang mảng mới
4. Giải phóng mảng cũ

```
capacity=4:  [A, B, C, D]
resize:      [A, B, C, D, _, _, _, _]  capacity=8
append E:    [A, B, C, D, E, _, _, _]
```

**Tại sao gấp đôi?** Nếu tăng thêm 1 mỗi lần → tổng chi phí copy $O(n^2)$. Gấp đôi → tổng chi phí $O(n)$ cho $n$ lần append → **amortized $O(1)$** mỗi lần.

```python
import sys

arr = []
for i in range(10):
    print(f"len={len(arr)}, size={sys.getsizeof(arr)} bytes")
    arr.append(i)
# getsizeof tăng đột ngột theo bước — đó là lúc resize xảy ra
```

| Thao tác | Complexity | Ghi chú |
|----------|------------|---------|
| Access   | $O(1)$     | Như static array |
| Append (cuối) | $O(1)$ amortized | Thỉnh thoảng $O(n)$ khi resize |
| Insert (đầu/giữa) | $O(n)$ | Phải dịch phần tử |
| Delete (cuối) | $O(1)$ | |
| Delete (đầu/giữa) | $O(n)$ | |

---

### 6.4 Jagged Array (Mảng răng cưa)

Mảng nhiều chiều mà **các hàng có độ dài khác nhau** — trái với 2D array thông thường.

```
2D thường:       Jagged Array:
[1, 2, 3]        [1, 2, 3]
[4, 5, 6]        [4, 5]
[7, 8, 9]        [7, 8, 9, 10, 11]
```

```python
jagged = [
    [1, 2, 3],
    [4, 5],
    [7, 8, 9, 10, 11],
]
jagged[2][4]  # → 11

for row in jagged:
    for val in row:
        print(val, end=" ")
    print()
```

Trong bộ nhớ, jagged array là **mảng các con trỏ** — mỗi hàng nằm ở vùng nhớ riêng biệt:

```
jagged →  [ptr0] → [1, 2, 3]
          [ptr1] → [4, 5]
          [ptr2] → [7, 8, 9, 10, 11]
```

→ Các hàng **không liền kề** nhau → kém cache-friendly hơn 2D array thông thường.

Dùng khi: Tam giác Pascal, adjacency list trong đồ thị, dữ liệu dạng bảng không đều.

---

### Tổng hợp các loại Array

| | Linear | Multi-dim | Dynamic | Jagged |
|---|---|---|---|---|
| Số chiều | 1 | 2+ | 1 | 2+ |
| Kích thước | Cố định | Cố định | Tự mở rộng | Cố định, hàng lệch nhau |
| Bộ nhớ | Liên tục | Liên tục (trải phẳng) | Liên tục (có dư) | Phân tán |
| Cache | Tốt | Tốt | Tốt | Kém hơn |
| Ví dụ Python | `ctypes` array | `list` of `list` | `list` | `list` of `list` (khác độ dài) |

---

## Lưu ý quan trọng

- Chỉ dùng mảng khi kích thước dữ liệu **biết trước và cố định**
- Nếu cần thêm/xóa nhiều hoặc kích thước thay đổi động, dùng `ArrayList`, `LinkedList`, hoặc các cấu trúc phù hợp hơn
- Binary Search chỉ áp dụng được khi mảng **đã sắp xếp**

---

## Nguồn

- [Array 101 - Mảng và những điều có thể bạn chưa biết (Phần 1) — Viblo](https://viblo.asia/p/array-101-mang-va-nhung-dieu-co-the-ban-chua-biet-phan-1-0gdJzQrv4z5)
