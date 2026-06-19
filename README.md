# Day 17 — Main Studio v4

## Thông tin học viên

| Trường | Mã học viên | Họ tên | Dự án đang làm | Vai trò của bạn trong dự án |
|--------|-------------|--------|----------------|----------------------------|
| VinUniversity | 2A202600967 | Nguyễn Văn Dưỡng | TraVy — trợ lý du lịch AI cho Việt Nam | Product / AI Engineer |

## Build-up Chain

| Hạng mục | Link / ghi chú |
|----------|----------------|
| Day 16 artifact 02 | [02-jtbd-project-analysis.md](https://github.com/DuongCoder04/Day16-A20-00967-NguyenVanDuong/blob/main/worksheet/02-jtbd-project-analysis.md) |
| Present board / frame / file của bạn | README.md + artifacts trong repo ([link](https://github.com/DuongCoder04/Day17-967-NguyenVanDuong)) |

---

## Bước 0 — JTBD Checkpoint

| Câu hỏi | Điền |
|---------|------|
| Core JTBD hiện tại là gì? | **Lên lịch trình du lịch phù hợp sở thích cá nhân và ràng buộc thời gian cho chuyến đi ngắn ngày đến nơi chưa quen** |
| Current workflow hiện tại là gì? | Du khách tự gom thông tin từ Google search + blog du lịch + group Facebook + TripAdvisor/Reddit (quốc tế) + hỏi bạn bè → copy-paste vào Google Sheets/Notes → tự sắp xếp thành lịch trình. Gần đây một số dùng ChatGPT/Gemini nhưng gặp hallucination và thiếu dữ liệu Việt Nam chuyên sâu. |
| Pain step đau nhất là gì? | **Locate** (tìm địa điểm/ẩm thực/hoạt động phù hợp) + **Prepare** (sắp xếp thành lịch trình theo ngày hợp lý) — chiếm 80–90% thời gian research (4–10 giờ) |
| AI leverage point nằm ở bước nào? | **Locate → Prepare liên tục**: AI tổng hợp địa điểm từ RAG rồi sắp xếp thành lịch trình theo ngày — gộp 2 bước đau nhất thành 1 bước qua hội thoại |
| Điểm nào còn mơ hồ nhất trước khi vào phần hypothesis? | Chưa validate được rằng pain thật sự đủ lớn với du khách nội địa và quốc tế — có thể họ thích tự research hoặc ChatGPT generic đã đủ tốt, khiến toàn bộ hypothesis sụp trước khi đầu tư RAG/feature |

---

## Bước 1 — Chọn 1 Hypothesis Quan Trọng

| Câu hỏi | Điền |
|---------|------|
| Hypothesis bạn chọn là gì? | **Du khách tự túc (nội địa và quốc tế) thật sự thấy đau khi tự lên lịch trình du lịch Việt Nam — pain đủ lớn (4–10 giờ research) để họ đổi sang một tool chuyên biệt** |
| Hypothesis này thuộc phần nào của dự án? | **Value hypothesis** — cốt lõi nhất: nếu pain không đủ lớn thì toàn bộ TraVy không có lý do tồn tại, bất kể AI có mạnh đến đâu |
| Nếu nó sai thì chuyện gì sập / yếu đi? | Toàn bộ TraVy sập: không ai cần tool chuyên biệt để giải pain đã không tồn tại — hoặc ChatGPT generic đã đủ tốt, hoặc user thích tự research |
| Vì sao bạn chọn hypothesis này để làm bài hôm nay? | Đây là **assumption nguy hiểm nhất** (Day 16 đã xác định). Mọi feature investment (RAG, chatbot, itinerary generation) đều dựa trên giả định pain này có thật. Nếu chưa test nó, rủi ro build sai thứ rất cao. |

---

## Bước 2 — Ghi Lại Current Approach Hiện Tại Của Dự Án

| Câu hỏi | Điền |
|---------|------|
| Dự án đã từng làm / đang định làm gì để test hypothesis này? | Đang định **build full MVP**: web app (React/Vite/Tailwind) + NestJS API + Python AI service (Gemini + RAG) + Supabase PostgreSQL/pgvector — rồi mới đưa user dùng thử để xem họ có thấy giá trị không |
| Cách đó có cần build gì tương đối lớn không? | **Có, rất lớn**: cần hoàn chỉnh cả stack (3 apps: web, API, AI service), data pipeline cho 171 địa điểm, RAG retrieval, chat UI + itinerary panel, hỗ trợ song ngữ |
| Cách đó tốn gì? (time / scope / dependency / design / code / data / people) | **Time**: 2 sprints (2 tuần) cho MVP. **Code**: ~3 apps riêng. **Data**: crawl + chunk + embed 171+ địa điểm. **Dependency**: Supabase, Gemini API, pgvector. **People**: cần full-stack + AI engineer |
| Vì sao cách hiện tại đang đắt hoặc chậm? | Build quá nhiều thứ trước khi biết pain có thật không. Nếu hypothesis sai, toàn bộ effort (2 tuần + 3 apps + data pipeline) là lãng phí. Cần infrastructure setup, integration testing, data quality — tất cả chỉ để trả lời 1 câu hỏi "có đau không?" |
| Câu hỏi bạn muốn tự ép mình hỏi lại là gì? | **"Có cách nào biết được pain này thật sự đủ lớn mà không cần build cả web app + AI + RAG trước không?"** |

---

## Bước 3 — Nghĩ Ra Ít Nhất 3 Cách Rẻ Hơn Để Test Cùng Hypothesis Đó

### Cách test A — Fake-door Landing Page

| Câu hỏi | Điền |
|---------|------|
| Tên hướng test A | **Fake-door: landing page + waitlist** |
| Loại artifact | Landing page (HTML tĩnh hoặc Carrd) + fake CTA |
| Người dùng sẽ thấy gì? | Một landing page giới thiệu TraVy: "Tạo lịch trình du lịch Việt Nam cá nhân hóa trong 5 phút — miễn phí" + input box để nhập điểm đến/ngày đi → click "Tạo lịch trình" → thấy "Sắp ra mắt! Để lại email để được dùng thử sớm" |
| Phía sau bạn sẽ làm gì? | Không có AI thật, không có RAG, không có backend. Chỉ tracking số người click CTA + số email submit |
| Vì sao cách này rẻ hơn current approach? | **Rẻ hơn rất nhiều**: 1 trang HTML tĩnh + form thu email = 2-3 giờ làm. Không cần code backend, AI service, data pipeline, database. Không cần deploy phức tạp (GitHub Pages / Netlify). Chi phí ≈ 0 đồng |
| Nó giúp học được gì? | **Signal về demand**: bao nhiêu người sẵn sàng để lại email = họ quan tâm đến giải pháp. Nếu chỉ 1-2% click → pain không đủ lớn hoặc value prop không hấp dẫn → build lại hypothesis trước khi invest |
| Nó chưa giúp học được gì? | Không test được: quality của lịch trình, trust vào AI, UX chat/hội thoại, willingness to pay. Chỉ test được interest ở mức "nghe có vẻ hay, để lại email thử" — hành vi rất nhẹ, chưa phải commitment thật |
| Artifact A sẽ nằm ở đâu? | `artifacts/fake-door-landing-page.html` (HTML tĩnh trong repo) |

### Cách test B — Wizard-of-Oz Interview

| Câu hỏi | Điền |
|---------|------|
| Tên hướng test B | **Wizard-of-Oz: giả AI + phỏng vấn user** |
| Loại artifact | Storyboard + WoZ flow script + sample output |
| Người dùng sẽ thấy gì? | User mở một giao diện chat đơn giản (Google Form hoặc chat page giả), nhập "Tôi muốn đi Hội An 3 ngày 2 đêm với bạn bè, thích ẩm thực và chụp ảnh", nhận được lịch trình từng ngày có kèm tên địa điểm, giờ mở cửa, gợi ý ăn uống — như thể hệ thống đang chạy thật |
| Phía sau bạn sẽ làm gì? | Bạn nhận request, thủ công tra cứu từ data có sẵn (171 địa điểm) + ChatGPT hỗ trợ + Google Maps check khoảng cách, rồi viết lịch trình tay + paste vào giao diện cho user. Mất 15-20 phút mỗi response — nhưng user nghĩ là real-time |
| Vì sao cách này rẻ hơn current approach? | **Không cần build AI pipeline**: không cần code RAG, không cần fine-tune, không cần deploy agent. Chỉ cần 1 giao diện tĩnh (Google Form hoặc HTML) + bạn làm backend người. Chi phí ≈ 5-6 giờ cho 5-10 user interviews |
| Nó giúp học được gì? | **Signal mạnh về value + trust**: user có thật sự thấy lịch trình hữu ích không? Họ có hỏi "làm sao mày biết chỗ này?" (trust) không? Họ có muốn sửa / customize không? Họ có sẵn sàng dùng lại không? — insight qualitative rất giàu |
| Nó chưa giúp học được gì? | Không test được: real-time response (vì bạn làm chậm hơn AI thật), scalability (chỉ làm tay được vài user), và không biết user có chịu đợi AI response 15-20s trong thực tế không |
| Artifact B sẽ nằm ở đâu? | `artifacts/woz-flow-script.md` + sample output screenshots |

### Cách test C — Community Engagement + Manual Demo

| Câu hỏi | Điền |
|---------|------|
| Tên hướng test C | **Community engagement: đăng mẫu + mời gửi nhu cầu** |
| Loại artifact | Before/after flow + 1 slide test setup + Google Form |
| Người dùng sẽ thấy gì? | Trong group Facebook du lịch / Reddit r/VietnamTravel, bạn đăng 1 bài: "Mình đang thử làm tool tạo lịch trình du lịch Việt Nam tự động — đây là mẫu output cho Hội An 3 ngày [kèm ảnh]. Ai đi tỉnh nào để lại nhu cầu, mình làm thử cho xem sao." User để lại comment hoặc điền form → bạn trả lời bằng lịch trình tự làm trong comment |
| Phía sau bạn sẽ làm gì? | Chuẩn bị sẵn 2-3 mẫu lịch trình đẹp (Hội An, Đà Lạt, Hà Nội) = data có sẵn + design tay. Khi user gửi nhu cầu, bạn thủ công làm thêm 3-5 cái bằng data + ChatGPT + Google Maps. Track số lượng comment, reaction, và tỉ lệ user quay lại hỏi thêm |
| Vì sao cách này rẻ hơn current approach? | **Rẻ nhất trong 3 cách**: không cần giao diện, không cần gặp mặt, chỉ cần bài đăng + ảnh mẫu + 3-5h manual work. Tiếp cận được nhiều user thật trong môi trường tự nhiên (không phải phỏng vấn setup) |
| Nó giúp học được gì? | **Signal về organic interest**: user tự động comment, share, hỏi thêm = demand có thật không cần prompting. Có thể thấy được pain language (user nói gì về việc tự research) để refine value prop |
| Nó chưa giúp học được gì? | Không test được: trust vào AI (vì họ biết bạn làm tay), UX (không có product interaction), willingness to pay. Sample bias: chỉ những người thích comment mới phản hồi |
| Artifact C sẽ nằm ở đâu? | `artifacts/community-demo-board.html` — board HTML tổng hợp sample outputs + form + test setup |

---

## Bước 4 — Làm Artifact Cho Cả 3 Cách

| Cách | Đã có artifact chưa? | Artifact là gì? | Link / ảnh / frame |
|------|---------------------|-----------------|-------------------|
| A | ✅ | Fake-door landing page (HTML) | [artifacts/fake-door-landing-page.html](./artifacts/fake-door-landing-page.html) |
| B | ✅ | WoZ flow script + sample output (Markdown) | [artifacts/woz-flow-script.md](./artifacts/woz-flow-script.md) |
| C | ✅ | Community demo board + test setup (HTML) | [artifacts/community-demo-board.html](./artifacts/community-demo-board.html) |

---

## Bước 5 — So Sánh Nhanh 3 Cách

| Tiêu chí | Cách A — Fake-door | Cách B — Wizard-of-Oz | Cách C — Community |
|----------|-------------------|----------------------|-------------------|
| Nhanh hơn current approach ở đâu? | **Trong 2-3 giờ** đã có thể deploy landing page, bắt đầu thu tín hiệu ngay | **Trong 1 tuần** có thể phỏng vấn 5-10 user — nhanh hơn build full MVP 2 tuần | **Trong 1 ngày**: chuẩn bị mẫu + đăng bài, bắt đầu thu tín hiệu ngay |
| Rẻ hơn current approach ở đâu? | **Gần như 0 đồng**: 1 file HTML tĩnh + GitHub Pages (miễn phí) | **~5-6 giờ manual work**: không cần code backend/AI. Chỉ tốn thời gian interview | **~3-4 giờ manual work**: không cần gặp mặt, không cần deploy |
| Gần với hành vi thật tới đâu? | **Trung bình**: user chỉ để lại email — hành vi nhẹ, chưa phải commitment thật | **Cao nhất**: user trải nghiệm sản phẩm giả định như thật, phản ứng qualitative rất giàu | **Trung bình**: user comment trong group — tự nhiên nhưng không có trải nghiệm sản phẩm |
| Học được điều gì rõ nhất? | **Demand signal**: có bao nhiêu người quan tâm đến value prop | **Value + trust**: user có thấy lịch trình hữu ích không? Có tin không? Có muốn dùng lại không? | **Organic interest + pain language**: user tự động hỏi, comment = demand thật không cần prompt |
| Giới hạn lớn nhất là gì? | Không test được chất lượng sản phẩm, trust, UX. Chỉ test interest ở mức rất nhẹ | Không scalable (chỉ vài user), response chậm (bạn làm tay) — user có thể nhận ra | Sample bias (chỉ người thích comment), không test được trust/Sản phẩm interaction |

**Trong 3 cách này, cách nào nhìn thuyết phục nhất khi present?**
<br>Cách B (Wizard-of-Oz) — vì nó cho qualitative insight sâu nhất về user reaction, trust, và value perception. Có thể trích dẫn lời user thật.

**Trong 3 cách này, cách nào dễ bị lớp phản biện nhất?**
<br>Cách A (Fake-door) — dễ bị hỏi "Email sign-up không có nghĩa là họ sẽ dùng thật", "Conversion rate bao nhiêu thì đủ?", "Chưa test được trust"

---

## Bước 6 — Gom Lại Thành Present Board Cá Nhân

| Câu hỏi | Điền |
|---------|------|
| Bạn đã gom đủ 5 khối lên board chưa? (Rồi / Chưa) | 🔲 Chưa — cần dựng board trên FigJam / Google Slides |
| Ảnh / screen / artifact nào quan trọng nhất khi present? | Sample output của WoZ (cách B) và landing page screenshot (cách A) |
| Chỗ nào trên board đang bị dài chữ quá? | Bảng so sánh 3 cách (Bước 5) — có thể rút gọn thành 3 bullet mỗi cách khi present |

---

## Bước 7 — Present Cá Nhân

**Link / frame present:** https://github.com/DuongCoder04/Day17-967-NguyenVanDuong — present trực tiếp từ README.md + artifacts trong repo

| Câu hỏi | Điền |
|---------|------|
| Bạn sẽ mở link / frame nào khi present? | FigJam board (5 khối) hoặc trực tiếp README.md + artifacts |
| Bạn muốn lớp phản biện mạnh nhất vào chỗ nào? | **Vào hypothesis và cách test**: "Pain này có thật không? Nếu user thích tự research thì sao? Cách test B (WoZ) có đủ để kết luận không?" |
| Nếu chỉ có 30 giây để mở đầu, bạn sẽ nói câu gì? | **"Mình đang làm TraVy — tool tạo lịch trình du lịch Việt Nam. Hypothesis nguy hiểm nhất: không biết pain research 4-10 tiếng có thật đủ lớn để user đổi tool không. Thay vì build full MVP 2 tuần, mình đề xuất 3 cách test rẻ hơn."** |

**Câu hỏi bạn muốn lớp phản biện:**
1. **"Theo bạn, nếu du khách thích tự research (coi nó là niềm vui), thì TraVy nên pivot hướng nào?"**
2. **"Cách WoZ (B) cho insight qualitative mạnh, nhưng liệu kết luận từ 5-10 user có generalize được không? Cần bao nhiêu user để đủ tin?"**

---

## Bước 8 — Ghi Lại Sau Present

| Câu hỏi | Điền |
|---------|------|
| Phản biện quan trọng nhất bạn vừa nhận là gì? | **(1)** Hypothesis chưa phân biệt "annoyance pain" vs "must-solve pain" — user có sẵn sàng trả tiền / đổi thói quen, hay chỉ thấy hơi phiền? **(2)** Cả 3 cách đều đo *intent* chứ không đo *retention* — click landing page, nói "ố hay" trong interview, react trên Facebook đều có thể là vanity signal. **(3)** Thiếu cách test "bỏ đói": cho user tự làm itinerary trên Excel rồi phỏng vấn ngay — quan sát friction thực tế, không cần build gì. |
| Trong 3 cách test, cách nào bị hỏi nhiều nhất? | **Cách A (Fake-door)** — bị phản biện nhiều nhất: (1) email sign-up là hành vi rất nhẹ, dễ nhiễu bởi tò mò nhất thời, gap "nói-làm" còn lớn hơn survey. (2) Bot traffic / click hợp lệ khó phân biệt nếu dùng static HTML thuần. (3) Không test được retention chút nào. |
| Bạn nhận ra cần chỉnh gì trên board / artifact? | (1) Thêm **cách test D — "bỏ đói" (deprivation test)**: cho user tự research thật rồi phỏng vấn ngay sau, 0 đồng, insight friction thực tế. (2) Cần phân biệt rõ trên board: cách nào đo *intent*, cách nào đo *behavior*, cách nào đo *retention*. (3) WoZ script cần thêm bước debrief để phát hiện leading bias từ researcher. |
| Điều gì bạn muốn giữ nguyên sau phản biện? | (1) **Cách B (WoZ) vẫn là mạnh nhất** — cho qualitative insight sâu nhất. (2) **Cấu trúc 3 cách test khác nhau** (demand / value / organic interest) là điểm mạnh, không nên làm 3 cách cùng loại. (3) **Hypothesis vẫn đúng** — pain 4-10h là thật, nhưng cần test mức độ ("đau cỡ nào?" thay vì "có đau không?"). |
