Đây là cách để bạn đóng gói các file thay đổi thành một file `update.zip`.

Điều quan trọng nhất khi zip file để deploy là **phải loại bỏ các file đã bị xóa (Deleted)**, vì nếu không lệnh zip sẽ báo lỗi "File not found".

Dưới đây là 3 cách thực hiện trên **Git Bash** (hoặc Terminal):

### 1\. Cách chuẩn nhất (Dùng `git archive`) - Khuyên dùng

Cách này sử dụng chính công cụ của Git. Ưu điểm là nó **lấy nội dung file đúng tại thời điểm commit đó** (tránh trường hợp bạn lỡ sửa file trên máy mà chưa commit, cách này sẽ không zip nhầm code chưa hoàn thiện).

Cú pháp:

```bash
git archive -o update.zip HEAD $(git diff --name-only --diff-filter=ACMR <hash_cũ> <hash_mới>)
```

  * **Giải thích:**
      * `git archive -o update.zip HEAD`: Tạo file zip từ commit hiện tại (HEAD).
      * `$()`: Chạy lệnh trong ngoặc trước để lấy danh sách file.
      * `--diff-filter=ACMR`: Chỉ lấy file Thêm (Added), Copy, Sửa (Modified), Đổi tên (Renamed). **Loại bỏ file xóa (Deleted).**

> **Ví dụ:** Zip các file thay đổi trong 5 commit gần nhất:
>
> ```bash
> git archive -o update.zip HEAD $(git diff --name-only --diff-filter=ACMR HEAD~5 HEAD)
> ```

-----

### 2\. Cách dùng lệnh `zip` (Thông dụng)

Nếu máy bạn có cài sẵn tiện ích `zip` (thường có sẵn trong Git Bash), cách này rất tiện vì nó hỗ trợ đọc danh sách từ đầu vào (stdin).

Cú pháp:

```bash
git diff --name-only --diff-filter=ACMR <hash_cũ> <hash_mới> | zip update.zip -@
```

  * **Giải thích:**
      * `|`: Chuyển danh sách file tìm được sang lệnh tiếp theo.
      * `zip update.zip -@`: Dấu `-@` nghĩa là "lấy danh sách tên file từ kết quả của lệnh đứng trước nó".

> **Lưu ý:** Cách này sẽ zip **file hiện có trên ổ cứng** của bạn. Nếu bạn đã sửa file nhưng chưa commit, nó sẽ zip luôn cả phần chưa commit đó.

-----

### 3\. Cách đóng gói từ file `.txt` có sẵn

Nếu ở bước trước bạn đã xuất danh sách da file `danh_sach_nhanh.txt`, bạn có thể chạy lệnh sau để zip các file nằm trong danh sách đó:

```bash
cat danh_sach_nhanh.txt | zip update.zip -@
```

-----

### Tóm tắt tình huống sử dụng

1.  **Muốn an toàn tuyệt đối (code đúng như trong commit):** Dùng **Cách 1**.
2.  **Muốn nhanh gọn (code lấy trên ổ cứng hiện tại):** Dùng **Cách 2**.
3.  **File zip bị lỗi (do danh sách quá dài):** Nếu danh sách file quá nhiều (hàng nghìn file), Cách 1 sẽ bị lỗi "Argument list too long". Lúc này bắt buộc phải dùng **Cách 2**.

Bạn có cần mình hỗ trợ thêm về cách tự động đẩy file zip này lên server không?
