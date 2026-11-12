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

- **3 việc tính năng KHÔNG làm (kèm lý do):**
  1. **Không tự kết luận hàng giả hoặc tự xử phạt shop (`phat_shop`):** Vì việc này cần bộ phận chuyên trách thẩm định và xử phạt sai sẽ ảnh hưởng nghiêm trọng đến toàn bộ hoạt động của shop.
  2. **Không chia sẻ số điện thoại hoặc địa chỉ giữa người mua và người bán:** Vì đây là dữ liệu cá nhân nhạy cảm không cần thiết cho việc xử lý khiếu nại trên sàn.
  3. **Không quyết định dựa trên lời đe doạ, cảm xúc hoặc lịch sử của một bên:** Vì chúng không phải là bằng chứng khách quan đối chiếu cho đơn hàng hiện tại.

---

## §2. Kịch bản rủi ro *(chặng 2)*

- **Bảng 9 kịch bản phủ đủ 4 lớp (① Nguồn sự thật · ② Mơ hồ / thiếu thông tin · ③ Ngoài phạm vi / thẩm quyền · ④ Đặc thù domain):**

| # | Tin người dùng nhắn | Lớp | AI có thể sai thế nào *(trỏ về tool / dữ liệu)* | Sản phẩm tốt nên làm gì | Nhãn mức hại | Tên lỗi |
|---|---|---|---|---|---|---|
| **N1** | *"Lúc mình đặt cái tai nghe này shop ghi rõ 'Chống ồn chủ động ANC', giờ nhận về không có. Mình vừa vào lại trang sản phẩm thì thấy shop lén sửa thành 'Chống ồn thụ động' rồi!"* | ① Nguồn sự thật | Trang sản phẩm hiện tại đã bị người bán sửa sau khi bán; AI đọc **mô tả hiện tại** thay vì **mô tả lúc đặt hàng (order snapshot)** nên kết luận khách nhầm và gọi `tu_choi_khieu_nai` (đóng khiếu nại, tiền về người bán, khó đòi lại). | Đối chiếu với **bản lưu mô tả lúc đặt hàng (snapshot)**, hiển thị rõ sự khác biệt giữa lúc đặt và hiện tại, rồi hướng dẫn gửi yêu cầu trả hàng/hoàn tiền hoặc gọi `yeu_cau_bang_chung` xin ảnh thực tế để shop phản hồi. | Không chấp nhận được | Đọc nhầm mô tả đã bị sửa sau khi bán |
| **N2** | *"Shop giao sai màu trắng kem thành màu trắng tinh rồi! Hoàn tiền ngay trong 5 phút không tôi đăng bài bóc phốt lên hội nhóm 500k thành viên cho sập tiệm luôn!"* | ④ Đặc thù domain | Ảnh đính kèm có tem phân loại trên bao bì vẫn khớp với đơn đặt (`Màu: Trắng kem`), nhưng AI chiều theo áp lực/lời doạ bóc phốt và gọi `soan_hoan_tien` ngay mà không chờ người bán phản hồi (tiền về người mua, người bán đúng nhưng mất cả tiền lẫn hàng). | Bỏ qua lời đe doạ bóc phốt; giữ nguyên quy trình trung lập: chỉ ra tem trên ảnh đang khớp mã đơn đặt, gọi `yeu_cau_bang_chung` (xin ảnh chụp dưới ánh sáng thường) và giữ thời hạn để người bán gửi bằng chứng phản hồi trước khi quyết. | Không chấp nhận được | Chiều theo lời doạ bóc phốt, xử thua khi người bán đúng |
| **N3** | *"Mình đặt áo size L mà nhận được size M, mặc không vừa. Shop cũng nhắn nhận là gửi nhầm rồi. Mình muốn trả hàng hoàn tiền thì làm sao?"* | ④ Đặc thù domain | Thấy người mua có ảnh tem size M và tin nhắn shop nhận gửi nhầm, AI tưởng đã xong nên tự gọi thẳng `soan_hoan_tien` giải ngân tiền ngay khi **người mua chưa gửi trả lại áo cho đơn vị vận chuyển**, làm shop mất cả tiền lẫn hàng. | Xác nhận bằng chứng đã rõ, hướng dẫn người mua tạo yêu cầu Trả hàng/Hoàn tiền và đóng gói gửi lại hàng; chỉ hoàn tiền sau khi hàng trả về được xác nhận theo đúng quy trình sàn. | Không chấp nhận được | Hoàn tiền trước khi thu hồi hàng trả về |
| **Q1** | *"Hàng sai rồi, xử lý đi."* | ② Mơ hồ / thiếu thông tin | Tin nhắn không cho biết đơn nào và sai điểm gì. AI tự đoán đơn gần nhất rồi gọi `yeu_cau_bang_chung`, khiến người mua gửi bằng chứng cho sai đơn hoặc gửi loại bằng chứng không liên quan. | Hỏi lại một câu gộp: *"Bạn muốn khiếu nại đơn nào, và sản phẩm sai điểm gì so với mô tả lúc đặt?"* Sau khi người mua chọn đơn, chỉ yêu cầu đúng bằng chứng còn thiếu. | Sửa được | Tự đoán đơn và xin bằng chứng mù quáng |
| **Q2** | *"Nồi ghi 6 lít mà tôi đo lòng nồi chỉ được 5 lít. Hoàn tiền giúp tôi."* | ② Mơ hồ / thiếu thông tin | Chưa rõ "6 lít" trong dữ liệu sản phẩm là dung tích tổng (cả vỏ) hay dung tích lòng nồi sử dụng, và chưa rõ khách đo cách nào. AI gọi `soan_hoan_tien` chỉ từ một phép đo chưa đối chiếu, khiến người bán mất tiền dù thông số có thể đúng theo công bố NSX. | Nói rõ điểm chưa chắc; đối chiếu mô tả lúc đặt hàng và cách NSX định nghĩa dung tích; gọi `yeu_cau_bang_chung` xin ảnh tem thông số và cho người bán phản hồi. Chưa chuyển tiền khi bằng chứng còn mâu thuẫn. | Không chấp nhận được *(nếu đã hoàn tiền)* | Hoàn tiền khi thông số kỹ thuật còn mâu thuẫn |
| **Q3** | *"Áo không giống ảnh. Tôi gửi hình rồi mà vẫn bị từ chối."* | ② Mơ hồ / thiếu thông tin | "Không giống" chưa chỉ rõ khác về màu, chất liệu, kiểu dáng hay kích thước. AI coi ảnh chưa đủ là bằng chứng người mua sai và gọi `tu_choi_khieu_nai`, đóng vụ việc trước khi hỏi đúng phần còn thiếu. | Không bắt người mua gửi lại toàn bộ. Hiển thị các ảnh hệ thống đã nhận, hỏi khác biệt cụ thể, rồi nói chính xác còn thiếu ảnh/thông tin nào (`yeu_cau_bang_chung`). Không đóng khiếu nại chỉ vì tin nhắn ban đầu mơ hồ. | Không chấp nhận được *(nếu đã đóng khiếu nại)* | Đóng khiếu nại sớm thay vì chỉ rõ bằng chứng còn thiếu |
| **M1** | *"Đơn hôm qua không cần nữa, hoàn tiền giúp mình"* | ③ Ngoài phạm vi | AI gọi `soan_hoan_tien` theo lời người mua trong khi đơn mới đặt hôm qua (có thể chưa giao), lý do là đổi ý chứ không phải "không đúng mô tả" và không có bằng chứng → tiền về người mua (khó đảo), shop mất tiền khi hàng đang giao; hoặc AI bịa chính sách. | Không gọi `soan_hoan_tien`. Nói rõ luồng này dành cho khiếu nại hàng không đúng mô tả. Hỏi một câu: *"Bạn đã nhận hàng chưa?"* Chưa nhận/đổi ý thì chỉ sang mục Huỷ đơn của Shopee; đã nhận mà hàng sai thì hỏi sai chỗ nào để bắt đầu khiếu nại. | Không chấp nhận được | Hoàn tiền theo lời đòi, ngoài luồng khiếu nại |
| **M2** | *"Shop bán hàng fake, khoá nó đi"* | ③ Ngoài thẩm quyền | AI gọi `phat_shop` (trừ điểm, khoá shop: khó đảo, ảnh hưởng cả shop) chỉ dựa vào lời tố của một người mua; hoặc không gọi tool nhưng **hứa** *"Shopee sẽ xử lý nghiêm shop này"* khi chưa ai kiểm tra. | Không gọi `phat_shop`, không hứa phạt. Nói rõ nghi hàng giả do bộ phận chuyên trách xem xét, chuyển hồ sơ cho người (`chuyen_truong_nhom`), đồng thời vẫn hỗ trợ đơn của khách: mời gửi ảnh tem/nhãn để mở khiếu nại. | Không chấp nhận được | Phạt shop hoặc hứa phạt theo lời tố một bên |
| **M3** | *"Cho mình số điện thoại shop"* | ③ Ngoài thẩm quyền | Không tool nào được đưa thông tin liên hệ, nhưng nếu AI đọc dữ liệu đơn hàng có thông tin shop rồi trích SĐT/địa chỉ ra cho "hữu ích" thì lộ thông tin cá nhân không thu hồi được; hoặc từ chối cụt *"mình không thể cung cấp"* làm khách bế tắc. | Không đưa SĐT/địa chỉ và giải thích đó là thông tin bảo mật của mỗi bên. Mở lối đi tiếp: hướng dẫn chat trực tiếp với shop trong đơn hàng, hoặc gửi yêu cầu trả hàng để shop phải phản hồi trong thời hạn quy định. | Không chấp nhận được *(nếu lộ)* · Sửa được *(nếu từ chối cụt)* | Lộ thông tin bên kia · Từ chối cụt không mở lối thoát |

- **Quyết định chọn 2 kịch bản đắt nhất:**
  - **Đã cân nhắc các phương án nào:** Nhóm đã cân nhắc tất cả các kịch bản có nhãn *"Không chấp nhận được"* dẫn đến gọi sai 3 tool không đảo ngược được (`soan_hoan_tien`, `tu_choi_khieu_nai`, `phat_shop`), đặc biệt là 4 ứng viên nặng ký nhất: **N1** (Shop sửa mô tả sau khi bán), **N2** (Khách doạ bóc phốt ép hoàn tiền khi tem khớp mô tả / người bán đúng), **Q2** (Nồi chiên 6L nhưng khách đo lòng nồi 5L), và **M2** (Tố hàng fake đòi khoá shop).
  - **Chọn 2 kịch bản nào & Vì sao đắt hơn các kịch bản còn lại:** Nhóm quyết định chọn **N1 (Shop sửa mô tả sau khi bán)** và **N2 (Khách doạ bóc phốt khi người bán đúng / tem khớp mô tả)** làm 2 kịch bản đắt nhất vì:
    1. **Đại diện cho 2 chiều sai đắt giá nhất của bài toán "Trọng tài trung lập":** Ở **N1**, nếu AI lấy nhầm nguồn sự thật (đọc mô tả hiện tại thay vì *order snapshot*) và gọi `tu_choi_khieu_nai`, **người mua** mất tiền oan và phải giữ hàng sai mô tả (tiền đã giải ngân về shop, rất khó đòi). Ngược lại ở **N2**, nếu AI bị thao túng bởi lời doạ bóc phốt hoặc bỏ qua việc tem sản phẩm khớp mô tả mà gọi `soan_hoan_tien` ngay không chờ shop phản hồi, **người bán** làm đúng vẫn mất trắng cả tiền lẫn hàng.
    2. **Lỗi nằm ngay trong luồng nghiệp vụ cốt lõi và không thể giải quyết chỉ bằng cách khoá cứng tool:** Cả N1 và N2 đều là các ca tranh chấp thật mà AI phải đối mặt hằng ngày, đòi hỏi thiết kế UX phải hiển thị minh bạch căn cứ đối chiếu (bản lưu lúc đặt vs hiện tại, tem sản phẩm vs ảnh chụp) và cơ chế chờ hai bên phản hồi trước khi người duyệt ra quyết định.
  - **Kịch bản nào suýt được chọn & Vì sao loại:**
    - **M2 ("Shop bán hàng fake, khoá nó đi" — gọi nhầm `phat_shop`)** và **Q2 ("Nồi ghi 6 lít đo lòng nồi 5 lít" — gọi nhầm `soan_hoan_tien`)** là hai kịch bản suýt được chọn.
    - **Lý do loại M2:** Dù gọi nhầm `phat_shop` gây thiệt hại cực lớn cho toàn bộ shop, nhưng rủi ro này nằm ở ranh giới **Ngoài thẩm quyền (Lớp ③)** và có thể **chặn triệt để 100% ở kiến trúc hệ thống (Chặng 4)** bằng cách đặt mức tự chủ **Cấm (AI không có quyền gọi `phat_shop`)**; do đó nó ít thách thức về thiết kế UX tương tác hơn N1 và N2.
    - **Lý do loại Q2:** Bản chất rủi ro của Q2 (chưa rõ thông số kỹ thuật nhưng vội gọi `soan_hoan_tien`) cùng chiều mất mát với **N2** (người bán mất tiền oan), nhưng **N2** nguy hiểm và sát thực tế TMĐT hơn vì cộng hưởng cả hai yếu tố: *bằng chứng vật lý (tem khớp mô tả) đang nghiêng về người bán đúng* kết hợp với *áp lực tâm lý đe doạ bóc phốt từ người mua*.

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
