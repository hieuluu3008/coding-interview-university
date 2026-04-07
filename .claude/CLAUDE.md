---
name: knowledge-note
description: Tạo hoặc cập nhật file markdown lưu kiến thức trong thư mục knowledge/. Dùng skill này khi user muốn: tổng hợp kiến thức từ NotebookLM, ghi lại ghi chú học tập, tạo file md cho một chủ đề mới, hoặc bất cứ khi nào nhắc đến "lưu lại", "tổng hợp kiến thức", "tạo note", "viết vào file knowledge". Nếu có nhắc đến notebook NotebookLM, query nó để lấy nội dung trước khi viết.
---

# Knowledge Note Skill

Skill này giúp tạo và ghi file markdown trong thư mục `knowledge/` của repo.

## Cấu trúc repo

```
knowledge/
├── asymptotic/          # Độ phức tạp, Big-O, phân tích tiệm cận
│   ├── big_o_notation.md
│   ├── amortized_analysis.md
│   └── computational_complexity.md
└── <topic>/             # Thêm topic mới nếu cần
```

**Quy tắc đặt tên file:** `snake_case.md`, mô tả ngắn gọn nội dung.

---

## Template chuẩn cho file knowledge

```markdown
# <Tên Chủ Đề>

## 1. Định nghĩa / Bản chất

<Giải thích cốt lõi. Tránh copy-paste — diễn đạt lại bằng ngôn ngữ tự nhiên.>

---

## 2. Ví dụ minh họa

<Code block hoặc ví dụ cụ thể. Ưu tiên ví dụ thực tế dễ hiểu.>

---

## 3. <Các phần nội dung chính>

<Dùng bảng khi so sánh nhiều thứ. Dùng LaTeX ($...$) cho công thức toán.>

---

## Lưu ý quan trọng

- <Điểm cần nhớ 1>
- <Điểm cần nhớ 2>

---

## Nguồn

- [<Tên nguồn>](<URL>)
```

**Quy tắc định dạng:**
- Tiêu đề H1 là tên chủ đề
- Các section cách nhau bằng `---`
- Code: dùng code block với ngôn ngữ cụ thể
- Toán học: LaTeX inline `$...$`, block `$$...$$`
- Nguồn luôn ở cuối file
- Ngôn ngữ: tiếng Việt cho giải thích, tiếng Anh cho thuật ngữ kỹ thuật

---

## Workflow: Lấy nội dung từ NotebookLM

Khi user nhắc đến notebook NotebookLM, dùng `nlm` CLI để query:

### 1. Lấy danh sách notebooks (nếu chưa biết ID)
```bash
PYTHONIOENCODING=utf-8 nlm list notebooks
```

### 2. Query notebook để lấy nội dung
```bash
PYTHONIOENCODING=utf-8 nlm query notebook <notebook-id> "<câu hỏi chi tiết>"
```

**Câu hỏi query nên bao gồm:** định nghĩa, các khái niệm chính, ví dụ cụ thể, quy tắc/công thức quan trọng, lưu ý thực tế.

### 3. Đọc kết quả (output có thể lớn)

Nếu output quá lớn, nó được lưu vào file. Dùng Python để extract:
```bash
PYTHONIOENCODING=utf-8 python -c "
import json
with open(r'<đường dẫn file output>', encoding='utf-8') as f:
    data = json.load(f)
answer = data['value']['answer']
with open(r'knowledge/<topic>/_raw.txt', 'w', encoding='utf-8') as out:
    out.write(answer)
print('Done, length:', len(answer))
"
```

Sau đó đọc `_raw.txt`, tổng hợp lại theo template, rồi xóa file tạm:
```bash
rm knowledge/<topic>/_raw.txt
```

---

## Quyết định topic/folder

| Nội dung | Folder |
|----------|--------|
| Big-O, phân tích độ phức tạp, tiệm cận | `asymptotic/` |
| Cấu trúc dữ liệu (stack, queue, tree...) | `data-structures/` |
| Thuật toán sắp xếp, tìm kiếm | `algorithms/` |
| Đệ quy, dynamic programming | `dp-recursion/` |
| Chủ đề mới chưa có folder | Tạo folder mới phù hợp |

Nếu chưa rõ topic, hỏi user trước khi tạo folder mới.
