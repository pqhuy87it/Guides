## 1. Lệnh xuất ra file

### Xuất commit merge

```bash
git log --merges --grep="TTP_VN-" --author="tungnh-expand" --author="HisakoIsaka" --pretty=format:"%h | %an | %d %s" > task_commit_list.txt
```
### không có author

```bash
git log --merges --grep="TTP_VN-" --pretty=format:"%h | %an | %d %s" > task_commit_list.txt
```

### Giải thích:

1.  **`--pretty=format:"%h | %an | %d %s"`**: Mình đã xóa các đoạn `%C(...)` để file text chỉ chứa chữ thuần túy, dễ đọc.
      * `%h`: Hash
      * `%an`: Tên tác giả
      * `%d`: Tên nhánh (nếu có)
      * `%s`: Nội dung commit
2.  **`> danh_sach_task.txt`**: Ký tự `>` sẽ chuyển toàn bộ kết quả hiển thị trên màn hình vào file này.
      * File sẽ được tạo tại thư mục hiện tại bạn đang đứng.
      * Nếu file chưa có, nó tự tạo. Nếu có rồi, nó sẽ **ghi đè**.
  
## 2. Lấy thông tin branch có format
### Có author

```bash
git log --merges main --grep="TTP_VN-" --author="tungnh-expand" --author="HisakoIsaka" --pretty=format:"%C(yellow)%h%Creset | %C(cyan)%an%Creset | %C(auto)%d%Creset %s"
```

### Không có author

```bash
git log --merges main --grep="TTP_VN-" --pretty=format:"%C(yellow)%h%Creset | %C(cyan)%an%Creset | %C(auto)%d%Creset %s"
```

**Giải thích các ký hiệu:**

  * `%h`: Mã hash ngắn (màu vàng).
  * `%an`: Tên tác giả (Author Name - màu xanh cyan) -\> Giúp bạn biết ai (Tung hay Hisako) đã merge.
  * **`%d`: Ref names (Tên nhánh, Tag...)** -\> Đây là cái bạn cần (nó sẽ hiện kiểu `(HEAD -> main, origin/develop)`).
  * `%s`: Nội dung commit (Message).

-----

### ⚠️ Lưu ý quan trọng về "Tên Branch" trong Merge Commit

Bạn cần phân biệt 2 loại "Tên Branch":

1.  **Branch hiện tại đang trỏ vào commit đó:**

      * Tham số `%d` (hay `--decorate`) sẽ hiển thị cái này.
      * Ví dụ: `(HEAD -> main, tag: v1.0)`
      * *Hạn chế:* Nếu nhánh `feature/TTP_VN-123` đã bị xóa sau khi merge, thì `%d` sẽ **không** hiện tên nhánh đó nữa.

2.  **Branch nguồn đã được merge (Đã bị xóa):**

      * Git không lưu metadata "commit này đến từ nhánh nào" sau khi nhánh đó bị xóa.
      * **Tuy nhiên**, vì bạn đang lọc **Merge Commit**, nên tên nhánh nguồn thường nằm ngay trong **Message (`%s`)**.
      * Ví dụ message: `Merge branch 'feature/TTP_VN-99' into main`. -\> Bạn sẽ thấy tên branch ngay trong nội dung.

### Dùng biểu thức chính quy (Regex)

Bạn có thể viết gộp vào một tham số, sử dụng dấu gạch đứng `|` (được escape bằng `\`) để biểu thị chữ "hoặc":

```bash
git log --merges --oneline --author="tungnh-expand\|HisakoIsaka"
```

## 3. Dùng git log --oneline

### Xem commit dạng rút gọn (Dễ nhìn nhất)

```bash
git log --oneline <tên_nhánh>
```

### Xem commit kèm biểu đồ (Visual)

```bash
git log --graph --oneline <tên_nhánh>
```

### Xem commit trên Remote (Server)

Nếu bạn muốn xem commit trên server (ví dụ Github/Gitlab) mà chưa pull về máy:

```bash
git fetch origin  # Cập nhật thông tin mới nhất từ server trước
git log --oneline origin/<tên_nhánh>
```
## 4.Lấy tên các branch trên origin

### Lọc riêng các nhánh của `origin` và loại bỏ các khoảng trắng ở đầu dòng để danh sách thẳng hàng

```bash
git branch -r | grep "origin" | awk '{$1=$1;print}' > danh_sach_nhanh.txt
```

### Chỉ lấy tên nhánh, bỏ chữ 'origin/'


```bash
git branch -r | grep "origin" | sed 's/origin\///' > danh_sach_nhanh.txt
```

**Giải thích các lệnh:**

  * `git branch -r`: Liệt kê các remote branch.
  * `grep "origin"`: Chỉ chọn các dòng có chứa chữ "origin".
  * `sed 's/origin\///'`: Thay thế (xóa) chữ "origin/" đi.
  * `> filename.txt`: Ghi kết quả vào file (thay vì hiện lên màn hình).


