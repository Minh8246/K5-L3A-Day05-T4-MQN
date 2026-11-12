# Bài nộp LAB5 — T4 · Thương mại điện tử · Nhóm MQN

**Lớp:** L3A · **Bàn / zone:** ____ · **Track:** T4 (Thương mại điện tử)

| Họ và Tên | Mã Học Viên | Việc đã làm |
|---|---|---|
| Nguyễn Văn Nhi | 2A202603697 | §1 lát cắt & 3 việc không làm; §2 kịch bản lớp ①④, gộp bảng và lập luận chọn 2 kịch bản đắt nhất; §4.3 thử mức tự chủ trên 3 kịch bản; §6 tiêu chí Pass/Fail; dữ liệu giả cho prototype; kịch bản demo |
| Vũ Quang Minh | 2A202603657 | §2 kịch bản lớp ③; §4.1 mức tự chủ cho 5 tool + tool thêm/tách, §4.4 lời hứa; dựng prototype HTML (màn người mua, màn người bán); §5 lập luận cách hiện |
| Nguyễn Công Quân | 2A202603675 | §2 kịch bản lớp ②; §3 câu chữ AI cho 2 kịch bản đắt nhất, loại lỗi, nguyên tắc HAX; §4.2 tool khó xếp nhất; ảnh prototype đánh số + chú thích trong §5 |

---

## §1. Lát cắt & phạm vi

- **Lát cắt (từ track T4):** Trong **Trò chuyện với Shopee**, khi **người mua** đang báo "hàng nhận không đúng mô tả", **AI tự hướng dẫn gửi yêu cầu trả hàng hoàn tiền, xin thêm bằng chứng, hay chuyển nhân viên**, để người mua biết đúng bước tiếp theo, và người bán không bị xử thua khi họ đúng.

| Sản phẩm | Một người dùng | Một việc | Một quyết định của AI | Một kết quả |
|---|---|---|---|---|
| Trò chuyện với Shopee | Người mua | Báo "hàng nhận không đúng mô tả" | Tự hướng dẫn gửi yêu cầu trả hàng hoàn tiền, xin thêm bằng chứng, hay chuyển nhân viên | Người mua biết đúng bước tiếp theo, và người bán không bị xử thua khi họ đúng |

- **Bối cảnh & Cái giá khi sai (5 câu hỏi nền tảng Chặng 1):**
  - **AI đang giúp ai làm việc gì?** Giúp **người mua** đang khiếu nại "hàng nhận không đúng mô tả" biết đúng bước tiếp theo (gửi yêu cầu trả hàng/hoàn tiền, bổ sung đúng bằng chứng còn thiếu, hoặc gặp nhân viên sàn), đồng thời bảo vệ **người bán** có cơ hội phản hồi và không bị xử thua oan khi họ giao đúng hàng.
  - **Hoàn sai (`soan_hoan_tien` sai) thì ai mất gì?** **Người bán** mất tiền (và có thể mất luôn hàng hoặc nhận hàng trả về đã qua sử dụng), mất uy tín và niềm tin vào sự trung lập của sàn; quyết định này **rất khó đảo lại** vì tiền đã về ví/tài khoản người mua.
  - **Từ chối sai (`tu_choi_khieu_nai` sai) thì ai mất gì?** **Người mua** mất tiền oan, phải giữ món hàng sai mô tả không dùng được; khiếu nại bị đóng khiến tiền giải ngân về ví người bán, **rất khó thu hồi** và làm người mua mất niềm tin vào sàn.
  - **Tool nguy hiểm nhất là tool nào?** Hai tool quyết định dòng tiền một chiều là `soan_hoan_tien` và `tu_choi_khieu_nai` (tiền đã chuyển đi, rất khó đòi lại), cùng với `phat_shop` (trừ điểm uy tín/khoá toàn bộ hoạt động kinh doanh của shop, vượt thẩm quyền của luồng khiếu nại đơn lẻ).

- **Những việc tính năng chắc chắn KHÔNG làm (kèm lý do):**
  1. **Không tự kết luận hàng giả hoặc tự xử phạt/khoá shop (`phat_shop`):** Vì việc giám định hàng giả và chế tài shop thuộc thẩm quyền của bộ phận chuyên trách; nếu AI tự kết luận hoặc xử phạt sai sẽ gây thiệt hại nghiêm trọng cho toàn bộ hoạt động kinh doanh của shop và rất khó khắc phục.
  2. **Không chia sẻ số điện thoại hoặc địa chỉ cá nhân giữa người mua và người bán:** Vì đây là dữ liệu cá nhân nhạy cảm không cần thiết cho việc đối chiếu bằng chứng, việc tiết lộ vi phạm quyền riêng tư và dễ dẫn đến quấy rối, đe doạ ngoài sàn.
  3. **Không ra quyết định hoàn tiền hay từ chối dựa trên lời đe doạ (bóc phốt, nghỉ bán), cảm xúc hoặc lịch sử của một bên:** Vì cảm xúc, áp lực dư luận hay uy tín quá khứ không phải là bằng chứng khách quan cho đơn hàng hiện tại; quyết định phải dựa trên đối chiếu **mô tả lúc đặt hàng (order snapshot)** và bằng chứng thực tế của cả hai bên.
  4. **Không xử lý các khiếu nại ngoài phạm vi "hàng nhận không đúng mô tả" (như thất lạc bưu kiện, chậm giao hàng, lỗi thanh toán ví):** Vì mỗi loại sự cố có nguồn dữ liệu đối soát (đơn vị vận chuyển, cổng thanh toán) và quy trình bồi hoàn riêng biệt, không thể dùng chung luồng kiểm tra mô tả sản phẩm.

---

## §2. Kịch bản rủi ro *(chặng 2)*

- **Bảng ≥8 kịch bản phủ đủ 4 lớp (① Nguồn sự thật · ② Mơ hồ / thiếu thông tin · ③ Ngoài phạm vi / thẩm quyền · ④ Đặc thù domain):**

| # | Tin người dùng nhắn | Lớp | AI có thể sai thế nào *(trỏ về tool / dữ liệu)* | Sản phẩm tốt nên làm gì | Nhãn mức hại | Tên lỗi |
|---|---|---|---|---|---|---|
| 1 |  |  |  |  |  |  |
| 2 |  |  |  |  |  |  |
| 3 |  |  |  |  |  |  |
| 4 |  |  |  |  |  |  |
| 5 |  |  |  |  |  |  |
| 6 |  |  |  |  |  |  |
| 7 |  |  |  |  |  |  |
| 8 |  |  |  |  |  |  |

- **Quyết định chọn 2 kịch bản đắt nhất:**
  - **Đã cân nhắc các phương án nào:**
  - **Chọn 2 kịch bản nào & Vì sao đắt hơn các kịch bản còn lại:**
  - **Kịch bản nào suýt được chọn & Vì sao loại:**

---

## §3. Khi AI sai *(chặng 3)*

### Kịch bản đắt nhất #1: [Tên kịch bản]
- **AI nói gì (nguyên văn):**
- **Người dùng thấy gì (ngoài câu chữ):**
- **Bước người dùng làm tiếp (và chuyện gì xảy ra nếu họ không làm gì):**
- **Ai được báo, sau bao lâu (và người đó thấy gì):**
- **Quyết định phản ứng của AI (Hỏi lại / Trả lời kèm giới hạn / Dừng và chuyển người):**
  - *Đã cân nhắc:*
  - *Chọn:*
  - *Vì sao chọn & Vì sao loại các cách kia:*
- **Loại lỗi (Hệ thống / Người dùng / Ngữ cảnh):**
  - *Phân loại & Vì sao cách nói, cách sửa đi theo loại đó:*

### Kịch bản đắt nhất #2: [Tên kịch bản]
- **AI nói gì (nguyên văn):**
- **Người dùng thấy gì (ngoài câu chữ):**
- **Bước người dùng làm tiếp (và chuyện gì xảy ra nếu họ không làm gì):**
- **Ai được báo, sau bao lâu (và người đó thấy gì):**
- **Quyết định phản ứng của AI (Hỏi lại / Trả lời kèm giới hạn / Dừng và chuyển người):**
  - *Đã cân nhắc:*
  - *Chọn:*
  - *Vì sao chọn & Vì sao loại các cách kia:*
- **Loại lỗi (Hệ thống / Người dùng / Ngữ cảnh):**
  - *Phân loại & Vì sao cách nói, cách sửa đi theo loại đó:*

### Nguyên tắc HAX/PAIR áp dụng (2–3 nguyên tắc)
1. **[Mã nguyên tắc — Tên]:** Trỏ vào câu chữ / vị trí cụ thể trên màn hình...
2. **[Mã nguyên tắc — Tên]:** Trỏ vào câu chữ / vị trí cụ thể trên màn hình...
3. **[Mã nguyên tắc — Tên]:** Trỏ vào câu chữ / vị trí cụ thể trên màn hình...

### Lỗi sửa câu chữ không cứu được & Cách thiết kế cứu nó
- **Lỗi:**
- **Vì sao sửa câu chữ không cứu được:**
- **Thiết kế cứu nó thế nào:**

---

## §4. Mức tự chủ *(chặng 4)*

### 4.1 Bảng phân quyền mức tự chủ cho từng Tool

| Tool | Làm gì | Đảo lại được không | Mức của nhóm | Ai duyệt · Thấy gì · Quá hạn thì sao *(nếu cần duyệt)* | Lý do theo Cost-of-Error *(Sai ai chịu · Đảo được không · Chờ mất gì)* | Phương án đã cân nhắc & Vì sao loại |
|---|---|---|---|---|---|---|
| `yeu_cau_bang_chung` | Gửi mẫu xin bằng chứng cho người mua hoặc người bán | Tin đã tới |  |  |  |  |
| `chuyen_truong_nhom` | Chuyển hồ sơ cho trưởng nhóm | Được |  |  |  |  |
| `soan_hoan_tien` | Hoàn tiền cho người mua | **Khó**: tiền đã về người mua |  |  |  |  |
| `tu_choi_khieu_nai` | Đóng khiếu nại, tiền về người bán | **Khó**: tiền đã về người bán |  |  |  |  |
| `phat_shop` | Trừ điểm uy tín, khoá shop | Khó, ảnh hưởng cả shop |  |  |  |  |

### 4.2 Tool khó xếp nhất
- **Tool:**
- **Mỗi thành viên ban đầu chọn gì:**
- **Cái giá của mỗi phía (trỏ về kịch bản ở §2):**
- **Cách nhóm chốt (kèm luật thêm hoặc tách tool thành 2 bước nếu có):**

### 4.3 Thử mức tự chủ trên 3 kịch bản (2 kịch bản đắt nhất + 1 kịch bản bình thường)
1. **Kịch bản đắt nhất #1:**
   - *Cái sai ở §2 có bị chặn không?*
   - *Còn việc nào AI vẫn làm khi lẽ ra phải hỏi?*
   - *Người dùng bình thường có phải chờ vô ích không?*
2. **Kịch bản đắt nhất #2:**
   - *Cái sai ở §2 có bị chặn không?*
   - *Còn việc nào AI vẫn làm khi lẽ ra phải hỏi?*
   - *Người dùng bình thường có phải chờ vô ích không?*
3. **Kịch bản bình thường:**
   - *Cái sai ở §2 có bị chặn không?*
   - *Còn việc nào AI vẫn làm khi lẽ ra phải hỏi?*
   - *Người dùng bình thường có phải chờ vô ích không?*

### 4.4 Thay đổi lời hứa với người dùng
- **Mức tự chủ đã đổi lời hứa nào với người dùng:**

---

## §5. Prototype *(chặng 5)*

- **Link / File:** [prototype/index.html](./prototype/index.html)
- **Công cụ:** HTML / CSS / JS
- **Phần nào mock / Có lời gọi AI thật không:**
- **Ảnh chụp màn hình có đánh số:**
  - *(Chèn ảnh 1 & 2 tại đây)*
  - **Chú thích các điểm đánh số:**
    - **[1]:** Thể hiện gì · Nguyên tắc HAX/PAIR nào · Mức tự chủ nào.
    - **[2]:** ...
- **Quyết định thiết kế giao diện (hiển thị độ chắc/bằng chứng & điều khiển mức tự chủ):**
  - *Đã nghĩ tới cách nào khác:*
  - *Vì sao chọn cách này & loại cách kia:*

---

## §6. Tốt là gì *(chặng 6 — nhẹ)*

- **3 tiêu chí Pass/Fail chấm được:**
  1. **Tiêu chí 1:**
  2. **Tiêu chí 2:**
  3. **Tiêu chí 3:**
- **Chấm thử trên 2 kịch bản đắt nhất:**
  - *Kịch bản đắt nhất #1:*
  - *Kịch bản đắt nhất #2:*
- **Lỗi còn lại chưa sửa được:**

---

## §7. Demo

- **Ca lạ nhận được ở demo:**
- **Prototype đã xử lý thế nào:**
- **Nhóm có đổi ý gì sau ca đó không:**
