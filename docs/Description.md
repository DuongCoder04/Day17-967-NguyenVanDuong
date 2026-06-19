# Mô tả dự án TraVy

> Tài liệu này tóm tắt PRD. Khi có khác biệt, [`PRD-TraVy.md`](./PRD-TraVy.md) luôn được ưu tiên.

## 1. Tổng quan

TraVy là trợ lý du lịch AI chuyên biệt cho Việt Nam. Sản phẩm giúp du khách trong nước và quốc tế tạo lịch trình cá nhân hóa qua hội thoại tự nhiên, đồng thời cung cấp bối cảnh văn hóa, tri thức địa phương và các gợi ý ít phổ biến.

Người dùng mô tả điểm đến, người đồng hành, ngày đi, loại trải nghiệm và phong cách du lịch. TraVy dùng dữ liệu RAG đã tuyển chọn để tạo lịch trình theo từng ngày, sau đó cho phép người dùng tinh chỉnh bằng hội thoại.

## 2. Vấn đề

Việc lập kế hoạch du lịch Việt Nam hiện bị phân mảnh giữa công cụ tìm kiếm, bản đồ, nền tảng đặt dịch vụ, blog và cộng đồng. Người dùng thường:

- Mất 4–10 giờ nghiên cứu cho mỗi chuyến đi.
- Nhận các lịch trình chung chung, tập trung vào điểm đông khách.
- Khó tiếp cận tri thức địa phương do rào cản ngôn ngữ.
- Thiếu bối cảnh lịch sử và văn hóa.
- Khó điều chỉnh kế hoạch khi ràng buộc thay đổi.
- Gặp thông tin cũ về giờ mở cửa, giá hoặc trạng thái địa điểm.

## 3. Giải pháp

TraVy hợp nhất quy trình nghiên cứu thành một trải nghiệm:

1. Thu thập 5 thông tin cá nhân hóa qua hội thoại.
2. Truy xuất dữ liệu từ RAG chính.
3. Tạo lịch trình có cấu trúc theo ngày.
4. Bổ sung phương tiện, chi phí, lưu trú và mẹo địa phương ở bản release.
5. Cung cấp chiều sâu văn hóa theo yêu cầu.
6. Dùng dữ liệu cộng đồng có disclaimer để giới thiệu local discovery.
7. Crawl web theo thời gian thực khi RAG thiếu hoặc thông tin nhạy cảm về thời gian.

## 4. Người dùng mục tiêu

### Du khách quốc tế tự túc

Muốn tiết kiệm thời gian, vượt qua rào cản ngôn ngữ và hiểu Việt Nam sâu hơn.

### Du khách nội địa

Muốn khám phá ngoài các địa điểm phổ biến và tiếp cận tri thức địa phương ở dạng có cấu trúc.

### Du khách gia đình

Cần nhịp độ, hoạt động và hậu cần phù hợp với trẻ em hoặc người cao tuổi.

### Du khách có sở thích chuyên biệt

Tập trung vào ẩm thực, lịch sử, thiên nhiên, trekking hoặc nhiếp ảnh.

## 5. Phạm vi MVP

MVP được thực hiện trong Tuần 1–2:

- Chat UI và bảng lịch trình.
- Luồng 5 câu hỏi cá nhân hóa trong hội thoại.
- Tạo lịch trình theo ngày dựa trên RAG.
- Dữ liệu chính cho 5+ tỉnh, thành phố và 50+ địa điểm.
- Hỗ trợ tiếng Anh và tiếng Việt.
- Loading state và xử lý lỗi cơ bản.
- Sử dụng theo phiên, không yêu cầu tài khoản.

MVP chưa gồm auth, saved trips, cultural depth, community data, realtime crawl, chi phí, lưu trú và flagging.

## 6. Phạm vi bản phát hành

Trong Tuần 3–5:

- Cultural depth.
- Community data trong namespace riêng, luôn có disclaimer.
- Realtime crawl fallback.
- Supabase Auth và saved trips.
- Phương tiện, chi phí ước tính và liên kết lưu trú.
- Báo cáo thông tin không chính xác.
- RAG cho 10+ tỉnh, thành phố và 150+ địa điểm.
- Onboarding có thể bỏ qua và giới hạn gói miễn phí.

## 7. Ngoài phạm vi 5 tuần

- Thanh toán hoặc đặt dịch vụ trong ứng dụng.
- Ứng dụng mobile native.
- Social feed, bài đăng cộng đồng và cổng UGC.
- Offline mode.
- Voice interface và push notification.
- Trợ lý thời gian thực trong chuyến đi.
- Hỗ trợ quốc gia ngoài Việt Nam.

## 8. Kiến trúc tóm tắt

### UI

React, TypeScript, Vite và Tailwind CSS tại `apps/web`, gồm chat panel và
itinerary panel responsive trên desktop và mobile.

### Agent

NestJS API tại `apps/api` xử lý nghiệp vụ/auth/persistence và gọi Python AI
service tại `apps/ai-service`. AI service dùng Gemini `gemini-3.1-flash-lite` theo
mẫu ReAct với 4 tool:

- `retrieve_from_rag`
- `retrieve_cultural_context`
- `realtime_crawl`
- `retrieve_community_data`

### Dữ liệu

Supabase PostgreSQL + pgvector với ba namespace:

- `primary`
- `cultural`
- `community`

Pipeline dữ liệu dùng adapter cho open dataset, API và website được duyệt; tạo document version, chuẩn hóa entity/fact, chunk, embed và publish theo chu kỳ.

## 9. Nguyên tắc chất lượng

- Không bịa địa điểm hoặc thông tin vận hành.
- Mọi tuyên bố cụ thể về địa điểm phải dựa trên dữ liệu truy xuất.
- Nếu không đủ dữ liệu, nêu rõ sự không chắc chắn.
- Dữ liệu cộng đồng không được trình bày ngang mức tin cậy với nguồn chính.
- Chỉ hỗ trợ du lịch trong Việt Nam.
- Phản hồi bằng ngôn ngữ người dùng.

## 10. Chỉ số thành công

### MVP

- Độ chính xác địa điểm ≥90%.
- ≥90% đầu vào hợp lệ tạo được lịch trình.
- ≥80% truy vấn trả ít nhất 3 chunk RAG liên quan.
- 90% phản hồi ban đầu hoàn tất trong 15 giây.
- Không có lỗi P0 trong luồng cốt lõi.

### Release

- Cultural depth hoạt động với ≥80% truy vấn địa điểm.
- Nội dung nâng cao xuất hiện trong ≥70% lịch trình.
- Dữ liệu cộng đồng bao phủ ≥20 địa điểm tại ≥5 tỉnh.
- Báo cáo tới hàng đợi đánh giá với độ tin cậy 100%.

## 11. Tiến độ

| Sprint | Tuần | Kết quả |
| --- | --- | --- |
| Sprint 1 | 1 | Agent + RAG v1 + UI cơ bản |
| Sprint 2 | 2 | MVP và cổng GO/NO-GO |
| Sprint 3 | 3 | Cultural, community, auth |
| Sprint 4 | 4 | Lịch trình nâng cao và flagging |
| Sprint 5 | 5 | QA, beta và production |
