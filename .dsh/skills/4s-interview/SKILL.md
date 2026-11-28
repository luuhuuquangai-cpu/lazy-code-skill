---
name: 4s-interview
description: >
  Hỏi cho ra chuyện trước khi sửa code: dồn mọi câu hỏi vào một chỗ, hỏi theo vòng và theo cây
  quyết định, chốt nghĩa rồi ghi vào LEXICON.md để người dùng và agent nói cùng một ngôn ngữ. Dùng khi
  yêu cầu còn mơ hồ, khi một từ hiểu hai kiểu, khi không biết phải sửa chỗ nào. Triggers: "hỏi
  lại trước khi làm", "làm rõ yêu cầu", "yêu cầu mơ hồ", "không hiểu ý", /4s-interview.
argument-hint: "[yêu cầu hoặc từ cần chốt]"
---

# Hỏi cho ra chuyện (`ASK`)

Trước khi sửa một dòng code, người dùng và agent phải hiểu giống nhau. Chỗ nào agent không hiểu thì
thành **câu hỏi**, không được thành phỏng đoán. Trả lời xong thì đọng lại ở `LEXICON.md`, để lần
sau không ai phải hỏi lại đúng câu đó.

Skill này **không sửa code**. Việc của nó là hỏi, chốt, ghi.

## 1. Luật quan trọng nhất: hỏi dồn một chỗ

**Mọi câu hỏi dồn vào một giai đoạn, trước khi code.** Hỏi rải rác trong lúc làm là điều tệ nhất:
mỗi câu đến muộn là một lần sửa lại code vừa viết.

- Vào giai đoạn hỏi thì hỏi cho hết. Ra khỏi giai đoạn hỏi là **không hỏi nữa**.
- Đang code mà thấy cần hỏi → đó là dấu hiệu giai đoạn hỏi chưa xong. Dừng, quay lại hỏi cho
  hết, rồi mới code tiếp. Không vừa code vừa hỏi.
- Ngoại lệ duy nhất: phát hiện điều mới làm thay đổi hẳn yêu cầu. Khi đó nói rõ "cái này đổi yêu
  cầu, cần chốt lại" rồi mới hỏi.

## 2. Ba loại mơ hồ phải hỏi

| Mã | Loại | Dấu hiệu | Hỏi kiểu |
|:---|:---|:---|:---|
| `ASK-1` | Nghĩa từ | một danh từ có thể là hai thứ ("đơn hàng", "phiên", "đồng bộ") | "X nghĩa là A hay B?" |
| `ASK-2` | Ranh giới hành vi | không biết khi nào chạy, lỗi thì làm gì, ai được làm | "Gặp lỗi thì a) báo b) thử lại c) bỏ qua?" |
| `ASK-3` | Phạm vi, nơi sửa | không biết sửa file nào, có được đổi dữ liệu cũ không | "Sửa A hay B? Được đổi dữ liệu đã lưu không?" |

Một câu hỏi thuộc đúng một loại. Câu trả lời phải ghi được vào một ô của `LEXICON.md`.

## 3. Trước khi hỏi: câu trả lời đang nằm ở đâu?

Đừng hỏi người dùng điều agent tự tìm được. Phân loại từng chỗ mơ hồ trước:

| Câu trả lời nằm ở | Cách lấy | Ai làm |
|:---|:---|:---|
| Trong code, filesystem, docs, schema | grep, đọc file, đọc `LEXICON.md` | **Agent tự tìm** |
| Trong đầu người dùng | hỏi người dùng, theo mục 4 | Agent hỏi |
| Trong đầu người khác (sếp, khách, vendor) | đề nghị người dùng đi hỏi, kèm sẵn câu hỏi soạn sẵn | Người dùng hỏi |
| Chưa nói được bằng lời (cảm giác UI, mô hình dữ liệu) | làm một bản nháp vứt đi để có cái phản ứng | Agent làm, **ở nhánh hoặc thư mục riêng** |
| Chưa ai biết | ghi vào `KE_HOACH.md` mục "chưa rõ", làm phần khác trước | Để sau |

**Phân biệt hai trạng thái dễ lẫn:**

- **Chưa ai biết** — chưa hỏi được ai (chưa có người trả lời, phải chờ người khác). Ghi vào
  `KE_HOACH.md`, làm phần việc khác trước. Không chặn.
- **Đã hỏi, chưa có câu trả lời** — đã hỏi người dùng và đang chờ. **Dừng hẳn, không sửa gì**
  (mục 8).

**Fact là việc của agent, quyết định là việc của người dùng.** Hỏi người dùng một fact tự grep được là làm mất
thời gian của người dùng và làm loãng những câu hỏi thật.

## 4. Hỏi theo vòng, theo cây

Đừng hỏi một loạt câu phẳng. Các quyết định có **thứ tự phụ thuộc**: chốt A rồi mới biết phải
hỏi gì về B.

- **Cây quyết định:** mỗi quyết định đẻ ra những quyết định con treo bên dưới.
- **Biên hỏi được:** những câu mà mọi thứ chúng phụ thuộc đã chốt xong. Chỉ hỏi những câu này.
- **Câu phụ thuộc một câu còn mở trong vòng này → để vòng sau.** Hỏi sớm thì người dùng phải đoán, mà
  đoán thì phải hỏi lại.

Mỗi vòng: hỏi **cả biên một lượt**, mỗi câu đánh số và **kèm sẵn câu trả lời agent đề xuất**.
Rồi **dừng, chờ người dùng**. Trả lời xong mới tính vòng tiếp.

**Biên nhiều hơn 5 câu thì sao:** hỏi trước **5 câu mà nhiều câu khác đang chờ nhất** (chốt chúng
thì mở ra nhiều câu mới). Phần còn lại để vòng sau. Nói rõ một dòng: "còn N câu nữa, hỏi tiếp
sau khi bạn trả lời". Không nhồi 8 câu vào một lượt — người ta sẽ trả lời qua loa.

Mẫu một vòng:

```
❓ Câu 1 — "đồng bộ" ở đây là gì?
   a) Đẩy dữ liệu lên server mỗi 5 phút
   b) Hai chiều, server sửa được dữ liệu trên máy
   c) Chỉ khi người dùng bấm nút
   → Mình nghiêng (a): sync_loop.js đang làm đúng thế. Bạn gõ "a" là xong.

❓ Câu 2 — Sửa `devices.themThietBi` hay thêm hàm mới?
   a) Sửa hàm cũ, mọi caller dùng chung
   b) Thêm hàm mới song song
   → Mình nghiêng (a): chỉ có 2 caller.
```

**Cách trả lời, thoải mái:** "a, b, câu 2 thì a" là đủ. "Cứ làm theo đề xuất" cũng đủ. Không cần
đúng định dạng. Không hiểu câu nào thì nói "câu 2 nói lại", agent giải thích bằng lời thường.

## 5. Cách đặt câu cho dễ trả lời

- **Đặt câu sao cho gõ "ừ" là đồng ý đề xuất.** Đây là mẹo giảm mệt nhiều nhất: người dùng chỉ phải gật
  hoặc sửa một chữ, không phải viết cả đoạn.
- **Hai đến ba lựa chọn lấy từ chính code** (tên bảng, tên hàm, nhánh `if` đang có), không bịa ra
  lựa chọn trên trời.
- **Bịa tình huống cụ thể để lộ ranh giới**, thay vì hỏi trừu tượng:

  > "Khách huỷ 1 trong 3 món thì đơn còn 2 món, hay thành một đơn mới?"

  Câu này lộ ra ranh giới mà câu "huỷ từng phần là gì?" không bao giờ lộ.
- **Đối chiếu lời người dùng nói với code.** Thấy mâu thuẫn thì nêu ra, đừng im lặng chọn một bên:

  > "Code đang huỷ cả đơn, nhưng bạn vừa nói huỷ từng món được. Cái nào đúng?"
- **Tối đa 5 câu một vòng.** Biên dài hơn thì chọn 5 câu mở ra nhiều câu khác nhất (mục 4).
- **Không hỏi lại câu đã hỏi.** Câu người dùng bỏ qua thì để chờ đó (mục 8).

## 6. Chốt và ghi vào từ điển

Người dùng trả lời xong, làm đúng thứ tự này:

1. Ghi nghĩa vào `LEXICON.md` mục 2, đủ **7 phần** theo `4s-lexicon` mục 2.
2. Nghĩa viết bằng lời đời thường để **người không biết code cũng hiểu**. Câu trả lời của người dùng
   thường đã đúng giọng đó — chép lại, đừng "chuyên môn hoá" lại. **Ví dụ** giữ nguyên tình
   huống và con số người dùng vừa kể.
3. Ghi quyết định vào mục 6: ngày, từ, chốt nghĩa, chủ, lý do một dòng.
4. Ghi xong mới quay lại code. **Chưa ghi thì chưa code.**
5. Người dùng nói "tuỳ bạn": agent chọn, nhưng vẫn ghi mục 6 với **Chủ = agent** và lý do, để sau này
   biết chỗ nào agent tự quyết.

Cấu trúc file và chuẩn dễ hiểu là việc của `4s-lexicon`. Skill này quyết định **nội dung nào**
được vào.

## 7. Khi nào dừng hỏi

Dừng khi **biên rỗng**: mọi nhánh của cây đã đi qua, không còn gì đang được ngầm giả định. Rồi
nói rõ một câu: "Mình hiểu thế này: … đúng chưa?" và chờ người dùng gật.

Không được tự tuyên bố "không còn gì mơ hồ" để đi tiếp. Biên rỗng là kết luận của người dùng, không
phải của agent.

## 8. Chưa có câu trả lời thì dừng

Đã hỏi mà chưa ai trả lời:

- **Không sửa gì.** Không code, không sửa `LEXICON.md`, không nêu giả định rồi vẫn làm.
- Câu chưa trả lời để ở cuối câu trả lời dưới nhãn `ASK`, một dòng mỗi câu.
- Người dùng nói rõ "làm đi, đừng hỏi" thì đó **là duyệt** cho đúng task đó: được làm, nhưng nêu ngay
  một dòng đang hiểu theo nghĩa nào, để người dùng chặn kịp nếu sai.
- Không ghi giả định vào mục 6 — chưa ai chốt. Nếu giả định đó chạm luật 4S thì khai `shortcut:`.

## 9. Ranh giới

- Không sửa file code. Chỉ sửa `LEXICON.md`.
- Ngoại lệ duy nhất: **bản nháp** ở mục 3 để trả lời một câu hỏi. Bản nháp làm trên **nhánh riêng
  hoặc thư mục riêng**, không sửa code thật, không merge, và nói rõ nó là bản nháp. Câu hỏi được
  trả lời xong thì bản nháp hoặc bị bỏ, hoặc được làm lại tử tế ở bước LÀM.
- Không biến phỏng vấn thành biên bản dài: trả lời xong thì phiếu hỏi biến mất, chỉ còn dòng trong
  `LEXICON.md`.
- Hỏi người, không tự hỏi rồi tự trả lời: tự trả lời được thì đã không cần hỏi.
