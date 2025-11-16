Để loại bỏ hoàn toàn commit `de0a8a3` (Task B) khỏi lịch sử (như thể nó chưa từng tồn tại), cách tốt nhất là sử dụng **Interactive Rebase (`git rebase -i`)**.

Dưới đây là các bước chi tiết dựa trên danh sách commit bạn cung cấp:

### Bước 1: Khởi động chế độ Rebase

Bạn cần chọn commit "gốc" (commit cha) nằm ngay trước commit bạn muốn xóa. Trong trường hợp này, đó là `0fc14a0` (commit task A).

Hãy chạy lệnh sau:

```bash
git rebase -i 0fc14a0
```

*(Lệnh này nghĩa là: "Hãy cho tôi sửa lại lịch sử bắt đầu từ sau commit 0fc14a0")*

### Bước 2: Chỉnh sửa danh sách commit

Một trình soạn thảo văn bản (thường là Vim hoặc Nano) sẽ hiện ra danh sách các commit theo thứ tự từ cũ đến mới (ngược lại với `git log`). Bạn sẽ thấy nội dung tương tự thế này:

```text
pick de0a8a3 commit task B
pick 91d4d35 commit task C
pick 2cf66ec no message
pick 5e157c9 commit task E
```

**Hành động:**
Bạn hãy tìm dòng `pick de0a8a3 commit task B` và thực hiện 1 trong 2 cách:

1.  **Xóa hẳn dòng đó đi.**
2.  Hoặc đổi từ khóa `pick` thành `drop` (ví dụ: `drop de0a8a3...`).

Sau khi sửa xong, danh sách sẽ trông như thế này (đã mất dòng Task B):

```text
pick 91d4d35 commit task C
pick 2cf66ec no message
pick 5e157c9 commit task E
```

### Bước 3: Lưu và thoát

  * Nếu là **Vim**: Bấm `Esc`, gõ `:wq` rồi `Enter`.
  * Nếu là **Nano**: Bấm `Ctrl + O` (Enter để lưu), rồi `Ctrl + X` (để thoát).

### Bước 4: Xử lý xung đột (Nếu có)

Nếu code trong "Task C" hoặc "Task E" có phụ thuộc vào code của "Task B" (mà bạn vừa xóa), Git sẽ báo lỗi **Conflict**.

1.  Mở các file bị conflict, sửa lại code cho đúng.
2.  Gõ `git add <tên_file>`.
3.  Gõ `git rebase --continue`.

Nếu không có conflict, Git sẽ báo thành công ngay lập tức.

### Bước 5: Cập nhật lên Server (Quan trọng)

Vì bạn thấy dòng `origin/main` trong log, nghĩa là commit này đã được đẩy lên server rồi. Việc xóa commit sẽ làm thay đổi lịch sử, nên bạn không thể `git push` thường được. Bạn bắt buộc phải dùng **Force Push**:

```bash
git push origin main --force
```

-----

### ⚠️ Cảnh báo quan trọng

Vì bạn đang dùng `--force` trên nhánh `main`:

  * Nếu bạn làm việc **một mình**: Cách này hoàn toàn ổn.
  * Nếu bạn làm việc **theo team**: Hãy chắc chắn không ai khác đang pull code về làm việc trên nhánh này, vì lệnh này sẽ gây lỗi cho họ.

**Phương án an toàn hơn (nếu làm việc team):**
Thay vì xóa hẳn commit, bạn dùng lệnh `git revert de0a8a3`. Nó sẽ tạo một commit mới đảo ngược lại những gì Task B đã làm, nhưng vẫn giữ lại lịch sử là "đã từng có Task B".
