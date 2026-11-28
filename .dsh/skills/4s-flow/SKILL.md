---
name: 4s-flow
description: >
  Luồng làm việc hệ thống cho một dự án: từ ý mơ hồ đến code xong, biết đang ở bước nào, bước nào
  bắt buộc hỏi cho hết, và khi nào được xoá context. Gồm kỷ luật context (vùng suy nghĩ còn sáng,
  năm lựa chọn ở ranh giới bước). Dùng khi bắt đầu một việc lớn, khi không biết nên hỏi hay nên
  làm, khi session dài và bắt đầu lú, khi muốn biết 4S chạy theo trình tự nào. Triggers: "luồng
  làm việc", "4s flow", "quy trình 4s", "bắt đầu từ đâu", "context dài quá", "khi nào clear",
  /4s-flow.
argument-hint: "[việc đang làm hoặc tên bước]"
---

# Luồng làm việc 4S (`FLOW`)

4S có bốn luật code, nhưng chưa có **trình tự**. Skill này là trình tự đó: một việc đi qua năm
bước, mỗi bước có điều kiện vào và điều kiện ra rõ ràng. Biết mình đang ở bước nào thì biết được
nên hỏi hay nên làm — và đó là thứ thiếu nhất khi làm với agent.

## 1. Năm bước

```
1. HỎI      → 2. CHỐT     → 3. LÀM       → 4. KIỂM     → 5. GHI LẠI
   4s-interview  4s-lexicon    code           4s-review     4s-lexicon
   (dồn hết câu hỏi) (ghi từ điển) (viết)      (soát diff)    (bài học)
```

| Bước | Làm gì | Xong khi nào | Không được sang bước sau nếu |
|:---|:---|:---|:---|
| **1. HỎI** | dồn mọi câu hỏi vào một chỗ, hỏi theo vòng | biên rỗng, bạn gật "đúng rồi" | còn câu chưa trả lời |
| **2. CHỐT** | ghi nghĩa vào `LEXICON.md` mục 2 + mục 6 | từ điển đã có dòng cho mọi từ mới | từ mới chưa có trong từ điển |
| **3. LÀM** | viết code theo bốn luật, diff nhỏ nhất | code chạy, test nhỏ xanh | còn chỗ phải đoán |
| **4. KIỂM** | `4s-review` trên diff | không còn phát hiện chưa xử lý | còn `S1`–`S4` vi phạm chưa sửa/khai |
| **5. GHI LẠI** | cập nhật từ điển nếu bước 4 lộ ra nghĩa sai | từ điển khớp code | từ điển lệch code |

**Việc nhỏ thì gộp bước.** Sửa một typo: không cần bước 1, không cần bước 4. Việc càng lớn thì
càng không được bỏ bước nào — đặc biệt bước 1, vì bỏ nó là bỏ luôn cả cái khiến 4S khác code
thường.

## 2. Hỏi dồn một chỗ (`GATE`)

Đây là luật quan trọng nhất của skill này, và là chỗ dễ vi phạm nhất:

- **Mọi câu hỏi thuộc bước 1.** Vào bước 3 là hết hỏi.
- Đang làm mà thấy cần hỏi → **dừng, quay lại bước 1**, hỏi cho hết, rồi làm lại từ chỗ dừng.
  Không vừa code vừa hỏi: mỗi câu đến muộn là một lần sửa lại code vừa viết.
- Hỏi rải rác làm bạn mệt và làm agent trông như đang mò. Hỏi dồn thì một lượt là xong.

Lý do phải dồn: câu trả lời của bạn ở câu 5 thường **làm đổi** cách hiểu câu 1. Hỏi rải thì phải
hỏi lại câu 1.

## 3. Kỷ luật context (`CTX`)

Agent chỉ suy nghĩ sắc trong một **vùng còn sáng** — khoảng 150k token đầu của một session. Qua
khỏi đó, chất lượng tụt mà không báo. Vì thế:

- **Giữ bước 1 → 2 → 3 trong cùng một mạch**, đừng xoá context giữa chừng. Lý do hỏi, lý do chọn
  nghĩa, và code viết ra phải nằm cùng một chỗ — xoá rồi thì cái "vì sao" mất, đọc lại diff không
  lấy lại được.
- **Việc lớn thì chia bước 3 thành nhiều phiên**, mỗi phiên một phần đã chốt xong ở bước 2. Từ
  điển là thứ mang qua các phiên.
- Thấy session bắt đầu lú (trả lời lan man, quên quyết định cũ, sửa đi sửa lại một chỗ) → đó là
  dấu hiệu hết vùng sáng, không phải dấu hiệu việc khó.

### Năm lựa chọn ở ranh giới bước — xét theo thứ tự, dừng ở cái đầu tiên đúng

| # | Câu hỏi | Chọn | Vì sao |
|:--|:---|:---|:---|
| 1 | Bước sau có cần chính mạch suy nghĩ này không? | **Đi tiếp** | Không mất gì, không mất "vì sao". Luôn xét cái này trước |
| 2 | Context này có dùng lại được gì cho bước sau không? | **Xoá context** (`/clear`) | Rẻ nhất, trả lại cả vùng sáng. Xoá nhầm thì mất "vì sao" |
| 3 | Có phải đổi máy, đổi thư mục, hay đưa cho người khác? | **Bàn giao** (viết file markdown) | Chỉ đáng khi thứ gì đó phải *di chuyển* |
| 4 | Việc này chạy một mình được, không cần bạn can thiệp? | **Giao agent phụ** | Session này không bị bẩn |
| 5 | Còn lại | **Nén context** (`/compact`) | Mặc định, nhưng là **cuối bảng**, không phải cái chọn đầu tiên |

**Luôn xét ở ranh giới bước, không xoá giữa bước.** Xoá giữa bước làm agent mất mạch. Đang làm dở
mà cần giải phóng context thì chia phần còn lại cho agent phụ, đừng nén.

**Cái giá phải biết:** mọi lựa chọn trừ "đi tiếp" đều biến **bản gốc** thành **bản tóm tắt**.
Bản gốc: đủ thông tin, nhiều nhiễu, ít chỗ thở. Bản tóm tắt: thiếu thông tin, ít nhiễu, nhiều chỗ
thở. Chỉ trả cái giá đó khi ở lại đắt hơn bỏ đi.

## 4. Việc lớn quá một phiên

Chưa nhìn thấy đường tới đích: đừng chia thành ticket code vội. Làm bản đồ quyết định trước:

- **Đích** chốt trước — nó quyết định phạm vi. Đích có thể là một bản đặc tả, một quyết định
  phải chốt, hoặc một thay đổi tại chỗ.
- Mỗi mục trên bản đồ là **một câu hỏi cần chốt**, không phải một việc cần làm. Việc làm để sau.
- **Sương mù:** biết là sẽ có quyết định nhưng chưa phát biểu nổi thành câu hỏi → ghi vào mục
  "chưa rõ". **Câu hỏi đã sắc thì thành mục, kể cả đang bị chặn.** Đừng cắt sương mù thành từng
  mảnh vụn.
- Chốt xong một câu thì những câu phía sau mới hiện ra rõ. Cứ thế cho tới khi đường sáng.
- **Làm một câu mỗi phiên.** Ham làm là dấu hiệu đã tới mép bản đồ, đến lúc bắt tay code.

Không cần issue tracker: `LEXICON.md` mục "chưa rõ" + một file `KE_HOACH.md` ở gốc repo là đủ.

## 5. Bước 5 hay bị bỏ nhất

Sau khi sửa xong, hỏi một câu: **có phải lần này lẽ ra phải biết trước không?**

- Nếu lỗi vì hiểu khác nghĩa hoặc khác đơn vị → từ điển thiếu, cập nhật (`4s-lexicon` mục 8).
- Nếu lỗi vì không có chỗ chặn → thêm một phép kiểm hoặc một dòng vào `AGENTS.md`.
- Nếu lỗi vì agent đoán bừa → luật hỏi lại có lỗ hổng, siết lại.

Sửa cái **môi trường**, không sửa lại code lần nữa. Lần sau làm việc khác sẽ nhẹ hơn.

## 6. Ranh giới

- Skill này **không sửa code, không sửa từ điển**. Nó chỉ nói đang ở bước nào và được làm gì.
- Việc nhỏ: nói một dòng "việc nhỏ, bỏ bước 1 và 4" rồi làm. Đừng bắt cả quy trình cho một dòng
  code — quy trình phục vụ việc, không phải ngược lại.
- Bước 1 dùng `4s-interview`, bước 2 và 5 dùng `4s-lexicon`, bước 4 dùng `4s-review`. Skill này
  không lặp lại nội dung của chúng.
