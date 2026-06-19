# Wizard-of-Oz — Flow Script + Sample Output

## Mục tiêu

Test hypothesis: *Du khách tự túc thật sự thấy đau khi tự lên lịch trình — pain đủ lớn để đổi tool.*

Bằng cách giả vờ TraVy đã chạy, bạn (người đứng sau) làm backend người — user nghĩ họ đang chat với AI thật.

## Setup

- **Giao diện**: Google Form hoặc chat page tĩnh (có thể dùng `fake-door-landing-page.html` sửa lại)
- **User**: 5-10 du khách tự túc (nội địa hoặc quốc tế) đã từng tự lên lịch trình Việt Nam
- **Thời gian**: 45-60 phút / user (30 phút test + 15 phút phỏng vấn sau)
- **Data chuẩn bị sẵn**: danh sách 170+ địa điểm (từ TraVy RAG), Google Maps khoảng cách, giờ mở cửa mẫu

## Flow từng bước

### 1. Giới thiệu (3 phút)

> "Cảm ơn bạn đã tham gia. Bạn sắp dùng thử TraVy — một trợ lý AI tạo lịch trình du lịch Việt Nam. Hãy tưởng tượng bạn sắp có chuyến đi và cần lên lịch. Bạn cứ nhập nhu cầu như chat bình thường."

### 2. User nhập nhu cầu (5 phút)

User điền / chat: điểm đến, ngày đi, đi cùng ai, sở thích, ghi chú.

### 3. Bạn làm backend (15-20 phút)

Trong lúc user chờ (bạn bảo "hệ thống đang xử lý..."):
1. Tra cứu địa điểm phù hợp từ data có sẵn
2. Dùng ChatGPT hỗ trợ gợi ý + Google Maps check khoảng cách
3. Viết lịch trình theo ngày + kèm citation (theo format mẫu bên dưới)

### 4. Trả output cho user (5 phút)

Gửi lịch trình hoàn chỉnh trong giao diện.

### 5. Phỏng vấn sau (15 phút)

Hỏi:
- "Bạn thấy lịch trình này thế nào? Có đi được không?"
- "Có địa điểm nào bạn thấy sai / không phù hợp không?"
- "Bạn có tin mấy cái citation này không?"
- "Nếu có tool làm được vậy thật, bạn có dùng không? Vì sao?"
- "Bạn sẵn sàng trả bao nhiêu / tháng?"

## Sample Output Format

```
📍 LỊCH TRÌNH: HỘI AN — 3 NGÀY 2 ĐÊM
🧑‍🤝‍🧑 Nhóm: 2 người, thích ẩm thực + chụp ảnh
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📅 NGÀY 1 — Đến + Phố cổ

Sáng:
  • 08:00 — Đến Đà Nẵng, di chuyển về Hội An (45 phút)
  • 09:30 — Check-in homestay, thả bộ phố cổ
  • 11:00 — Cà phê tại Phin Coffee (150 Nguyễn Thái Học) — không gian xưa, view mái ngói rêu phong

Trưa:
  • 12:30 — Ăn trưa: Cao lầu Bá Lễ (49/3 Phan Châu Trinh) — 35k/bát
  • 14:00 — Chụp ảnh Chùa Cầu + Nhà cổ Tấn Ký (mở cửa 07:00-17:30)

Chiều:
  • 16:00 — Chợ Hội An — trái cây, đồ khô, quà lưu niệm
  • 17:30 — Hoàng hôn bến sông Hoài — ngắm đèn lồng lên đèn

Tối:
  • 19:00 — Ăn tối: Mì Quảng Bích (407/27 Cửa Đại) — 40k/tô
  • 20:30 — Đi thuyền hoa đăng (30k/người) — thả đèn trên sông Hoài

📅 NGÀY 2 — Làng quê + Biển

Sáng:
  • 07:00 — Ăn sáng: Bánh mì Madam Khanh (115 Trần Cao Vân) — 25k/cái
  • 08:30 — Đạp xe thăm Làng rau Trà Quế (cách trung tâm 3km) — miễn phí vào cổng
  • 10:30 — Làng mộc Kim Bồng — xem thợ đục gỗ, có thể mua đồ lưu niệm

Trưa:
  • 12:00 — Ăn trưa: Cơm gà Hội An (41 Phan Chu Trinh) — 50k/suất

Chiều:
  • 14:00 — Biển An Bàng — tắm biển, chụp ảnh, cách phố cổ 4km (xe ôm 20k hoặc đạp xe 15 phút)
  • 17:00 — Quán cà phê The Deckhouse Hội An — view biển, đồ uống 45-70k

Tối:
  • 19:00 — Ăn tối: White Rose (Quán Mùi, 22 Phan Bội Châu) — 60k/suất
  • 20:30 — Dạo chợ đêm Hội An — lồng đèn, quà vặt

📅 NGÀY 3 — Buổi sáng cuối + Về

Sáng:
  • 07:00 — Ăn sáng: Cháo lòng (đầu đường Nguyễn Huệ) — 20k/bát
  • 08:00 — Tham quan làng gốm Thanh Hà (cách 3km) — vé 30k
  • 09:30 — Mua quà: Đặc sản Hội An tại chợ trung tâm

Trưa:
  • 11:00 — Trả phòng, di chuyển ra sân bay Đà Nẵng

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📌 Ghi chú:
  • Giá cả có thể thay đổi — nên gọi trước khi đến
  • Nên đặt homestay trước 1-2 tuần, nhất là mùa cao điểm (tháng 3-8)
  • Tham khảo thêm: [citation: TraVy data — Hội An destinations v1.2]
```

## Kịch bản backup

| User action | Bạn làm gì? |
|-------------|-------------|
| User hỏi trong lúc chờ | "Hệ thống đang tổng hợp dữ liệu từ 170+ địa điểm, khoảng 15-20 giây nữa ạ" |
| User muốn sửa lịch trình | Bạn sửa tay, gửi lại — note lại insight: user muốn sửa gì? |
| User hỏi sao biết chỗ này | Bạn chỉ vào citation: "Dữ liệu từ nguồn đã kiểm duyệt của TraVy" |
| User không tin lịch trình | Ghi nhận, hỏi lý do — trust barrier insight rất giá trị |
