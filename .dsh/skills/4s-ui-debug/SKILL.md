---
name: 4s-ui-debug
description: >
  Quy trình bắt buộc khi xử lý bug UI hoặc layout: mở trình duyệt thật, xem DOM đã render,
  console, style tính toán, ảnh chụp, so với template, rồi mới kết luận và sửa. Không kết luận
  từ code tĩnh. Dùng cho "lỗi giao diện", "vỡ layout", "không hiển thị", "bug UI", "nút không bấm
  được", "không reproduce được", /4s-ui-debug.
---

Bug UI không đọc code mà ra. Template khác HTML đã render, CSS viết khác style trình duyệt tính
ra, component có thể không mount. Mở trình duyệt thật trước, đọc code sau.

Dùng công cụ trình duyệt đang có trong môi trường (DevTools MCP, Playwright, hoặc trình duyệt
của người dùng). Không có công cụ nào: nói rõ, và nhờ người dùng gửi ảnh chụp và lỗi console.

## 1. Tái hiện

- Mở đúng URL, đúng kích thước màn hình, đúng trạng thái đăng nhập và dữ liệu người dùng báo.
- Làm lại đúng các bước dẫn tới lỗi. Chụp ảnh lúc lỗi hiện ra.
- Không tái hiện được: đổi kích thước màn hình (breakpoint), dữ liệu (rỗng, rất dài, có dấu),
  trình duyệt, chế độ tối. Chỉ được nói "không reproduce được" sau khi đã thử và liệt kê đã thử gì.

## 2. Thu bằng chứng

Theo thứ tự, dừng khi đã thấy nguyên nhân:

1. **Console:** error, warning, request lỗi (404, 500, CORS), asset không tải được. Một lỗi JS
   làm dừng cả trang thường là nguyên nhân thật của "nút không bấm được".
2. **DOM đã render:** snapshot cây phần tử. Phần tử có tồn tại không, đúng thứ tự không, đủ class
   và thuộc tính không. So với template nguồn.
3. **Style tính toán** của phần tử lỗi: `display: none`, `visibility`, `opacity: 0`,
   `overflow: hidden` cắt mất, `position: absolute` tràn ra ngoài, `z-index` bị đè, chiều rộng
   hoặc cao bằng 0. Xem rule nào đang thắng và đến từ file nào.
4. **Framework:** component đã mount chưa, state ban đầu có đúng không, hàm khởi tạo
   (`x-init`, `onMounted`, `useEffect`) có chạy và có ném lỗi không.
5. **Mạng:** dữ liệu API trả về có đúng hình dạng UI đang chờ không.

## 3. Sửa

- Nêu nguyên nhân bằng bằng chứng ở mục 2 (dòng console, rule CSS, phần tử thiếu), không đoán.
- Sửa tận gốc, một chỗ (`S2`). Không chồng thêm `!important` hay `z-index: 9999` để che lỗi.
- Log lỗi mới trong code UI kèm tên: `[pages.DevicesPage.taiDanhSach]` (`S4`).
- Sau khi sửa: mở lại trình duyệt, làm lại đúng các bước ở mục 1, chụp ảnh lại, kiểm console sạch.
  Chưa kiểm lại trên trình duyệt thì chưa xong.

## 4. Báo cáo

- **Nguyên nhân:** một hai câu, kèm bằng chứng (file:dòng, rule CSS, dòng console).
- **Đã sửa:** file:dòng.
- **Đã kiểm lại:** màn hình và trạng thái đã thử, ảnh trước và sau nếu có.
- **Chưa kiểm:** trình duyệt, breakpoint hay dữ liệu chưa thử.
