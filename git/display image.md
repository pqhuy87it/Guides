Để hiển thị các ảnh này dưới dạng thumbnail nhỏ gọn trên GitHub (thay vì kích thước gốc quá lớn), bạn có 2 cách phổ biến nhất.

Do bạn có nhiều ảnh chụp màn hình điện thoại (dạng dọc), cách tốt nhất là **xếp chúng nằm ngang (side-by-side)** để tiết kiệm không gian.

### Cách 1: Sử dụng Bảng (Table) - Khuyên dùng

Cách này giúp các ảnh nằm hàng ngang, trông gọn gàng nhất. Bạn hãy copy đoạn code dưới đây vào file `.md` của bạn:

```html
<table>
  <tr>
    <td><img src="https://github.com/user-attachments/assets/12b1a80e-316d-4fc0-be15-31fcaa43d3d0" width="200" /></td>
    <td><img src="https://github.com/user-attachments/assets/8fda922c-c28e-4844-9ef1-c6eb0382d0a1" width="200" /></td>
    <td><img src="https://github.com/user-attachments/assets/bcfa4184-6990-4ef0-a25c-6929665121cc" width="200" /></td>
  </tr>
   <tr>
    <td><img src="https://github.com/user-attachments/assets/d2a273d0-2134-4483-afc6-c4ed3adf371d" width="200" /></td>
    <td><img src="https://github.com/user-attachments/assets/f31d3a8b-6c1c-4636-a478-5b59a9b4bbaf" width="200" /></td>
    <td></td>
  </tr>
</table>

```

---

### Cách 2: Chỉ chỉnh kích thước (Xếp dọc hoặc trôi dòng)

Nếu bạn không muốn dùng bảng, bạn chỉ cần giảm tham số `width` xuống (ví dụ `200px`) và **xóa** tham số `height` để ảnh tự động co giãn đúng tỉ lệ.

```html
<img src="https://github.com/user-attachments/assets/12b1a80e-316d-4fc0-be15-31fcaa43d3d0" width="200" />
<img src="https://github.com/user-attachments/assets/8fda922c-c28e-4844-9ef1-c6eb0382d0a1" width="200" />
<img src="https://github.com/user-attachments/assets/bcfa4184-6990-4ef0-a25c-6929665121cc" width="200" />
<img src="https://github.com/user-attachments/assets/d2a273d0-2134-4483-afc6-c4ed3adf371d" width="200" />
<img src="https://github.com/user-attachments/assets/f31d3a8b-6c1c-4636-a478-5b59a9b4bbaf" width="200" />

```

**Lưu ý:**

* Mình đã đặt `width="200"`. Bạn có thể thay đổi số này thành `150` hoặc `250` tùy theo ý muốn to hay nhỏ hơn.
* Mình đã bỏ thuộc tính `height` để ảnh không bị méo.

Bạn muốn mình gộp các ảnh này vào một thẻ `<details>` (dạng danh sách sổ xuống/collapsible) để cho gọn file README không?
