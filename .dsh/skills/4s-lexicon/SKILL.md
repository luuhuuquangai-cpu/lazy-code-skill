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

Review / kiểm repo
  └─ từ mới trong code chưa có trong LEXICON.md → phát hiện `LEX` (nên sửa)
  └─ một từ mang hai nghĩa → phát hiện `LEX` (phải sửa)
```

**Luật cứng:** một từ chỉ được có **một** nghĩa. Hai nghĩa → phải tách thành hai từ khác nhau,
không được dùng chung một từ với hai nghĩa. Đây là `S1` áp cho ngôn ngữ.

## 2. Mỗi mục từ có đúng 6 phần

Thiếu phần nào thì mục từ đó chưa dùng được:

| Phần | Trả lời câu hỏi | Viết thế nào |
|:---|:---|:---|
| **Từ** | gọi là gì | tên thật trong dự án, kèm tên trong code nếu khác |
| **Nghĩa** | là cái gì | **một câu** tiếng Việt đời thường, không viết tắt, không dùng từ kỹ thuật |
| **Ví dụ** | trông thế nào trong thực tế | một tình huống cụ thể, có con số hoặc tên thật |
| **KHÔNG phải** | không phải cái gì | phần quan trọng nhất — ranh giới với từ gần nghĩa |
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
- Không viết tắt trong lần xuất hiện đầu: `API`, `DB`, `MQTT`, `CRUD` → viết rõ rồi mới viết tắt.
- Không dùng "v.v.", "và tương tự", "như trên": người đọc không tự suy ra được, và agent sẽ tự
  suy ra theo kiểu khác.
- Mỗi mục từ **tự đứng một mình**: đọc riêng vẫn hiểu, không phải đọc mục khác trước.
- Nói cái **KHÔNG phải** bằng tên của thứ dễ lẫn, không nói "không phải mấy cái khác".
- Nghĩa lấy từ code mà người dùng chưa xác nhận thì ghi `(chưa xác nhận)` ngay sau nghĩa.

## 5. Mẫu `LEXICON.md`

Sao nguyên khối dưới đây vào `LEXICON.md`, xoá dòng ví dụ khi đã điền thật.

```markdown
# LEXICON — Từ điển dự án <tên>

> Đọc file này TRƯỚC khi viết code. Gặp từ chưa có → thêm vào đây trước, code sau.
> Một từ = một nghĩa. Không cần biết code vẫn đọc hiểu file này.
> Cập nhật lần cuối: <YYYY-MM-DD>, bởi <tên>.

## 1. Cách đọc file này (60 giây)

| Bạn cần | Đọc phần |
|:---|:---|
| hiểu một từ | **Nghĩa** và **Ví dụ** |
| biết một thứ có tính là từ đó không | **KHÔNG phải** — đây là phần trả lời |
| tìm code liên quan | **Nơi ở** |
| không đồng ý với nghĩa | **Chủ** — hỏi đúng người đó |
| biết nghĩa này do ai chốt, khi nào | mục 6 |

Mục 6 là lịch sử. Không sửa dòng cũ, chỉ thêm dòng mới.

## 2. Từ vựng lõi (dùng ở mọi nơi)

### Đơn hàng (`order`)
- **Nghĩa:** một lần khách đặt mua, gồm nhiều món, có một mã để tra cứu và một trạng thái.
- **Ví dụ:** khách mua 2 bao gạo và 1 chai nước trong cùng một lần bấm "Đặt hàng" — đó là
  **một** đơn hàng, không phải ba.
- **KHÔNG phải:** không phải một món bên trong đơn (đó là `order_item`); không phải giỏ hàng
  khách đang chọn dở rồi bỏ đi (đó là `cart`).
- **Nơi ở:** bảng `orders`, `src/features/orders/`.
- **Chủ:** <tên>, <YYYY-MM-DD>.

## 3. Tầng kiến trúc (tên thật của dự án)
| Tầng 4S | Tên trong dự án | Thư mục | Được import gì |
|:---|:---|:---|:---|
| UI | `<components>` | `<src/components>` | Controller |
| Controller | `<handlers>` | `<src/api>` | Use Case, Infrastructure |
| Use Case | `<services>` | `<src/services>` | Entity |
| Entity | `<domain>` | `<src/domain>` | không gì cả |
| Infrastructure | `<adapters>` | `<src/db>` | Entity |

## 4. Bản đồ việc → nơi ở (nối `S2`)
| Việc | File duy nhất | Ghi chú |
|:---|:---|:---|
| thêm thiết bị | `src/features/devices/them_thiet_bi.js` | |

## 5. Từ bị CẤM dùng
| Không dùng | Dùng thay bằng | Vì sao |
|:---|:---|:---|
| `data`, `info`, `temp`, `utils`, `common` | tên đúng việc | không nói được nó là gì |

## 6. Quyết định nghĩa (bất biến, có ngày)
| Ngày | Từ | Chốt nghĩa | Chủ | Lý do |
|:---|:---|:---|:---|:---|

## 7. Đơn vị và định dạng
| Nhóm | Quy ước |
|:---|:---|
| Thời gian | <lưu UTC hay giờ địa phương, định dạng, ai đổi múi giờ> |
| Tiền | <đơn vị nhỏ nhất, làm tròn kiểu gì> |
| Đại lượng đo | <đơn vị SI, độ chính xác> |
```

## 6. Cách điền lần đầu

1. **Đọc trước, hỏi sau.** Lấy nghĩa từ code đang chạy, schema, README, tên bảng, tên topic.
   Chỉ hỏi người dùng những từ **code không trả lời được**.
2. **Ưu tiên từ gây tốn kém nhất khi hiểu sai:** tên thực thể nghiệp vụ (đơn hàng, thiết bị,
   phiên, gói cước), đơn vị đo (giây/ms, VND/nghìn đồng, UTC/giờ địa phương), trạng thái.
3. **Cách hỏi là việc của skill `4s-interview`** (cổng `ASK`): hỏi theo lô, mỗi câu một lựa
   chọn lấy từ code. Skill này chỉ nhận câu trả lời đã chốt rồi ghi, không tự soạn phiếu hỏi.
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
- Từ điển là của con người. Nghĩa lấy thẳng từ code hay schema thì agent ghi và báo một dòng.
  Nghĩa phải chọn giữa hai cách hiểu thì người chốt, ghi tên vào mục 6.
- Người đọc từ điển không bắt buộc biết code. Câu nào chỉ dev hiểu thì câu đó viết lại.
