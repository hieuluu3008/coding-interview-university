# Phân tích Chi phí Khấu hao và Hàm Tiềm năng (Amortized Analysis & Potential Functions)

---

## 1. Định nghĩa và Bản chất

**Amortized Analysis (Phân tích khấu hao)** không phải là phương pháp làm thuật toán chạy nhanh hơn — đây là một **cách tư duy về chi phí**. Mục tiêu là chứng minh rằng một thuật toán thực tế có hiệu quả tốt hơn so với cái nhìn ban đầu, bằng cách phân bổ các chi phí lớn phát sinh một lần lên nhiều thao tác khác nhau.

**Bản chất:** Lấy một khoản chi phí lớn và *rải nó theo thời gian* — chia nhỏ vào các "giỏ" công việc để việc chi trả trở nên dễ quản lý.

> **Điều kiện quan trọng:** Phân tích chỉ chính xác nếu ta thực hiện đủ số lượng thao tác như đã giả định. Nếu không làm đủ số lượng, kết quả khấu hao sẽ không còn đúng.

---

## 2. Ví dụ minh họa: Cửa hàng Dunkin' Donuts

| Khoản mục                           | Chi phí      |
| ------------------------------------- | ------------- |
| Phí nhượng quyền (một lần)      | 1.000.000 USD |
| Chi phí sản xuất mỗi chiếc bánh | 1 USD         |

**Vấn đề với cách tính thông thường:**

- Chiếc bánh đầu tiên tốn **1.000.001 USD** → không thể định giá bán được.
- Các chiếc tiếp theo chỉ tốn 1 USD → bất nhất hoàn toàn.

**Giải pháp khấu hao:**
Giả sử cửa hàng sẽ làm **1 triệu chiếc bánh**:

$$
\text{Chi phí khấu hao mỗi bánh} = \frac{1.000.000 + 1.000.000}{1.000.000} = \$2
$$

→ Mỗi chiếc bánh đều mang chi phí **2 USD** — nhất quán, có thể định giá, dễ lập kế hoạch.

---

## 3. Ví dụ CS: Bộ đếm nhị phân (Binary Counter)

**Định nghĩa công việc:** Trong một bộ đếm nhị phân, mỗi lần một bit bị lật (từ 0 thành 1 hoặc ngược lại) được tính là một đơn vị công việc.

**Sự biến động chi phí:** Khi tăng (increment) bộ đếm, chi phí thực tế ($c_i$) không cố định. Ví dụ:

| Chuyển đổi | Mô tả      | Số bit lật | Chi phí |
| ------------- | ------------ | ------------ | -------- |
| 0000 → 0001  | Từ 0 lên 1 | 1            | 1        |
| 0001 → 0010  | Từ 1 lên 2 | 2            | 2        |
| 0011 → 0100  | Từ 3 lên 4 | 3            | 3        |
| 0111 → 1000  | Từ 7 lên 8 | 4            | 6        |

**Phân tích thông thường (naive):** Nếu có k bit và đếm đến n, mỗi lần tăng tệ nhất lật k bit → tổng **O(nk)**.

**Thuật toán increment hoạt động như thế nào?**

Quét từ phải sang trái (từ bit thấp đến bit cao):

1. Nếu gặp bit **1** → lật thành **0**, tiếp tục sang trái.
2. Nếu gặp bit **0** đầu tiên → lật thành **1**, **dừng lại**.

Ví dụ tăng từ 7 lên 8 (`0111 → 1000`):

- Gặp bit 1 → lật thành 0, tiếp tục
- Gặp bit 1 → lật thành 0, tiếp tục
- Gặp bit 1 → lật thành 0, tiếp tục
- Gặp bit 0 → lật thành 1, **dừng** → kết quả `1000`

**Tại sao 50% lần tăng chỉ lật 1 bit?**

Số chẵn luôn có bit cuối = 0. Khi tăng từ số chẵn lên số lẻ, thuật toán gặp ngay bit 0 đầu tiên → lật thành 1 rồi dừng → chỉ **1 bit lật**. Vì số chẵn và số lẻ xen kẽ nhau, đúng 50% lần tăng là từ số chẵn → chỉ tốn 1 đơn vị công việc.

**Tại sao tổng chi phí là O(n) chứ không phải O(nk)?**

Phân tích naive cho rằng mỗi lần tăng có thể lật tới k bit → tổng O(nk). Nhưng thực tế không phải lần tăng nào cũng lật nhiều bit — chi phí lớn chỉ xảy ra thưa thớt. Phân tích khấu hao (sẽ trình bày ở phần 4) chứng minh chi phí trung bình mỗi thao tác là hằng số **2** → tổng n lần tăng chỉ tốn **2n = O(n)**.

---

## 4. Phương pháp Kế toán (The Accounting Method)

Tương tự mua **vé tàu khứ hồi New Jersey Transit**:

| Cách                | Mô tả                                                   | Tổng chi phí |
| -------------------- | --------------------------------------------------------- | -------------- |
| Vé một chiều × 2 | 10 USD đi + 10 USD về                                   | 20 USD         |
| Vé khứ hồi        | Trả**20 USD ngay lúc đi**, lượt về miễn phí | 20 USD         |

**Áp dụng vào bộ đếm nhị phân:**

- Khi bit **0 → 1**: trả chi phí **2** (1 đơn vị để lật + 1 đơn vị "trả trước" cho lần lật ngược)
- Khi bit **1 → 0**: chi phí **miễn phí** (đã được trả trước)

**Kết luận:** Mỗi lần increment chỉ có *đúng một* bit 0 chuyển thành 1 → chi phí khấu hao mỗi thao tác = **2** = O(1).

---

## 5. Hàm Tiềm năng (Potential Functions)

Phương pháp toán học hóa (formalize) cách kế toán trên.

### 5.1 Định nghĩa

| Ký hiệu     | Ý nghĩa                                              |
| ------------- | ------------------------------------------------------ |
| $D_i$       | Trạng thái cấu trúc dữ liệu sau thao tác thứ i |
| $\Phi(D_i)$ | Hàm tiềm năng tại trạng thái$D_i$              |
| $c_i$       | Chi phí thực tế của thao tác thứ i               |
| $a_i$       | Chi phí khấu hao của thao tác thứ i               |

**Điều kiện bắt buộc:**

- $\Phi(D_0) = 0$ (trạng thái ban đầu)
- $\Phi(D_i) \geq 0$ với mọi $i$

**Ý nghĩa vật lý:** Hàm tiềm năng lưu trữ "năng lượng tích lũy" từ các thao tác rẻ, để bù đắp chi phí cho các thao tác tốn kém trong tương lai.

> **Với bộ đếm nhị phân:** $\Phi(D_i)$ = **số lượng bit 1** trong bộ đếm tại thời điểm $i$ (= số vé khứ hồi đã mua nhưng chưa dùng).

### 5.2 Công thức chi phí khấu hao

$$
a_i = c_i + \Phi(D_i) - \Phi(D_{i-1})
$$

| Thành phần                  | Ý nghĩa                                                               |
| ----------------------------- | ----------------------------------------------------------------------- |
| $c_i$                       | Chi phí thực tế thao tác i                                          |
| $\Phi(D_i) - \Phi(D_{i-1})$ | Thay đổi tiềm năng (positive = tích lũy, negative = giải phóng) |

### 5.3 Tổng chi phí — Telescoping Sum

Cộng dồn tất cả $a_i$, các số hạng trung gian triệt tiêu nhau:

$$
\sum_{i=1}^{n} a_i = \sum_{i=1}^{n} c_i + \Phi(D_n) - \Phi(D_0)
$$

Vì $\Phi(D_0) = 0$ và $\Phi(D_n) \geq 0$:

$$
\sum_{i=1}^{n} c_i \leq \sum_{i=1}^{n} a_i
$$

→ Tổng chi phí thực tế **bị chặn trên** bởi tổng chi phí khấu hao.

### 5.4 Ví dụ tính toán cụ thể

**Thao tác 1: 0000 → 0001**

$$
c_1 = 1 \quad (\text{lật 1 bit})
$$

$$
\Phi(D_1) = 1, \quad \Phi(D_0) = 0
$$

$$
a_1 = 1 + (1 - 0) = \mathbf{2}
$$

**Thao tác: 0011 → 0100**

$$
c_i = 3 \quad (\text{lật 3 bit: } 1 \to 0, 1 \to 0, 0 \to 1)
$$

$$
\Phi(D_i) = 1 \text{ (một bit 1)}, \quad \Phi(D_{i-1}) = 2 \text{ (hai bit 1)}
$$

$$
a_i = 3 + (1 - 2) = \mathbf{2}
$$

Dù chi phí thực tế dao động (1, 2, 3, 4...), chi phí khấu hao **luôn = 2**.

---

## 6. Key Takeaways

- **Amortized Analysis ≠ tối ưu hóa thuật toán** — đây là công cụ *phân tích* để thấy rõ hơn thực tế.
- **Phương pháp kế toán** là cách trực quan: trả trước chi phí tương lai ngay lúc thao tác rẻ.
- **Hàm tiềm năng** là cách hình thức hóa: chọn $\Phi$ phản ánh đúng "năng lượng tích lũy" của cấu trúc.
- **Telescoping sum** là kỹ thuật toán học then chốt: các số hạng trung gian triệt tiêu, chỉ còn trạng thái đầu và cuối.
- **Quy tắc chọn $\Phi$:** Chọn hàm mà khi thao tác rẻ thì $\Phi$ tăng, khi thao tác đắt thì $\Phi$ giảm — hai chiều bù nhau.

---

## Nguồn

- **"Amortized Analysis - Potential Functions"** – Prof. Bill Byrne
  [https://www.youtube.com/watch?v=B3SpQZaAZP4](https://www.youtube.com/watch?v=B3SpQZaAZP4)
