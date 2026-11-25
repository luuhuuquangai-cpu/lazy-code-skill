---
name: 4s-lexicon
description: >
  Tạo và bảo trì LEXICON.md ở gốc repo, từ điển chung để người và agent hiểu cùng một nghĩa trước
  khi viết code: từ vựng nghiệp vụ, tên tầng kiến trúc, việc nào nằm ở file nào, đơn vị đo. Skill
  4s gọi skill này khi repo chưa có từ điển. Dùng khi một từ bị hiểu hai kiểu, khi đặt tên module
  hay feature, khi cần chốt nghĩa thuật ngữ. Triggers: "từ điển dự án", "thống nhất định nghĩa",
  "lexicon", "glossary", "thuật ngữ", "hiểu sai ý nhau", /4s-lexicon.
---

# Từ điển dự án (`LEX`)

Mục tiêu: **hai người (người và agent) không bao giờ phải đoán một từ nghĩa là gì.**
Không có từ điển thì mỗi lần đọc code là một lần suy diễn lại — đó là nguồn lỗi đắt nhất
và là thứ duy nhất mà "viết ít code" không giải quyết được.

File: `LEXICON.md` ở gốc repo. Một file, một chỗ (`S2`). Vượt 200 dòng thì tách theo
miền nghiệp vụ: `LEXICON.devices.md`, `LEXICON.billing.md`, giữ `LEXICON.md` làm mục lục.

## 1. Vòng đời

```
Repo mới / chưa có LEXICON.md
  └─ đọc code + README + schema → điền mục 2 (từ vựng), 3 (tầng), 4 (việc → nơi ở), 7 (đơn vị)
     └─ hỏi người dùng CHỈ những từ còn mâu thuẫn → ghi vào mục 6 (Quyết định)

Trong lúc code
  └─ gặp từ chưa có trong từ điển
     └─ tự định nghĩa được từ code/schema? → CÓ: ghi luôn, báo 1 dòng
                                            → KHÔNG: dừng, hỏi người dùng, rồi ghi

Review / kiểm repo
  └─ từ mới trong code chưa có trong LEXICON.md → phát hiện `LEX` (nên sửa)
  └─ một từ mang hai nghĩa → phát hiện `LEX` (phải sửa)
```

**Luật cứng:** một từ chỉ được có **một** nghĩa. Hai nghĩa → phải tách thành hai từ khác nhau,
không được dùng chung một từ với hai nghĩa. Đây là `S1` áp cho ngôn ngữ.

## 2. Cấu trúc bắt buộc

Mỗi mục từ có đúng 5 phần. Thiếu phần nào thì mục từ đó chưa dùng được:

| Phần | Trả lời câu hỏi | Vì sao cần |
|:---|:---|:---|
| **Từ** | gọi là gì | để grep và đặt tên file |
| **Nghĩa** | là cái gì, một câu | để hiểu |
| **KHÔNG phải** | không phải cái gì | phần quan trọng nhất — ranh giới |
| **Nơi ở** | file / bảng / topic nào | để tìm (nối `S2`) |
| **Chủ** | ai chốt nghĩa này | để biết hỏi ai khi nghi ngờ |

Phần **"KHÔNG phải"** là phần bị bỏ qua nhiều nhất và cũng là phần cứu nhiều bug nhất:
`Device` nghĩa là "thiết bị vật lý đã đăng ký", **không phải** "bản ghi trong bảng devices"
— hai thứ đó khác nhau và code sẽ sai nếu lẫn.

## 3. Mẫu `LEXICON.md`

Sao nguyên khối dưới đây vào `LEXICON.md`, xoá dòng ví dụ khi đã điền thật.

```markdown
# LEXICON — Từ điển dự án <tên>

> Đọc file này TRƯỚC khi viết code. Gặp từ chưa có → thêm vào đây trước, code sau.
> Một từ = một nghĩa. Cập nhật lần cuối: <YYYY-MM-DD>, bởi <tên>.

## 1. Cách đọc file này
Mỗi mục từ: **Nghĩa** (là gì) · **KHÔNG phải** (ranh giới) · **Nơi ở** (code nào) · **Chủ**.

## 2. Từ vựng lõi (dùng ở mọi nơi)

### Thiết bị (`device`)
- **Nghĩa:** một thiết bị vật lý đã đăng ký với hệ thống và có mã định danh duy nhất.
- **KHÔNG phải:** không phải một dòng trong bảng `devices`; không phải "kênh" MQTT.
- **Nơi ở:** bảng `devices`, `src/features/devices/`.
- **Chủ:** <tên>.

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

## 4. Cách điền lần đầu

1. **Đọc trước, hỏi sau.** Lấy nghĩa từ code đang chạy, schema, README, tên bảng, tên topic.
   Chỉ hỏi người dùng những từ **code không trả lời được**.
2. **Ưu tiên từ gây tốn kém nhất khi hiểu sai:** tên thực thể nghiệp vụ (đơn hàng, thiết bị,
   phiên, gói cước), đơn vị đo (giây/ms, VND/nghìn đồng, UTC/giờ địa phương), trạng thái.
3. **Hỏi theo lô, mỗi câu một lựa chọn.** Không hỏi "nghĩa là gì?" mà đưa 2–3 nghĩa ứng viên
   lấy từ code để người dùng chọn. Chọn xong mới ghi.
4. **Ghi cả từ đã thống nhất mà code đang làm SAI.** Đó là bug thật, ghi vào mục 6 kèm mã `LEX`.
5. Dừng lại khi hết từ gây tranh cãi. Từ điển 15 mục từ đúng nghĩa tốt hơn 100 mục từ sao chép.

## 5. Đơn vị và định dạng — luôn ghi rõ (mục 7 của mẫu)

Ba nhóm gần như chắc chắn gây bug nếu không ghi:

| Nhóm | Ghi rõ trong từ điển |
|:---|:---|
| Thời gian | mốc lưu là UTC hay giờ địa phương, định dạng nào, ai đổi múi giờ |
| Tiền | đơn vị nhỏ nhất, có làm tròn không, làm tròn kiểu gì, ai chịu thuế/phí |
| Số lượng / nhiệt độ / công suất | đơn vị SI nào, dấu thập phân, độ chính xác |

`LedgerEntry.amount` không nói gì thì một người viết 1000 (=1.000đ), người kia hiểu 1000đ.
Chỉ một dòng trong từ điển chặn được bug đó.

## 6. Ranh giới

- Từ điển chỉ chứa **định nghĩa**, không chứa hướng dẫn code, không chứa TODO, không chứa lịch sử.
  Nó phình ra là vì bị dùng sai mục đích.
- Skill này **sửa được** `LEXICON.md` (đó là việc của nó). Không sửa file code khác.
- Từ điển lệch code (code làm khác định nghĩa) → **code sai**, hoặc từ điển sai. Không được để lệch.
  Sửa cái nào cũng phải nói rõ đã sửa cái nào.
- Từ điển là của con người. Nghĩa lấy thẳng từ code hay schema thì agent ghi và báo một dòng.
  Nghĩa phải chọn giữa hai cách hiểu thì người chốt, ghi tên vào mục 6.
