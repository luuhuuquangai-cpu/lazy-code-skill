# Mẫu `LEXICON.md`

Sao nguyên khối dưới đây vào `LEXICON.md` ở gốc repo, xoá dòng ví dụ khi đã điền thật.
Luật và cách điền: `SKILL.md` cùng thư mục.

```markdown
# LEXICON — Từ điển dự án <tên>

> Đọc file này TRƯỚC khi viết code. Gặp từ chưa có → thêm vào đây trước, code sau.
> Một từ = một nghĩa, một khái niệm = một từ. Không cần biết code vẫn đọc hiểu file này.
> Cập nhật lần cuối: <YYYY-MM-DD>, bởi <tên>.

## 1. Cách đọc file này (60 giây)

| Bạn cần | Đọc phần |
|:---|:---|
| hiểu một từ | **Nghĩa** và **Ví dụ** |
| biết một thứ có tính là từ đó không | **KHÔNG phải** — đây là phần trả lời |
| biết nên gọi nó bằng từ nào | **Tránh dùng** — các từ bị loại |
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
- **Tránh dùng:** đừng gọi là "purchase", "transaction", "bill" — ba từ này còn dùng cho việc khác.
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

## 8. Chưa rõ (chờ chốt)
| Từ / câu hỏi | Ai biết | Đang chặn việc gì |
|:---|:---|:---|
```
