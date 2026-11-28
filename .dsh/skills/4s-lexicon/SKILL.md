---
name: 4s-lexicon
description: >
  Tạo và bảo trì LEXICON.md ở gốc repo: từ điển chung viết bằng lời đời thường để người không
  biết code (quản lý, khách hàng, người mới) và agent hiểu cùng một nghĩa trước khi viết code.
  Ghi từ vựng nghiệp vụ, tên tầng kiến trúc, việc nào nằm ở file nào, đơn vị đo. Skill 4s gọi
  skill này khi repo chưa có từ điển. Dùng khi một từ bị hiểu hai kiểu, khi đặt tên module hay
  feature, khi cần chốt nghĩa thuật ngữ. Triggers: "từ điển dự án", "thống nhất định nghĩa",
  "lexicon", "glossary", "thuật ngữ", "hiểu sai ý nhau", /4s-lexicon.
---

# Từ điển dự án (`LEX`)

Mục tiêu: **hai người (người và agent) không bao giờ phải đoán một từ nghĩa là gì.**

Một từ điển đạt chuẩn khi **người không biết code đọc cũng hiểu**: người quản lý dự án, khách
hàng, người mới vào team. Chỉ dev đọc được thì đó là tài liệu kỹ thuật, không phải từ điển —
và người không hiểu sẽ đoán, đoán sai thì yêu cầu sai, code viết đúng cũng thành sai.

File: `LEXICON.md` ở gốc repo. Một file, một chỗ (`S2`). Vượt 200 dòng thì tách theo miền
nghiệp vụ: `LEXICON.devices.md`, `LEXICON.billing.md`, giữ `LEXICON.md` làm mục lục.

## 1. Vòng đời

```
Repo mới / chưa có LEXICON.md
  └─ đọc code + README + schema → điền mục 2 (từ vựng), 3 (tầng), 4 (việc → nơi ở), 7 (đơn vị)
     └─ hỏi người dùng CHỈ những từ còn mâu thuẫn → ghi vào mục 6 (Quyết định)

Trong lúc code
  └─ gặp từ chưa có trong từ điển
     └─ tự định nghĩa được từ code/schema? → CÓ: ghi luôn, báo 1 dòng
                                            → KHÔNG: dừng, chạy `4s-interview`, rồi ghi

Sửa bug
  └─ bug này có phải vì hai người hiểu khác nhau một từ, hay một đơn vị?
     └─ KHÔNG (code sai): sửa code, KHÔNG đụng từ điển
     └─ CÓ: sửa mục 2 (làm rõ) hoặc mục 7 (đơn vị), rồi thêm dòng mục 6 ghi lý do là bug đó

Review / kiểm repo
  └─ từ mới trong code chưa có trong LEXICON.md → phát hiện `LEX` (nên sửa)
  └─ một từ mang hai nghĩa → phát hiện `LEX` (phải sửa)
```

**Luật cứng:** một từ chỉ được có **một** nghĩa. Hai nghĩa → phải tách thành hai từ khác nhau,
không được dùng chung một từ với hai nghĩa. Đây là `S1` áp cho ngôn ngữ.

**Chiều ngược lại cũng là luật cứng:** một khái niệm chỉ được có **một** tên. Cùng một thứ mà
lúc gọi `khách hàng`, lúc gọi `client`, lúc gọi `user`, lúc gọi `account` là bốn cái tên cho một
thứ — code sẽ có bốn chỗ xử lý cùng một việc. Chọn một tên, ghi các tên còn lại vào **Tránh
dùng**. Đây là `S2` áp cho ngôn ngữ: một khái niệm nằm ở một tên.

## 2. Mỗi mục từ có đúng 7 phần

Thiếu phần nào thì mục từ đó chưa dùng được:

| Phần | Trả lời câu hỏi | Viết thế nào |
|:---|:---|:---|
| **Từ** | gọi là gì | tên thật trong dự án, kèm tên trong code nếu khác |
| **Nghĩa** | là cái gì | **một câu** tiếng Việt đời thường, không viết tắt, không dùng từ kỹ thuật |
| **Ví dụ** | trông thế nào trong thực tế | một tình huống cụ thể, có con số hoặc tên thật |
| **KHÔNG phải** | không phải cái gì | phần quan trọng nhất — ranh giới với từ gần nghĩa |
| **Tránh dùng** | đừng gọi nó bằng từ nào khác | các từ đồng nghĩa bị loại, để một khái niệm chỉ có một tên |
| **Nơi ở** | tìm ở đâu | file / bảng / topic nào (nối `S2`) |
| **Chủ** | ai chốt nghĩa này | tên người + ngày, để biết hỏi ai khi nghi ngờ |

**Ví dụ** và **KHÔNG phải** là hai phần bị bỏ qua nhiều nhất và cũng cứu nhiều bug nhất:
`Device` nghĩa là "thiết bị vật lý đã đăng ký", **không phải** "bản ghi trong bảng devices" —
hai thứ đó khác nhau và code sẽ sai nếu lẫn. Người không biết code không đọc được schema,
nên với họ **Ví dụ** chính là phần giải thích.

## 3. Bài kiểm: mục từ này đã dùng được chưa

Hỏi ba câu. Trượt một câu là phải viết lại, không phải viết thêm:

1. **Người ngoài đọc hiểu không?** Đưa mục từ cho một người không biết code trong dự án. Họ
   phải nói lại được nghĩa bằng lời của họ, và chỉ đúng một thứ trong hai thứ dễ lẫn.
2. **Phân biệt được với từ gần nghĩa không?** Nếu bỏ phần **KHÔNG phải** đi mà người đọc vẫn
   không lẫn, thì phần đó chưa viết đúng chỗ đau.
3. **Tìm được không?** Đọc **Nơi ở** rồi mở đúng file/bảng đó trong một lần thử.

## 4. Quy tắc viết cho người không biết code

- Một câu một ý. Câu dài thứ hai thì xuống dòng, không nối bằng dấu phẩy.
- **Định nghĩa cái nó LÀ, không phải cái nó LÀM.** "Một lần khách đặt mua" là đúng; "dùng để
  lưu thông tin đơn" là sai — cái sau là mô tả chức năng, không phải định nghĩa.
- **Chỉ thuật ngữ riêng của dự án.** `timeout`, `error`, `cache`, `utils` là khái niệm lập trình
  chung, không vào từ điển dù dự án dùng nhiều. Trước khi thêm một từ, hỏi: đây là khái niệm riêng
  của dự án này, hay khái niệm chung ai cũng biết? Chỉ cái đầu mới vào.
- Không viết tắt trong lần xuất hiện đầu: `API`, `DB`, `MQTT`, `CRUD` → viết rõ rồi mới viết tắt.
- Không dùng "v.v.", "và tương tự", "như trên": người đọc không tự suy ra được, và agent sẽ tự
  suy ra theo kiểu khác.
- Mỗi mục từ **tự đứng một mình**: đọc riêng vẫn hiểu, không phải đọc mục khác trước.
- Nói cái **KHÔNG phải** bằng tên của thứ dễ lẫn, không nói "không phải mấy cái khác".
- Nghĩa lấy từ code mà người dùng chưa xác nhận thì ghi `(chưa xác nhận)` ngay sau nghĩa.

## 5. Mẫu `LEXICON.md`

Sao nguyên khối mẫu ở [LEXICON.template.md](./LEXICON.template.md) vào `LEXICON.md` ở gốc
repo, xoá dòng ví dụ khi đã điền thật. Mẫu để riêng file để luật ở đây không phình quá `L200`.

## 6. Cách điền lần đầu

1. **Đọc trước, hỏi sau.** Lấy nghĩa từ code đang chạy, schema, README, tên bảng, tên topic.
   Cách hỏi là việc của `4s-interview` (mục 3: không hỏi điều code đã trả lời được).
2. **Ưu tiên từ gây tốn kém nhất khi hiểu sai:** tên thực thể nghiệp vụ (đơn hàng, thiết bị,
   phiên, gói cước), đơn vị đo (giây/ms, VND/nghìn đồng, UTC/giờ địa phương), trạng thái.
3. **Cách hỏi là việc của skill `4s-interview`** (cổng `ASK`): hỏi theo vòng và theo cây quyết
   định. Skill này chỉ nhận câu trả lời đã chốt rồi ghi, không tự soạn phiếu hỏi.
4. **Ghi cả từ đã thống nhất mà code đang làm SAI.** Đó là bug thật, ghi vào mục 6 kèm mã `LEX`.
5. Dừng lại khi hết từ gây tranh cãi. Từ điển 15 mục từ đúng nghĩa tốt hơn 100 mục từ sao chép.

## 7. Đơn vị và định dạng — luôn ghi rõ (mục 7 của mẫu)

Ba nhóm gần như chắc chắn gây bug nếu không ghi:

| Nhóm | Ghi rõ trong từ điển |
|:---|:---|
| Thời gian | mốc lưu là UTC hay giờ địa phương, định dạng nào, ai đổi múi giờ |
| Tiền | đơn vị nhỏ nhất, có làm tròn không, làm tròn kiểu gì, ai chịu thuế/phí |
| Số lượng / nhiệt độ / công suất | đơn vị SI nào, dấu thập phân, độ chính xác |

`LedgerEntry.amount` không nói gì thì một người viết 1000 (=1.000đ), người kia hiểu 1000đ.
Chỉ một dòng trong từ điển chặn được bug đó.

## 8. Ranh giới

- Từ điển chỉ chứa **định nghĩa**, không chứa hướng dẫn code, không chứa TODO, không chứa lịch sử.
  Nó phình ra là vì bị dùng sai mục đích.
- Skill này **sửa được** `LEXICON.md` (đó là việc của nó). Không sửa file code khác.
- Từ điển lệch code (code làm khác định nghĩa) → **code sai**, hoặc từ điển sai. Không được để lệch.
  Sửa cái nào cũng phải nói rõ đã sửa cái nào.
- **Sau mỗi bug fix, một câu hỏi bắt buộc:** bug này có phải vì hai người hiểu khác nhau một từ hay
  một đơn vị không? Không thì sửa code, không đụng từ điển — đa số bug là loại này. Có thì sửa mục
  2 hoặc mục 7, rồi **thêm dòng mục 6 ghi lý do là chính bug đó**: đó là sợi dây chặn bug quay lại.
- **Ai được sửa gì:** agent tự thêm **Ví dụ**, **KHÔNG phải**, **Nơi ở** để làm rõ nghĩa đã có.
  Đổi **Nghĩa** hoặc đảo quyết định cũ thì phải hỏi người chốt (`ASK-1`): nghĩa nghiệp vụ là của
  người dùng. Đảo quyết định = thêm dòng mục 6 mới, không xoá dòng cũ. Người dùng **báo lỗi**
  thì chờ duyệt trước khi sửa từ điển (`FIX`, `4s` mục 0.3).
- Từ điển là của con người. Nghĩa lấy thẳng từ code hay schema thì agent ghi và báo một dòng.
  Nghĩa phải chọn giữa hai cách hiểu thì người chốt, ghi tên vào mục 6.
- Người đọc từ điển không bắt buộc biết code. Câu nào chỉ dev hiểu thì câu đó viết lại.
