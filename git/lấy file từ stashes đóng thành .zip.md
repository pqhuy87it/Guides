Dưới đây là các bước để lấy cụ thể một file từ stash:

### Tìm ID của stash chứa file đó

Đầu tiên, bạn cần biết file đó đang nằm ở stash số mấy. Hãy chạy lệnh:

```bash
git stash list
```

Kết quả sẽ hiển thị danh sách các stash, ví dụ: `stash@{0}`, `stash@{1}`, v.v.

*(Mẹo: Nếu bạn không nhớ file nằm ở stash nào, bạn có thể xem danh sách file bên trong 1 stash cụ thể bằng lệnh: `git stash show stash@{0} --name-only`)*

### Lấy đúng 1 file ra ngoài

Có 2 cách để làm việc này, tùy thuộc vào phiên bản Git bạn đang dùng hoặc thói quen của bạn:

**Cách 1: Dùng `git checkout` (Cách truyền thống, máy nào cũng chạy được)**
Cú pháp này rất giống với việc bạn lấy 1 file từ nhánh khác về.

```bash
git checkout stash@{0} -- <đường_dẫn_tới_file>
```

  * **Ví dụ:** `git checkout stash@{0} -- src/main.js`
  * **Lưu ý:** Nhớ có dấu `--` và khoảng trắng trước tên file nhé. Lệnh này sẽ kéo nội dung file từ `stash@{0}` đè lên file hiện tại ở máy bạn và tự động đưa nó vào trạng thái Staging (đã `git add`).

**Cách 2: Dùng `git restore` (Khuyên dùng cho các bản Git mới từ 2.23 trở lên)**
Git mới ra mắt lệnh `restore` để thay thế cho `checkout` trong các thao tác phục hồi file, giúp tránh nhầm lẫn với việc chuyển nhánh.

```bash
git restore --source=stash@{0} -- <đường_dẫn_tới_file>
```

  * **Ví dụ:** `git restore --source=stash@{0} -- src/main.js`
  * **Đặc điểm:** Cách này sẽ lấy file về nhưng **để nó ở trạng thái Unstaged** (màu đỏ, chưa `git add`), cho phép bạn xem lại code cẩn thận trước khi tự tay add.

----

### Dùng `git archive`
Cách này sẽ chui thẳng vào trong `stash`, lấy đúng phiên bản code lúc bạn cất đi và nén lại. Bạn **không cần** phải chạy `git stash apply` ra ngoài màn hình.

```bash
git archive -o update.zip stash@{0} $(git stash show --name-only --diff-filter=ACMR stash@{0})
```

**Giải thích lệnh:**
* `git archive -o update.zip stash@{0}`: Tạo file `update.zip` từ nội dung của commit `stash@{0}`.
* `$()`: Chạy lệnh bên trong để lấy danh sách tên file truyền vào cho lệnh zip.
* `git stash show --name-only`: Chỉ in ra tên các file có trong stash.
* `--diff-filter=ACMR`: Chỉ lấy các file Thêm, Sửa, Copy, Đổi tên. Bỏ qua file Xóa (nếu file đã xóa mà đem đi zip sẽ bị báo lỗi).

*(Lưu ý: Nếu stash của bạn không phải là `stash@{0}`, hãy đổi số `0` thành số thứ tự tương ứng khi bạn kiểm tra bằng `git stash list`)*.

---

### Dùng lệnh `zip` thông thường (Thông qua Pipe)
Nếu bạn đang dùng Git Bash hoặc Linux/macOS có sẵn lệnh `zip`, bạn có thể xuất danh sách file từ stash và truyền thẳng vào lệnh zip.

**Cảnh báo:** Cách này sẽ nén các file **đang nằm trên ổ cứng hiện tại** của bạn. Do đó, bạn phải chắc chắn rằng mình đã chạy `git stash apply` để code bung ra ngoài rồi mới chạy lệnh này.

```bash
git stash show --name-only --diff-filter=ACMR stash@{0} | zip update.zip -@
```

**Giải thích lệnh:**
* `|`: Chuyển danh sách file tìm được sang cho lệnh `zip` xử lý.
* `-@`: Báo cho lệnh `zip` biết hãy đọc danh sách file từ kết quả của lệnh đứng trước nó (stdin).

> **Mẹo nhỏ:** Nếu bạn có chứa cả những file tạo mới hoàn toàn (untracked files) lúc `git stash -u`, lệnh `git stash show` mặc định có thể không liệt kê chúng. Trong hầu hết các trường hợp làm việc với code đã được track, Cách 1 luôn là lựa chọn hoàn hảo và không sợ lỗi vặt nhất.

----

### Dùng lệnh `git archive` 
```bash
git archive -o /Users/phamhuy/Downloads/update.zip stash@{0} $(git stash show --name-only --diff-filter=ACMR stash@{0})
```

### Dùng lệnh `zip` thông thường
```bash
git stash show --name-only --diff-filter=ACMR stash@{0} | zip /Users/phamhuy/Downloads/update.zip -@
```

---

> **Mẹo nhỏ (Rất hữu ích trên macOS/Linux):**
> Thư mục `/Users/phamhuy/` chính là thư mục người dùng (Home directory) của bạn. Thay vì gõ một đường dẫn dài, bạn có thể dùng ký tự ngã `~` để viết tắt cho thư mục Home. 
> 
> Câu lệnh của bạn sẽ ngắn gọn và trông "pro" hơn rất nhiều:
> ```bash
> git archive -o ~/Downloads/update.zip stash@{0} $(git stash show --name-only --diff-filter=ACMR stash@{0})
> ```
