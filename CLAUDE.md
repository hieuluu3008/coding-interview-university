# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Mục đích repo

Đây là nhật ký học tập cá nhân để ôn luyện phỏng vấn kỹ thuật, fork từ [jwasham/coding-interview-university](https://github.com/jwasham/coding-interview-university). Hoạt động chính là viết các ghi chú kiến thức có cấu trúc vào thư mục `knowledge/` theo từng chủ đề.

## Workflow tạo knowledge note

Dùng skill `/knowledge-note` khi user muốn tạo hoặc cập nhật ghi chú. Skill này xử lý toàn bộ workflow kể cả query NotebookLM nếu cần.

### Quy tắc folder

| Nội dung | Folder |
|----------|--------|
| Big-O, phân tích độ phức tạp, tiệm cận | `knowledge/asymptotic/` |
| Mảng, linked list, stack, queue, tree... | `knowledge/data_structures/` |
| Thuật toán sắp xếp, tìm kiếm | `knowledge/algorithms/` |
| Đệ quy, dynamic programming | `knowledge/dp-recursion/` |
| Chủ đề mới chưa có folder | Tạo folder mới — hỏi user trước |

Tên file: `snake_case.md`.

### Template markdown

```markdown
# <Tên Chủ Đề>

## 1. Định nghĩa / Bản chất
<giải thích cốt lõi bằng tiếng Việt, thuật ngữ kỹ thuật giữ tiếng Anh>

---

## 2. Ví dụ minh họa
<code block hoặc ví dụ cụ thể>

---

## 3. <Các phần nội dung chính>
<dùng bảng khi so sánh, LaTeX $...$ cho công thức toán>

---

## Lưu ý quan trọng
- <điểm cần nhớ>

---

## Nguồn
- [<Tên nguồn>](<URL>)
```

Quy tắc: H1 = tên chủ đề, section cách nhau bằng `---`, tiếng Việt cho giải thích, tiếng Anh cho thuật ngữ, nguồn luôn ở cuối.

**Ngôn ngữ lập trình:** Chỉ dùng Python cho tất cả các ví dụ code.

### Query NotebookLM (công cụ `nlm`)

Khi user nhắc đến notebook NotebookLM:

```bash
# Lấy danh sách notebooks
PYTHONIOENCODING=utf-8 nlm list notebooks

# Query một notebook
PYTHONIOENCODING=utf-8 nlm query notebook <notebook-id> "<câu hỏi chi tiết>"
```

Nếu output được lưu ra file (quá lớn cho stdout), extract bằng:

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

Đọc `_raw.txt`, tổng hợp theo template, rồi xóa file tạm:

```bash
rm knowledge/<topic>/_raw.txt
```