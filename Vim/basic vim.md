Vim là một trình soạn thảo văn bản mạnh mẽ nhưng có "đường cong học tập" khá cao vì nó hoạt động dựa trên các **Chế độ (Modes)**.

Dưới đây là bảng tổng hợp các lệnh cơ bản nhất để bạn có thể bắt đầu làm việc ngay lập tức.



### 1. Hiểu về các Chế độ (Modes)
Đây là điều quan trọng nhất. Vim không giống Notepad; bạn không thể mở lên và gõ ngay.
* **Normal Mode (Mặc định):** Dùng để di chuyển và thực hiện lệnh. Khi vừa mở Vim, bạn ở chế độ này. Bấm `Esc` để luôn quay về đây.
* **Insert Mode:** Dùng để gõ văn bản.
* **Visual Mode:** Dùng để bôi đen (chọn) văn bản.
* **Command Mode:** Dùng để lưu, thoát, tìm kiếm (bắt đầu bằng dấu `:`).

---

### 2. Thoát và Lưu (Quan trọng nhất)
Nếu bạn bị kẹt, hãy bấm `Esc` vài lần để chắc chắn bạn đang ở **Normal Mode**, sau đó gõ:

| Lệnh | Tác dụng |
| :--- | :--- |
| `:q` | Thoát (nếu chưa sửa gì) |
| `:q!` | **Thoát không lưu** (Bắt buộc thoát) |
| `:w` | Lưu (Write) |
| `:wq` | Lưu và Thoát (Write & Quit) |
| `:x` | Giống `:wq` (ngắn gọn hơn) |

---

### 3. Di chuyển con trỏ (Normal Mode)
Bạn có thể dùng phím mũi tên, nhưng "chuẩn Vim" là dùng các phím sau để không phải nhấc tay khỏi bàn phím:

| Phím | Hướng di chuyển |
| :--- | :--- |
| **h** | Sang Trái |
| **j** | Xuống Dưới |
| **k** | Lên Trên |
| **l** | Sang Phải |

**Di chuyển nhanh:**
* `w`: Nhảy đến đầu từ tiếp theo.
* `b`: Nhảy về đầu từ trước đó.
* `0` (số 0): Về đầu dòng.
* `$`: Về cuối dòng.
* `gg`: Về đầu file.
* `G`: Về cuối file.
* `:dòng`: Nhảy đến số dòng cụ thể (ví dụ `:10` nhảy đến dòng 10).

---

### 4. Chỉnh sửa văn bản (Vào Insert Mode)
Từ **Normal Mode**, bấm các phím sau để bắt đầu gõ:

| Phím | Tác dụng |
| :--- | :--- |
| **i** | Chèn trước con trỏ (**Insert**) |
| **a** | Chèn sau con trỏ (**Append**) |
| **A** | Chèn vào cuối dòng hiện tại |
| **o** | Mở dòng mới bên **dưới** và gõ |
| **O** | Mở dòng mới bên **trên** và gõ |

> **Lưu ý:** Sau khi gõ xong, hãy bấm **`Esc`** để quay lại Normal Mode ngay.

---

### 5. Cắt, Copy và Paste (Normal Mode)
Trong Vim, "Cut" (Cắt) thực chất là Delete (Xóa).

| Lệnh | Tác dụng |
| :--- | :--- |
| **x** | Xóa 1 ký tự ngay tại con trỏ |
| **dd** | Cắt (Xóa) toàn bộ dòng hiện tại |
| **dw** | Cắt (Xóa) từ hiện tại |
| **yy** | Copy toàn bộ dòng (**Yank**) |
| **p** | Dán nội dung vừa Cắt/Copy xuống **dưới** con trỏ (**Paste**) |
| **P** | Dán nội dung lên **trên** con trỏ |

---

### 6. Hoàn tác (Undo/Redo)

| Phím | Tác dụng |
| :--- | :--- |
| **u** | Undo (Hoàn tác thao tác vừa làm) |
| **Ctrl + r** | Redo (Làm lại thao tác vừa Undo) |

---

### 7. Tìm kiếm và Thay thế

| Lệnh | Tác dụng |
| :--- | :--- |
| `/từ_khóa` | Tìm kiếm "từ_khóa" trong văn bản |
| **n** | Nhảy đến kết quả tìm kiếm tiếp theo (Next) |
| **N** | Nhảy về kết quả tìm kiếm trước đó |
| `:%s/cũ/mới/g` | Thay thế tất cả chữ "cũ" thành "mới" trong toàn bộ file |

---

### Mẹo tư duy Vim (Verb + Noun)
Sức mạnh của Vim nằm ở việc kết hợp lệnh. Công thức thường là: **Hành động (Verb) + Số lượng (Number) + Đối tượng (Noun)**.

Ví dụ với lệnh `d` (delete - xóa):
* `d` + `w` = `dw` (Delete Word - Xóa 1 từ).
* `d` + `2` + `w` = `d2w` (Delete 2 Words - Xóa 2 từ).
* `d` + `$` = `d$` (Delete to end - Xóa từ con trỏ đến cuối dòng).

Bạn có muốn tôi giải thích kỹ hơn về cách cấu hình Vim (`.vimrc`) để code tiện hơn không?
