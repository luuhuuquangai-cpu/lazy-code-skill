---
name: 4s-review
description: >
  Review một thay đổi (diff, commit, nhánh, PR) theo 4S: có nhét thêm việc vào file, có tạo bản
  sao thứ hai của một luật, có import ngược tầng, catch và log lỗi có tên module, file và hàm có
  vượt giới hạn, có dòng nào không cần tồn tại, tên mới có khớp LEXICON.md. Đọc cả code liên quan,
  không chỉ diff. Báo cáo đánh số, không sửa code. Dùng cho "review 4s", "duyệt PR theo 4s",
  "check diff này", /4s-review.
---

Review thay đổi như người sẽ bị gọi dậy khi nó hỏng. Thứ tự quan trọng: đúng, an toàn, `S4`,
`S3`, `S2`, `S1`, `LEX`, gọn. Đây là báo cáo người dùng yêu cầu, nên đưa đủ.
Muốn kiểm cả repo thì dùng `4s-check`.

## 1. Hiểu trước

- Review đúng thứ người dùng chỉ: thay đổi chưa commit, staged, một nhánh, một PR, hoặc file.
  Không chỉ gì: thay đổi chưa commit; không có thì commit cuối.
  `git diff`, `git diff --staged`, `git diff HEAD~1`, `git diff main...HEAD`.
- Đọc diff, rồi đọc cả file chứa nó: diff không cho biết file đang làm mấy việc. Đọc caller của
  mọi hàm bị đổi, hàm nó gọi, test, và `LEXICON.md`.
- Đổi chữ ký, giá trị trả về hay hành vi thì grep mọi caller. Thay đổi có thể làm hỏng code nó
  không chạm tới.
- Diff rỗng: nói rỗng, dừng.

## 2. Tìm

1. **Bug:** kết quả sai, crash, sót edge case (rỗng, 0, phần tử cuối, làm tròn, múi giờ), caller
   bị hỏng, sửa ở một caller trong khi hàm dùng chung vẫn sai.
2. **Rủi ro:** lỗ hổng bảo mật (injection, secret trong code, thiếu kiểm input người dùng), mất
   dữ liệu (ghi sai thứ tự, không transaction).
3. **`S4` Số Danh:** `catch`/`except` mới nuốt lỗi (`catch {}`, `return null`, `except: pass`),
   log error/warn mới không có `[module.ham]`, thông điệp không đủ để người trực biết chuyện gì.
4. **`S3` Suôn Dòng:** import ngược tầng (Entity hay Use Case import DB, API, UI), import vòng
   mới, fetch/ghi/emit lén giữa dòng xử lý. Tên tầng thật ở `LEXICON.md` mục 3.
5. **`S2` Sát Nơi:** logic đã có ở chỗ khác (grep trước khi kết luận), bản sao thứ hai của cùng
   một luật, một việc mới rải ra nhiều thư mục.
6. **`S1` Sạch Việc:** thêm việc thứ hai vào file vốn làm việc khác, hàm mới vào `utils`/`helpers`.
7. **Thiếu test:** logic rủi ro mới (rẽ nhánh, parser, tiền, bảo mật, ghi dữ liệu, sửa bug) không
   có test nào hỏng khi nó hỏng. Một test tốt, không cần coverage.
8. **`LEX`:** tên mới (biến, hàm, file, cột, topic) lệch nghĩa trong `LEXICON.md`, từ mới chưa có
   trong từ điển, từ bị cấm ở mục 5. Thay đổi chạm tiền, thời gian, đơn vị đo thì đối chiếu mục 7.
   Diff sửa bug: nếu nó đổi nghĩa hoặc đơn vị trong `LEXICON.md` thì phải có dòng mục 6 ghi lý do
   là bug đó; không có → phát hiện `LEX`. Nếu bug đáng lẽ phải sinh ra dòng đó mà diff không có →
   phát hiện `LEX` (bug sẽ quay lại).
9. **Gọn và giới hạn:** dòng không cần tồn tại (code chết, option không ai dùng, wrapper chỉ
   chuyển tiếp, interface một bản cài, dependency mới cho vài dòng); file sau thay đổi trên 200
   dòng hoặc hàm mới trên 30 dòng mà không khai `SC`; `shortcut:` mới không nêu khi nào nâng cấp.

## 3. Kiểm trước khi báo

- Mỗi phát hiện cần một trường hợp cụ thể: "input hoặc tình huống này dẫn tới kết quả sai này".
  Không có trường hợp cụ thể thì không phải phát hiện.
- Đọc lại dòng code và xác nhận: caller có thật, giá trị thật sự có thể rỗng, code thật sự
  không ai dùng.
- Vi phạm có `shortcut:` nêu rõ giới hạn là quyết định, không phải phát hiện, trừ khi tải thật
  đã vượt giới hạn đó.
- Đề xuất cách sửa nhỏ nhất. Ưu tiên cách sửa xoá code. Không thêm tầng hay interface.
- Không báo sở thích style, không "nên cân nhắc". Không kết luận bug UI từ diff: cần thì làm
  theo `4s-ui-debug`.

## 4. Đầu ra

Tiếng Việt đơn giản, câu ngắn. Giải thích thuật ngữ ở lần đầu dùng.

Mở đầu: `Thay đổi này làm gì:` hai ba câu, kèm đang review cái gì (vd `git diff main...HEAD`).

Rồi ba nhóm, bỏ nhóm rỗng:
- **Phải sửa:** bug, bảo mật, mất dữ liệu, `S4`, `S3`, `S2` bản sao luật, `S1`, `LEX` một từ hai
  nghĩa.
- **Nên sửa:** logic rủi ro không có test, `S2` một việc rải nhiều nơi, `LEX` từ chưa định nghĩa,
  vượt giới hạn không khai `SC`, `shortcut:` thiếu điều kiện nâng cấp, code thừa.
- **Nhỏ:** cách viết ngắn hơn.

Đánh số xuyên suốt. Mỗi phát hiện đủ bốn phần, mỗi phần một hai câu:

```
1. [S4] Gửi lệnh thất bại mà không ai biết (src/mqtt/gui_lenh.js:31-34)
   - Đây là gì: hàm gửi lệnh bật tắt tới thiết bị qua MQTT.
   - Vấn đề: catch nuốt lỗi và trả null. Mất kết nối broker thì nút bấm vẫn báo thành công.
   - Sửa: console.error('[mqtt.guiLenh] Gửi lệnh thất bại:', err.message); return { ok: false }.
   - Nếu bỏ qua: người dùng tưởng đã tắt thiết bị, và log không có dòng nào để lần ra.
```

Kết thúc bằng:
- `Kết luận: merge được.` hoặc `Kết luận: sửa 1 và 3 trước.`
- `Gọn: -<N> dòng được.` khi có phát hiện về code thừa.
- `Không kiểm:` một dòng, nếu có thứ quan trọng chưa kiểm được (test chưa chạy, UI chưa mở).

Không có phát hiện: `Thay đổi này làm gì:`, rồi `Đạt 4S. Merge được.` và một dòng về những gì
đã kiểm.

Báo cáo, không sửa code.
