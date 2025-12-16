### 1\. Lấy trong một khoảng (Từ commit cũ $\rightarrow$ commit mới)

**Cú pháp:**

```bash
git diff --name-only <hash_cũ> <hash_mới>
```

**Ví dụ thực tế:**

  * **Lấy danh sách file thay đổi giữa nhánh `main` và nhánh `dev`:**
    ```bash
    git diff --name-only main dev
    ```
  * **Lấy danh sách file trong 5 commit gần nhất:**
    ```bash
    git diff --name-only HEAD~5 HEAD
    ```

> **Lưu ý:** Lệnh này sẽ **gộp** các thay đổi lại. Nếu file A sửa ở commit 1, rồi lại sửa tiếp ở commit 2, nó chỉ hiện tên file A một lần (rất gọn).

-----

### 2\. Lấy từ các commit cụ thể (Rời rạc)

Nếu bạn muốn chọn lọc (ví dụ: chỉ lấy file của commit A và commit C, bỏ qua commit B ở giữa), bạn dùng `git show` kết hợp với lệnh lọc trùng lặp.

```bash
git show --name-only <hash_1> <hash_2> <hash_3> | sort | uniq
```

  * `git show --name-only ...`: Liệt kê file của từng commit (sẽ bị trùng tên file nếu file đó sửa ở cả 2 commit).
  * `sort | uniq`: Sắp xếp và **loại bỏ các file trùng nhau**.

-----

### 3\. Nâng cao: Chỉ lấy file "Tồn tại" (Bỏ file đã xóa)

Nếu mục đích của bạn là lấy danh sách để copy file đi deploy, bạn sẽ gặp lỗi nếu danh sách chứa các file **đã bị xóa (Deleted)**.

Để lọc bỏ file đã xóa, chỉ lấy file Thêm mới (Added), Sửa (Modified), hoặc Đổi tên (Renamed), hãy thêm tham số `--diff-filter`:

```bash
git diff --name-only --diff-filter=ACMR <hash_cũ> <hash_mới>
```

*(ACMR = Added, Copied, Modified, Renamed)*

-----

### Tổng kết và Xuất ra file

Dù bạn dùng cách nào ở trên, bạn chỉ cần thêm dấu `>` để xuất ra file `.txt`:

```bash
# Ví dụ: Xuất danh sách file thay đổi trong 10 commit gần nhất ra file text
git diff --name-only HEAD~10 HEAD > file_thay_doi.txt
```
