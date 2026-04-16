# Dynamic Array (Mảng Động / Vector)

## 1. Định nghĩa / Bản chất

Dynamic array là một mảng **tự động mở rộng kích thước** khi hết chỗ. Thay vì cố định số ô nhớ lúc khởi tạo, nó duy trì hai giá trị riêng biệt:

- **size** — số phần tử hiện đang chứa
- **capacity** — số ô nhớ đã cấp phát (luôn ≥ size)

Khi `size == capacity` và cần thêm phần tử, nó cấp phát mảng mới lớn hơn, copy toàn bộ dữ liệu sang, rồi giải phóng mảng cũ.

Trong các ngôn ngữ phổ biến: Python `list`, Java `ArrayList`, C++ `std::vector`, JavaScript `Array` đều là dynamic array.

---

## 2. Cơ chế resize (tăng gấp đôi)

```
capacity=4, size=4:   [A, B, C, D]          ← hết chỗ
                           ↓ resize × 2
capacity=8, size=4:   [A, B, C, D, _, _, _, _]
append(E):            [A, B, C, D, E, _, _, _]  size=5
append(F):            [A, B, C, D, E, F, _, _]  size=6
...
capacity=8, size=8:   [A, B, C, D, E, F, G, H]  ← hết chỗ lại
                           ↓ resize × 2
capacity=16, size=8:  [A, B, C, D, E, F, G, H, _, _, _, _, _, _, _, _]
```

**Tại sao nhân đôi mà không tăng thêm 1?**

Nếu tăng thêm 1 mỗi lần → resize xảy ra ở lần append 1, 2, 3, ..., n → tổng số lần copy = $1 + 2 + 3 + ... + n = O(n^2)$

Nếu nhân đôi mỗi lần → resize xảy ra ở lần append 1, 2, 4, 8, ..., n → tổng số lần copy = $1 + 2 + 4 + ... + n = 2n = O(n)$

→ Amortized $O(1)$ mỗi lần append.

---

## 3. Phân tích Amortized O(1) cho append

Xét n lần append vào dynamic array rỗng (capacity ban đầu = 1, nhân đôi mỗi khi đầy):

| Lần append | Resize? | Chi phí copy   |
| ----------- | ------- | --------------- |
| 1           | có     | 0 (mảng rỗng) |
| 2           | có     | 1               |
| 3           | có     | 2               |
| 4           | không  | 0               |
| 5           | có     | 4               |
| 6-8         | không  | 0               |
| 9           | có     | 8               |
| ...         | ...     | ...             |

Tổng chi phí copy sau n lần append: $1 + 2 + 4 + 8 + ... ≤ 2n$

→ Chi phí trung bình mỗi append = $\frac{2n}{n} = 2 = O(1)$ amortized.

Giải thích trực quan: Mỗi phần tử được "trả tiền" cho chính nó 1 lần, và "trả trước" 1 lần nữa để cover chi phí khi nó bị copy trong lần resize tiếp theo.

---

## 4. Độ phức tạp các thao tác

| Thao tác            | Time               | Space    | Ghi chú                                |
| -------------------- | ------------------ | -------- | --------------------------------------- |
| Access `arr[i]`    | $O(1)$           | $O(1)$ | Index trực tiếp vào bộ nhớ         |
| Append (cuối)       | $O(1)$ amortized | $O(1)$ | Thỉnh thoảng$O(n)$ khi resize       |
| Insert (đầu/giữa) | $O(n)$           | $O(1)$ | Phải dịch phần tử về phía sau     |
| Delete (cuối)       | $O(1)$           | $O(1)$ | Giảm size, không xóa bộ nhớ        |
| Delete (đầu/giữa) | $O(n)$           | $O(1)$ | Phải dịch phần tử về phía trước |
| Search (linear)      | $O(n)$           | $O(1)$ | Duyệt tuần tự                        |
| Search (binary)      | $O(\log n)$      | $O(1)$ | Yêu cầu mảng đã sắp xếp          |

---

## 5. Cài đặt thủ công bằng Python

```python
import ctypes

class DynamicArray:
    def __init__(self):
        self._size = 0
        self._capacity = 1
        self._data = self._make_array(self._capacity)

    def _make_array(self, cap):
        return (cap * ctypes.py_object)()

    def __len__(self):
        return self._size

    def __getitem__(self, i):
        if not (0 <= i < self._size):
            raise IndexError("Index out of range")
        return self._data[i]

    def append(self, item):
        if self._size == self._capacity:
            self._resize(2 * self._capacity)   # nhân đôi
        self._data[self._size] = item
        self._size += 1

    def _resize(self, new_cap):
        new_data = self._make_array(new_cap)
        for i in range(self._size):
            new_data[i] = self._data[i]        # copy sang mảng mới
        self._data = new_data
        self._capacity = new_cap

    def insert(self, index, item):
        if self._size == self._capacity:
            self._resize(2 * self._capacity)
        # dịch phần tử về phía sau
        for i in range(self._size, index, -1):
            self._data[i] = self._data[i - 1]
        self._data[index] = item
        self._size += 1

    def delete(self, index):
        if not (0 <= index < self._size):
            raise IndexError("Index out of range")
        # dịch phần tử về phía trước
        for i in range(index, self._size - 1):
            self._data[i] = self._data[i + 1]
        self._size -= 1

# Demo
arr = DynamicArray()
for i in range(10):
    arr.append(i * 10)
print(arr[5])       # → 50
arr.insert(2, 99)
arr.delete(0)
```

---

## 6. Quan sát capacity thực tế trong Python

```python
import sys

arr = []
prev_size = sys.getsizeof(arr)
for i in range(20):
    arr.append(i)
    curr_size = sys.getsizeof(arr)
    if curr_size != prev_size:
        print(f"Resize at len={len(arr)}: {prev_size} → {curr_size} bytes")
        prev_size = curr_size

# Output (Python 3.x, 64-bit):
# Resize at len=1:  56 → 88 bytes   (capacity: 0 → 4)
# Resize at len=5:  88 → 120 bytes  (capacity: 4 → 8)
# Resize at len=9:  120 → 184 bytes (capacity: 8 → 16)
# Resize at len=17: 184 → 248 bytes (capacity: 16 → 25? — CPython dùng hệ số 1.125 thay vì ×2)
```

> CPython (Python chuẩn) thực ra không nhân đúng 2 mà dùng công thức xấp xỉ $n + n/8 + 6$ — nhưng phân tích amortized O(1) vẫn đúng với bất kỳ hệ số nhân cố định nào > 1.

---

## 7. So sánh Static Array vs Dynamic Array

| Tiêu chí    | Static Array                | Dynamic Array                      |
| ------------- | --------------------------- | ---------------------------------- |
| Kích thước | Cố định                  | Tự mở rộng                      |
| Bộ nhớ      | Đúng bằng cần           | Có thể dư (overhead capacity)   |
| Append        | Không hỗ trợ             | $O(1)$ amortized                 |
| Access        | $O(1)$                    | $O(1)$                           |
| Cache         | Tốt                        | Tốt (bộ nhớ liên tục)         |
| Overhead      | Không có                  | `size`, `capacity`, con trỏ   |
| Dùng khi     | Kích thước biết trước | Kích thước không biết trước |

---

## Lưu ý quan trọng

- Không phải mọi thao tác append đều $O(1)$ — chỉ là **amortized** $O(1)$. Nếu cần **worst-case** $O(1)$ thì dynamic array không đáp ứng được.
- Resize sinh ra $O(n)$ spike — trong các hệ thống realtime/latency-sensitive, đây là vấn đề.
- Capacity chỉ **tăng**, không tự **giảm** — sau khi xóa nhiều phần tử, bộ nhớ không được giải phóng tự động (Python có `list.clear()`, C++ có `shrink_to_fit()`).
- Insert/Delete ở đầu/giữa vẫn $O(n)$ — nếu thao tác này chiếm ưu thế, xét dùng Linked List hoặc Deque.

---

## 8. Dùng con trỏ để duyệt mảng (thay vì chỉ mục)

Trong C, `arr[i]` chỉ là cú pháp viết tắt của `*(arr + i)` — cùng một lệnh máy. Python không có raw pointer, nhưng dùng `ctypes` để tạo mảng C thực sự và thao tác trực tiếp với địa chỉ bộ nhớ, qua đó minh họa đúng khái niệm này.

### 8.1 Ba cách viết tương đương

```python
import ctypes

# ===== TẠO MẢNG =====
arr = (ctypes.c_int * 5)(10, 20, 30, 40, 50)
# Mảng: [10, 20, 30, 40, 50]
#         ^
#       index 0

# ===== LẤY ĐỊA CHỈ CON TRỎ TỚI PHẦN TỬ ĐẦU =====
ptr = ctypes.cast(arr, ctypes.POINTER(ctypes.c_int))
print(f"Giá trị tại ptr (index 0): {ptr[0]}")  # 10

# ===== NHẢY CON TRỎ BẰNG PHÉP TOÁN SỐ HỌC =====
# Thay vì arr[2], ta dịch con trỏ đi 2 bước
ptr_jump = ctypes.cast(
    ctypes.addressof(arr) + 2 * ctypes.sizeof(ctypes.c_int),
    ctypes.POINTER(ctypes.c_int)
)
print(f"Nhảy tới index 2 bằng con trỏ: {ptr_jump[0]}")  # 30

# ===== DUYỆT MẢNG BẰNG CON TRỎ (không dùng index) =====
print("\nDuyệt mảng bằng con trỏ:")
base_addr = ctypes.addressof(arr)
size = ctypes.sizeof(ctypes.c_int)

for i in range(5):
    current_ptr = ctypes.cast(
        base_addr + i * size,
        ctypes.POINTER(ctypes.c_int)
    )
    print(f"  Địa chỉ: {base_addr + i * size} | Giá trị: {current_ptr[0]}")
```

### 8.2 Tại sao `base + i * step` nhảy đúng chỗ?

Khi tiến 1 ô không nhảy 1 byte mà nhảy `sizeof(int)` = 4 bytes (do một số `int` cần 4 bytes để lưu). Phải nhân thủ công kích thước kiểu dữ liệu.

```
Địa chỉ bộ nhớ (base = ctypes.addressof(c_arr), step = 4):

base + 0*4  →  c_arr[0] = 10
base + 1*4  →  c_arr[1] = 20
base + 2*4  →  c_arr[2] = 30
base + 3*4  →  c_arr[3] = 40
base + 4*4  →  c_arr[4] = 50
```

### 8.3 Ví dụ thực tế: tìm phần tử lớn nhất

```python
import ctypes

def max_element(c_arr, n):
    step     = ctypes.sizeof(ctypes.c_int)
    addr     = ctypes.addressof(c_arr)
    end_addr = addr + n * step            # "quá cuối" — không đọc tại đây
    max_val  = ctypes.cast(addr, ctypes.POINTER(ctypes.c_int))[0]

    while addr < end_addr:
        val = ctypes.cast(addr, ctypes.POINTER(ctypes.c_int))[0]
        if val > max_val:
            max_val = val
        addr += step
    return max_val

data = (ctypes.c_int * 5)(3, 17, 8, 42, 5)
print(f"Max = {max_element(data, 5)}")   # Max = 42
```

> **Lưu ý:** `c_arr[i]` và truy cập qua địa chỉ `base + i * step` cho kết quả giống nhau. Trong thực tế Python dùng `arr[i]` hoặc vòng `for item in arr` — kiểu duyệt địa chỉ thủ công chỉ dùng khi cần làm việc với buffer thô (image processing, binary protocol, v.v.).

---

## Nguồn

- [UC San Diego - Data Structures](https://www.coursera.org/learn/data-structures)
