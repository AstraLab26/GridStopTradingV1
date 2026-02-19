# Grid Stop Trading V1 (Bản có Panel)

EA lưới chỉ dùng lệnh **Buy Stop** và **Sell Stop** trên MetaTrader 5. Buy Stop đặt trên đường gốc, Sell Stop đặt dưới đường gốc. Hỗ trợ gồng lãi, cân bằng lệnh, giờ hoạt động và dừng theo tích lũy. **Bản có Panel** hiển thị thông tin trực tiếp trên chart.

---

## Tổng quan

- **Đường gốc**: Giá Bid tại thời điểm EA khởi động (hoặc sau mỗi lần reset).
- **Lưới**: Các mức giá cách đường gốc theo pips (cố định hoặc cấp số cộng).
- **Stop A**: Lệnh Stop có gấp thếp (martingale) theo mức lưới.
- **Stop B**: Lệnh Stop không gấp thếp, lot cố định, có gồng lãi riêng cho từng lệnh.

Mỗi level tối đa 1 Stop A (nếu bật) và 1 Stop B (nếu bật). A và B có thể cùng level, cài đặt và hoạt động độc lập.

---

## Cài đặt lưới

| Input | Mô tả |
|-------|------|
| Chế độ khoảng cách | **Cố định** (0): bậc n cách n×x pips. **Cấp số cộng** (1): bậc 1 = x, bậc 2 cách bậc 1 = 2x, bậc 3 cách bậc 2 = 3x... |
| Khoảng cách lưới cơ sở (pips) | Khoảng cách cơ sở / bậc 1 |
| Khoảng cách lưới cố định (pips) | Dùng khi Cấp số cộng: bậc 1 = x, bậc 2 cách bậc 1 = 2x, bậc 3 cách bậc 2 = 3x... |
| Tự động bổ sung lệnh | Bật: khi lệnh đạt TP thì đặt lại lệnh chờ tại level đó |
| Tối đa lệnh mở để bổ sung | VD: 2 = có 2 lệnh Buy mở thì không bổ sung Buy Stop |

---

## Stop A (có gấp thếp)

| Input | Mô tả |
|-------|------|
| Bật lệnh Stop A | Bật/tắt Stop A |
| Số lượng lưới tối đa | Số level mỗi chiều (tối đa 100) |
| Khối lượng (mức 1) | Lot cho level gần gốc nhất |
| Chế độ TP | Pips cố định hoặc theo lưới (bậc k → TP tại bậc k+1) |
| Take Profit (pips) | Khi chế độ = Pips cố định |
| Bật gấp thếp | Lot tăng theo mức lưới |
| Chế độ gấp thếp | Cấp số nhân (x2, x4...) hoặc cấp số cộng (+0.01 mỗi bậc) |
| Giới hạn lot | Lot tối đa mỗi lệnh (0 = không giới hạn) |

---

## Stop B (không gấp thếp)

| Input | Mô tả |
|-------|------|
| Bật lệnh Stop B | Bật/tắt Stop B |
| Số lượng lưới tối đa | Số level mỗi chiều (có thể khác Stop A, VD: A=20, B=10) |
| Khối lượng | Lot cố định mọi mức |
| Chế độ TP / Take Profit | Giống Stop A |
| Bật gồng lãi cho từng lệnh Stop B | Khi giá cách entry X pips → đặt SL dương, giá đi thêm step → dịch SL theo step |
| Khi giá cách entry X pips | Kích hoạt đặt SL (break-even) |
| Step pips | Bước dịch SL theo giá |

---

## Trading Stop, Step Tổng (Gồng lãi)

Khi lãi đạt ngưỡng → EA xóa lệnh chờ gần giá, xóa TP, đặt SL tại điểm A, đóng lệnh ngược chiều, rồi gồng lãi (dịch SL theo step).

| Input | Mô tả |
|-------|------|
| Chế độ gồng lãi | Theo lệnh mở / Theo phiên / Cả 2 |
| Lãi kích hoạt (USD) | Ngưỡng lãi để bật gồng lãi |
| Lãi quay lại (USD) | Ngưỡng để hủy gồng lãi (chỉ khi chưa đặt SL) |
| Điểm A (pips) | Khoảng cách từ lệnh dương gần nhất |
| Step (pips) | Bước dịch SL |
| Hành động khi chạm SL | Dừng EA hoặc Reset EA |

---

## Chế độ cân bằng lệnh

Ưu tiên trước gồng lãi. Khi **cả hai** điều kiện đều đạt → đóng hết lệnh mở + lệnh chờ → đặt đường gốc mới tại giá hiện tại → tiếp tục đặt lệnh. Lãi phiên = Equity hiện tại − Equity khi bắt đầu phiên (hoặc sau lần reset gần nhất).

| Input | Mô tả |
|-------|------|
| Bật | Bật/tắt chế độ cân bằng lệnh |
| Điều kiện 1: Tổng lot lệnh đang mở ≥ X | Chỉ tính lệnh đang mở (không tính lệnh chờ). Đặt 0 = không kiểm tra điều kiện này |
| Điều kiện 2: Lãi phiên đạt tối thiểu (USD) | Phiên đang lãi ≥ X USD. Đặt 0 = không kiểm tra điều kiện này |

**Lưu ý:** Cần ít nhất một điều kiện > 0 để chế độ hoạt động. Đặt cả hai = 0 thì chế độ không kích hoạt.

---

## Giờ hoạt động

| Input | Mô tả |
|-------|------|
| Bật giờ hoạt động | EA chỉ chạy trong khoảng giờ cài đặt |
| Giờ/phút bắt đầu, kết thúc | Khoảng thời gian EA được phép đặt lệnh mới |

Nếu đang có lệnh mở mà hết giờ → EA vẫn quản lý cho đến khi reset và không còn lệnh.

---

## Dừng EA theo tích lũy lãi

- Tích lũy = Số dư hiện tại − Số dư khi EA khởi động.
- Khi tích lũy ≥ ngưỡng → EA dừng (đóng hết lệnh, không đặt lệnh mới). Kiểm tra sau mỗi lần reset (cân bằng lệnh, Trading Stop, v.v.).

---

## Cài đặt chung

| Input | Mô tả |
|-------|------|
| Magic Number | Magic của lệnh do EA đặt |
| Comment | Comment cho lệnh |
| Bật thông báo khi EA reset | Gửi push notification về điện thoại khi EA reset |
| Hiển thị panel | Bật/tắt panel thông tin trên chart |

---

## Panel

| Thông tin | Mô tả |
|-----------|------|
| Gốc (đường gốc) | Giá cơ sở |
| Buy / Sell / Chờ | Số lệnh mở và lệnh chờ |
| Gồng lãi | ON/OFF |
| USD tích lũy | Tích lũy từ lúc EA bật |
| Lãi/Lỗ Stop A | Lãi/lỗ Stop A kể từ lần reset gần nhất |
| Lãi/Lỗ Stop B | Lãi/lỗ Stop B kể từ lần reset gần nhất |

Số tiền lớn hiển thị ngắn: 1000 → 1.00K USD, 1 triệu → 1.00M USD.

---

## Thông báo điện thoại

Khi bật **Bật thông báo về điện thoại khi EA reset**, mỗi lần reset EA sẽ gửi tin nhắn với nội dung:

```
EA RESET
Biểu đồ: EURUSD
Chức năng: Cân bằng lệnh
Số dư: 93.44K
Tích lũy lần 3: 800.00 + 1481.82 = 2281.82
Lỗ lớn nhất: -120.50 / 30000.00 (0.40%)
Lot: 0.16 / 1.28
```

**Chức năng** có thể là: `Cân bằng lệnh`, `Trading Stop, Step Tổng`, `Thủ công`, v.v.

---

## Yêu cầu

- MetaTrader 5
- Tài khoản cho phép giao dịch tự động
- Bật thông báo trong MT5: Tools → Options → Notifications → Enable push notifications

---

## Cài đặt

### Bản có Panel (GridStopTradingV1.mq5)

1. Copy file `GridStopTradingV1.mq5` vào thư mục `MQL5/Experts/`.
2. Biên dịch trong MetaEditor.
3. Gắn EA lên chart.
4. Input **Hiển thị panel** = true để xem thông tin trên chart.

### Bản NoPanel (nhẹ, không panel)

- File: `GridStopTradingV1_NoPanel.mq5`
- Không có panel hiển thị trên chart → EA nhẹ, chạy mượt hơn.
- Chức năng giống bản có Panel (Stop A/B, gồng lãi, cân bằng lệnh, v.v.).

---

## Phiên bản

**V1.0** – Grid Stop Trading
