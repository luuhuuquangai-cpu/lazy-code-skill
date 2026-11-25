---
name: 4s-interview
description: >
  Phỏng vấn người đưa ra yêu cầu trước khi sửa code: tìm chỗ agent không hiểu, hỏi lại bằng lựa
  chọn lấy từ chính code, chốt nghĩa, rồi ghi vào LEXICON.md để người và agent nói cùng một ngôn
  ngữ. Skill 4s gọi skill này ở cổng hỏi lại. Dùng khi yêu cầu mơ hồ, khi một từ hiểu hai kiểu,
  khi không biết phải sửa chỗ nào. Triggers: "phỏng vấn yêu cầu", "hỏi lại trước khi làm", "làm
  rõ yêu cầu", "yêu cầu mơ hồ", "không hiểu ý", /4s-interview.
argument-hint: "[yêu cầu hoặc từ cần chốt]"
---

# Phỏng vấn yêu cầu (`ASK`)

Trước khi sửa một dòng code, người đưa yêu cầu và agent phải **nói cùng một ngôn ngữ**. Mọi chỗ
agent không hiểu phải thành câu hỏi, không được thành phỏng đoán. Trả lời xong thì đọng lại ở
`LEXICON.md`, để lần sau không ai phải hỏi lại đúng câu đó.

Skill này **không sửa code**. Việc của nó là hỏi, chốt, ghi.

## 1. Ba loại mơ hồ phải hỏi

| Mã | Loại | Dấu hiệu | Câu hỏi dạng |
|:---|:---|:---|:---|
| `ASK-1` | Nghĩa từ | một danh từ có thể là hai thứ ("đơn hàng", "phiên", "đồng bộ") | "X nghĩa là A hay B?" |
| `ASK-2` | Ranh giới hành vi | không biết khi nào chạy, lỗi thì làm gì, ai được làm | "Gặp lỗi thì a) báo b) thử lại c) bỏ qua?" |
| `ASK-3` | Phạm vi, nơi sửa | không biết sửa file nào, có được đổi dữ liệu cũ không | "Sửa A hay B? Được đổi dữ liệu đã lưu không?" |

Một câu hỏi thuộc đúng một loại. Câu trả lời phải ghi được vào một ô của `LEXICON.md`.

## 2. Việc phải làm trước khi hỏi

1. Grep và đọc code liên quan. **Không hỏi điều code đã trả lời được** — hỏi câu đó làm mất thời
   gian người trả lời và làm mất uy tín của những câu hỏi sau.
2. Đọc `LEXICON.md`. Từ đã có nghĩa thì không hỏi lại; nghĩa trong từ điển lệch code thì nêu
   thành một `ASK-1`.
3. Với mỗi chỗ mơ hồ, soạn sẵn 2–3 nghĩa ứng viên **lấy từ chính code** (tên bảng, tên hàm,
   nhánh `if` đang có), chọn sẵn một ứng viên đề xuất kèm lý do một dòng.

## 3. Cách hỏi

- **Theo lô, tối đa 5 câu một lần.** Hỏi 12 câu một lượt thì người ta trả lời qua loa.
- **Mỗi câu là một lựa chọn**, không hỏi mở "anh muốn gì". Có 2–3 phương án đánh số, ghi rõ
  phương án đề xuất và vì sao.
- **Không hỏi lại câu đã hỏi.** Câu người dùng bỏ qua thì thành giả định (mục 5), không hỏi lại.
- **Xếp theo giá trị:** câu nào sai phải viết lại cả feature thì hỏi trước; câu nào sai chỉ đổi
  một dòng thì để sau.
- Trả lời được bằng một chữ là câu hỏi tốt. "A hay B?" tốt hơn "anh thấy thế nào?".

Mẫu phiếu hỏi:

```
[ASK-1] "đồng bộ" trong yêu cầu này là gì?
  1. Đẩy dữ liệu lên server mỗi 5 phút  (đề xuất — sync_loop.js đang làm đúng thế)
  2. Hai chiều, server sửa được dữ liệu trên máy
  3. Chỉ khi người dùng bấm nút

[ASK-3] Sửa `devices.themThietBi` hay thêm hàm mới?
  1. Sửa hàm cũ, mọi caller dùng chung  (đề xuất — chỉ có 2 caller)
  2. Thêm hàm mới song song
```

## 4. Chốt và ghi vào từ điển

Người trả lời xong, làm đúng thứ tự này:

1. Ghi nghĩa vào `LEXICON.md` mục 2, đủ 6 phần: Từ, Nghĩa, **Ví dụ**, **KHÔNG phải**, Nơi ở, Chủ.
2. Nghĩa viết bằng lời đời thường để **người không biết code cũng hiểu** — người quản lý dự án,
   khách hàng, người mới. Câu trả lời của họ thường đã đúng giọng đó, chép lại đừng "chuyên môn
   hoá" lại. **Ví dụ** là phần họ vừa kể: giữ nguyên tình huống và con số thật.
3. Ghi quyết định vào `LEXICON.md` mục 6: ngày, từ, chốt nghĩa, chủ, lý do một dòng.
4. Ghi xong mới quay lại code. Chưa ghi thì chưa code.
5. Người trả lời nói "tuỳ anh": agent chọn, nhưng vẫn ghi vào mục 6 với **Chủ = agent** và lý do,
   để sau này biết chỗ nào agent tự quyết.

Cấu trúc file và chuẩn dễ hiểu là việc của skill `4s-lexicon`. Skill này quyết định **nội dung
nào** được vào.

## 5. Không trả lời được thì sao

Người dùng bỏ qua câu hỏi, hoặc nói "làm đi, đừng hỏi":

- Vẫn làm, nhưng mở đầu câu trả lời bằng khối **Giả định** liệt kê đúng những gì đã đoán.
- Không ghi giả định vào `LEXICON.md` mục 6 — chưa ai chốt. Nếu giả định đó chạm vào luật 4S thì
  khai bằng comment `shortcut:`.
- Câu chưa có trả lời thì để ở cuối câu trả lời dưới nhãn `ASK`, một dòng mỗi câu.

Im lặng đoán rồi làm là lỗi nặng nhất của skill này.

## 6. Ranh giới

- Không sửa file code. Chỉ sửa `LEXICON.md`.
- Không biến phỏng vấn thành biên bản dài: trả lời xong thì phiếu hỏi biến mất, chỉ còn dòng
  trong `LEXICON.md`.
- Hỏi người, không tự hỏi rồi tự trả lời: tự trả lời được thì đã không cần hỏi.
- Yêu cầu đã rõ, không có điểm mơ hồ: nói một dòng "không có điểm mơ hồ" rồi làm luôn. Cổng hỏi
  lại không được dùng để trì hoãn việc rõ ràng.
