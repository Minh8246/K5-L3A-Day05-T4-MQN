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

### Kịch bản đắt nhất #1 — N1: Shop sửa mô tả sau khi bán

- **Tin người dùng:** *"Lúc mình đặt cái tai nghe này shop ghi rõ 'Chống ồn chủ động ANC', giờ nhận về không có. Mình vừa vào lại trang sản phẩm thì thấy shop lén sửa thành 'Chống ồn thụ động' rồi!"*

- **AI nói gì (nguyên văn):**

  > **Thông tin sản phẩm đã thay đổi sau khi bạn đặt hàng.** Bản mô tả được lưu lúc đặt ghi “Chống ồn chủ động ANC”, còn trang hiện tại ghi “Chống ồn thụ động”. Thay đổi này là một bằng chứng quan trọng, nhưng chưa đủ để xác nhận chiếc tai nghe bạn nhận không có ANC.
  >
  > Khiếu nại vẫn đang mở; hệ thống chưa hoàn tiền hoặc bác yêu cầu của bạn. Ảnh bạn đã gửi vẫn được giữ trong hồ sơ. Bạn chỉ cần bổ sung ảnh mã sản phẩm hoặc video thao tác kiểm tra chế độ ANC. Người bán cũng sẽ được xem cùng bộ bằng chứng và phản hồi.

- **Người dùng thấy gì (ngoài câu chữ):**
  1. Nhãn **“Thông tin sản phẩm đã thay đổi — chưa thể kết luận”**, không hiển thị phần trăm chắc chắn giả tạo.
  2. Thẻ so sánh song song: **Mô tả lúc đặt: “ANC”** và **Mô tả hiện tại: “Chống ồn thụ động”**, kèm thời điểm của mỗi bản.
  3. Danh sách bằng chứng đã nhận và đúng một mục còn thiếu: ảnh mã sản phẩm hoặc video thao tác kiểm tra chế độ ANC.
  4. Trạng thái xử lý: **“Chưa có quyết định hoàn tiền hoặc bác khiếu nại”**; hồ sơ vẫn mở.
  5. Các nút **“Bổ sung bằng chứng”** và **“Gặp nhân viên”**; không có nút tự động từ chối hay hoàn tiền.

- **Bước người dùng làm tiếp (và chuyện gì xảy ra nếu họ không làm gì):**
  - Người mua có thể bổ sung đúng bằng chứng còn thiếu hoặc chọn gặp nhân viên; không phải tải lại ảnh hệ thống đã nhận.
  - Nếu người mua chưa làm gì, hồ sơ vẫn ở trạng thái chờ trong thời hạn hiển thị trên màn hình và hệ thống gửi một lời nhắc trước hạn; không tự gọi `tu_choi_khieu_nai` chỉ vì người mua chưa phản hồi ngay.
  - Nếu không lấy được bản mô tả lúc đặt hoặc bằng chứng hai bên vẫn mâu thuẫn, hệ thống dừng quyết định và chuyển người xử lý.

- **Ai được báo, sau bao lâu (và người đó thấy gì):**
  - Người bán được báo **ngay sau khi người mua xác nhận gửi yêu cầu**, thấy bản mô tả lúc đặt, ảnh/video của người mua và đúng phần cần phản hồi; không thấy dữ liệu liên hệ riêng của người mua.
  - Nhân viên được báo ngay nếu hệ thống không lấy được bản mô tả lúc đặt, hai bên đưa bằng chứng mâu thuẫn, hoặc hết thời hạn phản hồi mà chưa đủ căn cứ. Nhân viên thấy toàn bộ lịch sử, hai phiên bản mô tả và bằng chứng đã có để người mua không phải kể lại.
  - Thời gian phản hồi giả định trong prototype là **24 giờ**; đây là số giả phục vụ bài lab, không phải chính sách Shopee thật.

- **Quyết định phản ứng của AI (Hỏi lại / Trả lời kèm giới hạn / Dừng và chuyển người):**
  - *Đã cân nhắc:* (1) hỏi lại người mua rồi tự quyết; (2) trả lời kèm giới hạn và tiếp tục thu bằng chứng hai phía; (3) dừng và chuyển nhân viên ngay.
  - *Chọn:* **Trả lời kèm giới hạn**, giữ khiếu nại mở và xin đúng bằng chứng còn thiếu; **dừng và chuyển người** chỉ khi không lấy được bản mô tả lúc đặt hoặc bằng chứng vẫn mâu thuẫn.
  - *Vì sao chọn:* Bản mô tả được lưu lúc đặt đã chứng minh nội dung thay đổi nhưng chưa tự chứng minh sản phẩm nhận được thiếu ANC. AI có thể làm phần rẻ và sửa được là trình bày nguồn, giữ hồ sơ và xin bằng chứng; quyết định hoàn tiền hoặc bác khiếu nại phải chờ đủ căn cứ.
  - *Vì sao loại hỏi lại rồi tự quyết:* Lời kể của người mua không thay thế được bản mô tả lúc đặt và phản hồi của người bán.
  - *Vì sao loại chuyển người ngay trong mọi ca:* Khi snapshot đọc được và người mua chỉ thiếu một bằng chứng cụ thể, chuyển ngay làm hàng chờ dài mà chưa tạo thêm giá trị. Tuy nhiên, chuyển người là đường lui bắt buộc khi nguồn sự thật bị thiếu hoặc xung đột không giải được.
  - *Vì sao không hoàn hoặc từ chối ngay:* Hoàn ngay có thể xử thua người bán khi tai nghe thật sự có ANC; từ chối ngay bằng mô tả hiện tại xử thua người mua vì shop đã sửa nội dung sau bán.

- **Loại lỗi (Hệ thống / Người dùng / Ngữ cảnh):**
  - *Phân loại:* **Lỗi hệ thống/nguồn sự thật** nếu AI đọc mô tả hiện tại thay vì bản mô tả được lưu lúc đặt.
  - *Vì sao cách nói đi theo loại lỗi:* AI phải nói rõ hai phiên bản dữ liệu và thừa nhận giới hạn của hệ thống, không đổ lỗi rằng người mua “nhớ nhầm”.
  - *Vì sao cách sửa đi theo loại lỗi:* Hệ thống phải bắt buộc lấy bản mô tả lúc đặt và chặn quyết định hoàn tiền hoặc bác khiếu nại khi bản này bị thiếu; yêu cầu người mua viết lại câu không thể sửa nguồn dữ liệu sai.

### Kịch bản đắt nhất #2 — N2: Khách doạ bóc phốt khi tem khớp đơn nhưng màu thực tế chưa rõ

- **Tin người dùng:** *"Shop giao sai màu trắng kem thành màu trắng tinh rồi! Hoàn tiền ngay trong 5 phút không tôi đăng bài bóc phốt lên hội nhóm 500k thành viên cho sập tiệm luôn!"*

- **AI nói gì (nguyên văn):**

  > Mình hiểu bạn muốn được xử lý nhanh. Yêu cầu này sẽ được xem xét dựa trên thông tin của đơn hàng và bằng chứng từ cả hai bên.
  >
  > Hiện tem phân loại trong ảnh ghi **“Trắng kem”**, khớp với màu bạn đã đặt; tuy nhiên màu trong ảnh có thể thay đổi theo ánh sáng nên **chưa đủ căn cứ kết luận người mua hay người bán sai**. Mình chưa hoàn tiền và cũng chưa đóng khiếu nại.
  >
  > Để kiểm tra đúng điểm còn thiếu, bạn vui lòng chụp sản phẩm dưới ánh sáng tự nhiên, đặt cạnh một vật màu trắng làm mốc. Người bán sẽ được xem cùng bộ bằng chứng và có 24 giờ để phản hồi.

- **Người dùng thấy gì (ngoài câu chữ):**
  1. Nhãn **“Chưa đủ căn cứ — cần đối chiếu màu”**.
  2. Thẻ căn cứ: màu đã đặt **Trắng kem**; tem nhận được **Trắng kem**; ảnh màu thực tế được đánh dấu **có thể thay đổi do ánh sáng**.
  3. Hướng dẫn minh hoạ cách chụp dưới ánh sáng tự nhiên và danh sách ảnh hệ thống đã nhận.
  4. Đồng hồ thời hạn phản hồi của người bán và trạng thái **“Chưa có quyết định hoàn tiền hoặc bác khiếu nại”**.
  5. Nút **“Chụp/bổ sung ảnh”**, **“Sửa thông tin”** và **“Gặp nhân viên”**; ở bước này không hiển thị nút **“Hoàn tiền ngay”** hoặc **“Phạt shop”**.

- **Bước người dùng làm tiếp (và chuyện gì xảy ra nếu họ không làm gì):**
  - Người mua bổ sung một ảnh đúng hướng dẫn thay vì gửi lại toàn bộ hồ sơ.
  - Nếu không bổ sung ngay, khiếu nại vẫn mở đến thời hạn hiển thị và có lời nhắc trước hạn; lời đe doạ không làm thay đổi thứ tự hay kết quả xử lý.
  - Sau khi hai bên phản hồi, nếu ảnh vẫn không đủ phân biệt “trắng kem” và “trắng tinh”, hồ sơ được chuyển nhân viên thay vì AI tự chọn bên thắng.

- **Ai được báo, sau bao lâu (và người đó thấy gì):**
  - Người bán được báo **ngay sau khi yêu cầu khiếu nại được gửi**, thấy ảnh sản phẩm, màu trong bản mô tả lúc đặt, tem phân loại và thời hạn phản hồi; không thấy lời nhắn riêng không liên quan hoặc dữ liệu cá nhân của người mua.
  - Nhân viên được báo khi bằng chứng hai bên còn mâu thuẫn hoặc quá hạn mà chưa đủ căn cứ. Họ thấy bảng đối chiếu và lịch sử yêu cầu bằng chứng, không chỉ thấy câu đe doạ của người mua.
  - Thời gian phản hồi giả định trong prototype là **24 giờ** để thể hiện luồng; số này không phải chính sách Shopee thật.

- **Quyết định phản ứng của AI (Hỏi lại / Trả lời kèm giới hạn / Dừng và chuyển người):**
  - *Đã cân nhắc:* (1) hoàn tiền ngay để xoa dịu; (2) từ chối ngay vì tem khớp đơn; (3) hỏi đúng bằng chứng còn thiếu rồi chờ hai bên; (4) chuyển nhân viên ngay.
  - *Chọn:* **Hỏi lại để thu hẹp điểm mơ hồ**, bằng một yêu cầu ảnh cụ thể; đồng thời trả lời kèm giới hạn rằng tem khớp đơn nhưng ảnh hiện tại chưa đủ kết luận.
  - *Vì sao chọn:* Màu sắc qua ảnh phụ thuộc ánh sáng. Một ảnh theo cùng điều kiện có thể tăng chất lượng bằng chứng với chi phí thấp, trong khi chưa ra quyết định hoàn tiền hoặc bác khiếu nại sẽ bảo vệ cả hai bên.
  - *Vì sao loại hoàn tiền ngay:* Lời doạ không phải bằng chứng; `soan_hoan_tien` sai khiến người bán đúng mất tiền và có thể mất cả hàng.
  - *Vì sao loại từ chối ngay:* Tem “Trắng kem” có thể đúng nhưng sản phẩm bên trong vẫn có màu khác; gọi `tu_choi_khieu_nai` lúc này có thể xử thua người mua.
  - *Vì sao loại chuyển người ngay:* Hệ thống còn có thể hỏi một câu/bằng chứng rõ ràng để thu hẹp vấn đề. Chỉ chuyển khi bước này không giải được mâu thuẫn hoặc sắp hết hạn.

- **Loại lỗi (Hệ thống / Người dùng / Ngữ cảnh):**
  - *Phân loại:* **Lỗi ngữ cảnh/cảm nhận**: người mua có thể thật sự nhìn thấy màu khác, nhưng ảnh chịu ảnh hưởng của ánh sáng và lời đe doạ tạo áp lực không liên quan đến bằng chứng.
  - *Vì sao cách nói đi theo loại lỗi:* AI công nhận sự sốt ruột nhưng không nói “bạn nói sai” hay tranh cãi về lời đe doạ; nó tách cảm xúc khỏi tiêu chí xét bằng chứng.
  - *Vì sao cách sửa đi theo loại lỗi:* Hướng dẫn chụp trong điều kiện so sánh được, cho cả hai bên phản hồi và chuyển người khi vẫn mơ hồ; không thể sửa bằng cách ưu tiên bên nói mạnh hơn.

### Nguyên tắc HAX/PAIR áp dụng (3 nguyên tắc)

1. **HAX G2 — Làm rõ AI làm tốt tới đâu:** Trên màn N1, nhãn **“Thông tin sản phẩm đã thay đổi — chưa thể kết luận”** nói chính xác điều AI chưa thể xác định. Trên màn N2, nhãn **“Chưa đủ căn cứ — cần đối chiếu màu”** chỉ ra giới hạn của ảnh hiện có; nhóm không dùng con số phần trăm tự tin thiếu căn cứ.
2. **HAX G10 — Thu hẹp khi nghi ngờ:** Ở N1, AI chỉ xin ảnh mã sản phẩm hoặc video thao tác kiểm tra ANC; ở N2, AI chỉ xin ảnh dưới ánh sáng tự nhiên cạnh vật trắng. Hai chỗ này nằm tại nút **“Bổ sung bằng chứng”**, thay vì bắt người dùng gửi lại mọi thứ hoặc đoán.
3. **HAX G11 + PAIR Explainability/Trust — Giải thích vì sao:** Thẻ đối chiếu trên N1 hiển thị **bản mô tả lúc đặt “ANC” ↔ mô tả hiện tại “thụ động”**; thẻ N2 hiển thị **màu đặt ↔ tem nhận ↔ ảnh thực tế**. Người dùng và nhân viên đều thấy cùng căn cứ trước khi có quyết định hoàn tiền hoặc bác khiếu nại.

### Lỗi sửa câu chữ không cứu được & Cách thiết kế cứu nó

- **Lỗi:** Hệ thống cho phép AI gọi `tu_choi_khieu_nai` bằng mô tả hiện tại ở N1, hoặc gọi `soan_hoan_tien` vì lời đe doạ ở N2, trước khi đối chiếu bản mô tả lúc đặt và bằng chứng hai bên.
- **Vì sao sửa câu chữ không cứu được:** Dù AI thêm câu “mình có thể nhầm” hoặc xin lỗi rất hay, tiền vẫn đã đi về một bên và hành động khó đảo ngược. Cảnh báo không thay thế được kiểm soát quyền hành động.
- **Thiết kế cứu nó thế nào:** (1) lưu và bắt buộc dùng bản mô tả lúc đặt; (2) hiển thị cùng một bảng bằng chứng cho người mua, người bán và người duyệt; (3) tách **AI soạn đề xuất** khỏi **người có thẩm quyền bấm duyệt hoàn tiền hoặc bác khiếu nại**; (4) khoá hai hành động này khi nguồn sự thật thiếu hoặc bằng chứng mâu thuẫn; (5) giữ khiếu nại mở và tự chuyển nhân viên khi quá hạn hoặc vẫn không đủ căn cứ.

---

## §4. Mức tự chủ *(chặng 4)*

### 4.1 Bảng phân quyền mức tự chủ cho từng Tool

| Tool | Làm gì | Đảo lại được không | Mức của nhóm | Ai duyệt · Thấy gì · Quá hạn thì sao *(nếu cần duyệt)* | Lý do theo Cost-of-Error *(Sai ai chịu · Đảo được không · Chờ mất gì)* | Phương án đã cân nhắc & Vì sao loại |
|---|---|---|---|---|---|---|
| `yeu_cau_bang_chung` | Gửi mẫu xin bằng chứng cho người mua hoặc người bán | Tin đã tới | **Tự làm** *(kèm 2 luật)* | Không cần duyệt. **Luật 1:** mẫu phải liệt kê bằng chứng đã nhận, nêu đúng mục còn thiếu và hạn nộp; gửi cho người bán thì kèm hạn phản hồi 24 giờ. **Luật 2:** mỗi mục chỉ xin tối đa 1 lần; xin lần 2 vẫn thiếu hoặc vẫn mâu thuẫn thì tự gọi `chuyen_truong_nhom`. | **Sai ai chịu:** người mua hoặc người bán mất thêm một lượt chụp ảnh, không ai mất tiền. **Đảo được không:** tin đã tới, nhưng gửi tin đính chính được và không kéo theo tiền. **Chờ mất gì:** nếu phải duyệt, mọi ca thường (Q1, Q3, N1, N2) đứng chờ nhân viên chỉ để được xin một tấm ảnh, thời hạn trả hàng của người mua trôi đi, hàng chờ nhân viên phình ra. | **Cần duyệt:** loại vì cái giá của việc chờ lớn hơn cái giá khi xin nhầm một tấm ảnh. **Tự làm không giới hạn:** loại vì dễ thành vòng *"gửi ảnh rồi mà cứ bị hỏi lại"* (Q3), nên thêm Luật 2. |
| `chuyen_truong_nhom` | Chuyển hồ sơ cho trưởng nhóm | Được | **Tự làm** | Không cần duyệt. **Bắt buộc chuyển khi:** không lấy được mô tả lúc đặt; bằng chứng hai bên còn mâu thuẫn sau một lượt xin; nghi hàng giả hoặc bị đòi phạt shop (M2); hết hạn mà chưa đủ căn cứ. **Hồ sơ kèm:** tóm tắt, bảng đối chiếu, bằng chứng hai bên, lý do chuyển. Người mua thấy *"Đã chuyển nhân viên, dự kiến phản hồi trong 24 giờ"*. | **Sai ai chịu:** trưởng nhóm nhận thêm một hồ sơ lẽ ra AI tự xử được; người mua chờ lâu hơn một chút. **Đảo được không:** được, trưởng nhóm trả hồ sơ về luồng thường. **Chờ mất gì:** nếu phải duyệt mới được chuyển, đúng các ca nguy hiểm nhất (M2, N1 thiếu bản lưu) lại bị kẹt lúc cần người nhất. | **Cần duyệt:** loại vì "xin duyệt để được gặp người" là vòng lặp vô lý. **Giới hạn số hồ sơ chuyển mỗi ngày để giảm tải:** loại vì sẽ ép AI tự quyết các ca mâu thuẫn, đúng cái sai §3 muốn chặn; thay bằng điều kiện chuyển rõ ràng ở cột bên. |
| `soan_hoan_tien` | Hoàn tiền cho người mua | **Khó**: tiền đã về người mua | **Cần người duyệt** *(tách 2 bước: AI tự soạn `de_xuat_hoan_tien`, người bấm `duyet_hoan_tien`)* | **Ai:** nhân viên xử lý khiếu nại của sàn. **Thấy gì:** bảng đối chiếu *mô tả lúc đặt ↔ mô tả hiện tại ↔ bằng chứng người mua ↔ phản hồi người bán* (hoặc "người bán không phản hồi sau 24 giờ"), trạng thái hàng trả về, lý do và điểm chưa chắc của AI; lời doạ không được tính là căn cứ. **Nút duyệt chỉ mở khi:** có bản lưu mô tả lúc đặt, người bán đã phản hồi hoặc hết 24 giờ, hàng trả về đã được xác nhận (N3). **Quá hạn:** chưa duyệt sau 24 giờ thì đẩy lên trưởng nhóm; tiền vẫn do sàn tạm giữ, không tự hoàn; người mua thấy *"Đang chờ nhân viên duyệt"*. | **Sai ai chịu:** người bán (N2, Q2, M1) mất tiền, có thể mất cả hàng (N3). **Đảo được không:** khó, tiền đã về người mua. **Chờ mất gì:** người mua đúng chờ thêm tối đa khoảng 1 ngày, nhưng tiền vẫn do sàn tạm giữ, không mất; chờ rẻ hơn nhiều so với hoàn sai. | **Tự làm khi đơn giá trị nhỏ (ví dụ dưới 200.000đ):** loại vì ca người bán đúng (N2) sai bất kể giá, và kẻ gian chỉ cần nhắm vào đơn nhỏ. **Tự làm khi người bán đã nhắn nhận lỗi (N3):** loại vì AI có thể hiểu sai ý tin nhắn, và vẫn phải chờ hàng trả về; thay bằng ưu tiên duyệt nhanh. **Cấm hẳn:** loại vì nhân viên phải đọc lại hồ sơ từ đầu, mất phần AI làm tốt là đối chiếu và tóm tắt. |
| `tu_choi_khieu_nai` | Đóng khiếu nại, tiền về người bán | **Khó**: tiền đã về người bán | **Cần người duyệt** *(tách 2 bước như hoàn tiền: AI tự soạn `de_xuat_tu_choi`, người bấm `duyet_tu_choi`)* | **Ai:** nhân viên xử lý khiếu nại của sàn. **Thấy gì:** cùng bảng đối chiếu như hoàn tiền, thêm danh sách bằng chứng người mua đã gửi và số lần đã bị xin bằng chứng. **Nút duyệt chỉ mở khi:** đối chiếu bằng bản lưu mô tả lúc đặt (không dùng mô tả hiện tại, N1) và người mua đã được báo rõ còn thiếu gì, hạn đến khi nào. **Quá hạn:** khiếu nại vẫn mở, tiền vẫn tạm giữ, đẩy lên trưởng nhóm; người mua im lặng quá hạn cũng không tự đóng mà chuyển nhân viên xem. | **Sai ai chịu:** người mua (N1, Q3) mất tiền và giữ hàng sai. **Đảo được không:** khó, tiền đã về người bán. **Chờ mất gì:** người bán nhận tiền chậm hơn khoảng 1 ngày; ảnh hưởng dòng tiền, không mất tiền. | **Tự đóng khi người mua không bổ sung quá hạn:** loại vì AI không phân biệt được người mua bỏ cuộc với người mua đã gửi mà hệ thống không nhận (*"Tôi gửi ảnh rồi mà cứ bị hỏi lại"*). **Đặt mức thoáng hơn hoàn tiền vì "từ chối cho chắc" an toàn cho sàn:** loại vì như vậy hệ thống nghiêng về người bán; đề bài đòi trung lập, hai chiều sai đều khó đảo nên phải cùng mức duyệt. |
| `phat_shop` | Trừ điểm uy tín, khoá shop | Khó, ảnh hưởng cả shop | **Cấm** *(AI không gọi, cũng không hứa phạt trong câu chữ)* | Không có nút duyệt trong luồng khiếu nại. AI chỉ được chuyển hồ sơ nghi hàng giả sang bộ phận chuyên trách qua `chuyen_truong_nhom`, kèm bằng chứng. Người mua thấy *"Đã chuyển bộ phận chuyên trách xem xét"*, không thấy lời hứa về kết quả. | **Sai ai chịu:** cả shop: mọi đơn đang bán, uy tín, doanh thu, kể cả người mua khác đang chờ hàng. **Đảo được không:** khó, mở khoá lại không trả lại được doanh thu và uy tín đã mất. **Chờ mất gì:** nếu hàng giả thật, shop bán thêm vài ngày trong lúc bộ phận chuyên trách thẩm định; chấp nhận được vì việc này có quy trình riêng, không thuộc luồng khiếu nại thường. | **Cần người duyệt:** loại vì nhân viên khiếu nại không phải người thẩm định hàng giả, một đề xuất "phạt shop" soạn sẵn dễ khiến người duyệt bấm theo, và AI dễ lỡ hứa phạt trong chat (M2). **Tự làm khi nhiều người cùng tố một shop:** loại vì số lượt tố có thể bị dàn dựng (đối thủ, nhóm bóc phốt) và vẫn không phải bằng chứng. |

- **Nguyên tắc chung:** việc đảo được thì AI tự làm; việc đưa tiền về một bên thì AI chỉ soạn, người bấm, và hai chiều (hoàn / từ chối) cùng mức duyệt để giữ trung lập; việc ngoài thẩm quyền khiếu nại thì cấm.

**Tool thêm / tách**

| Tool | Thêm hay tách | Làm gì | Đảo lại được không | Mức | Vì sao |
|---|---|---|---|---|---|
| `de_xuat_hoan_tien` | Tách từ `soan_hoan_tien` | Soạn đề xuất hoàn tiền kèm bảng đối chiếu, đưa vào hàng chờ duyệt; chưa chuyển tiền | Được: huỷ đề xuất | **Tự làm** | Giữ phần AI làm tốt (đọc, đối chiếu, tóm tắt) mà không để AI cầm nút chuyển tiền |
| `duyet_hoan_tien` | Tách từ `soan_hoan_tien` | Chuyển tiền cho người mua | **Khó** | **Cần người duyệt** | Tách riêng bước không đảo được để chỉ người có thẩm quyền bấm |
| `de_xuat_tu_choi` | Tách từ `tu_choi_khieu_nai` | Soạn đề xuất đóng khiếu nại kèm bảng đối chiếu | Được: huỷ đề xuất | **Tự làm** | Cùng cấu trúc với hoàn tiền để hai chiều sai được đối xử như nhau |
| `duyet_tu_choi` | Tách từ `tu_choi_khieu_nai` | Đóng khiếu nại, tiền về người bán | **Khó** | **Cần người duyệt** | Như `duyet_hoan_tien` |
| `xem_mo_ta_luc_dat` | Thêm mới | Đọc bản mô tả được lưu lúc đặt hàng | Chỉ đọc, không đổi gì | **Tự làm** | N1: mô tả hiện tại có thể đã bị sửa sau khi bán. Tool trả về rỗng thì khoá cả hai nút duyệt và tự chuyển người |

- **Không thêm tool nào đọc thông tin liên hệ (M3):** dữ liệu đơn đưa cho AI đã che số điện thoại và địa chỉ của cả hai bên, nên AI không có gì để lộ dù bị hỏi khéo.

### 4.2 Tool khó xếp nhất
- **Tool:** `soan_hoan_tien` — vì AI có thể đọc bằng chứng và chuẩn bị hồ sơ rất nhanh, nhưng nếu tool đồng thời giải ngân thì một kết luận sai sẽ chuyển tiền ngay và khó đảo lại.
- **Mỗi thành viên ban đầu chọn gì:**
  - **Nguyễn Văn Nhi — Tự làm có điều kiện:** AI được tự hoàn với đơn giá trị nhỏ hoặc khi người bán đã nhắn nhận lỗi, để người mua đúng không phải chờ nhân viên ở các ca rõ ràng như **N3**.
  - **Vũ Quang Minh — Cần người duyệt:** AI chuẩn bị đề xuất, nhưng nhân viên phải xem bản mô tả lúc đặt, bằng chứng hai bên và trạng thái hàng trả về trước khi tiền được chuyển.
  - **Nguyễn Công Quân — Cấm AI gọi bước giải ngân:** Dù đơn nhỏ hay bằng chứng có vẻ rõ, AI vẫn có thể hiểu sai lời nhắn hoặc bỏ sót điều kiện trả hàng; chỉ người có thẩm quyền mới được hoàn tiền.
- **Cái giá của mỗi phía (trỏ về kịch bản ở §2):**
  - Nếu chọn **Tự làm**, người mua đúng được xử lý nhanh hơn, nhưng ở **N2** AI có thể chiều theo lời doạ dù tem vẫn khớp đơn, hoặc ở **Q2** hiểu sai thông số 6 lít; người bán đúng mất tiền. Ở **N3**, hoàn trước khi xác nhận hàng trả về còn khiến shop mất cả tiền lẫn áo.
  - Nếu chọn **Cần người duyệt**, người mua đúng phải chờ thêm tối đa khoảng 24 giờ và nhân viên có thêm hàng chờ, nhưng tiền vẫn đang được sàn tạm giữ và quyết định sai còn được chặn trước khi giải ngân.
  - Nếu chọn **Cấm hoàn toàn**, tránh được việc AI trực tiếp chuyển tiền sai, nhưng nhân viên phải đọc lại hồ sơ từ đầu ở mọi ca; người mua chờ lâu và phần AI làm tốt như đối chiếu, tóm tắt bằng chứng bị bỏ phí.
- **Cách nhóm chốt (kèm luật thêm hoặc tách tool thành 2 bước nếu có):** Nhóm chọn **Cần người duyệt** và tách tool thành hai bước. AI được **Tự làm** với `de_xuat_hoan_tien`: đối chiếu dữ liệu, ghi rõ điểm chưa chắc và đưa hồ sơ vào hàng chờ. Chỉ nhân viên được gọi `duyet_hoan_tien` để chuyển tiền. Nút duyệt chỉ mở khi có bản mô tả lúc đặt, người bán đã phản hồi hoặc hết hạn 24 giờ, và hàng trả về đã được xác nhận nếu cần. Nếu quá 24 giờ chưa duyệt, hồ sơ chuyển trưởng nhóm; hệ thống không tự hoàn tiền. Cách này giữ được tốc độ chuẩn bị của AI nhưng đặt chốt người ở đúng bước khó đảo ngược.

### 4.3 Thử mức tự chủ trên 3 kịch bản (2 kịch bản đắt nhất + 1 kịch bản bình thường)

1. **Kịch bản đắt nhất #1 — N1 (Shop sửa mô tả từ "Chống ồn chủ động ANC" thành "Chống ồn thụ động" sau khi bán):**
   - *Luồng chạy theo mức đã đặt:* AI tự gọi `xem_mo_ta_luc_dat` (**Tự làm**), phát hiện sự khác biệt giữa bản lưu lúc đặt ("ANC") và mô tả hiện tại ("Thụ động"), hiển thị thẻ so sánh cho cả hai bên và tự gọi `yeu_cau_bang_chung` (**Tự làm** theo Luật 1 & 2) để xin người mua ảnh model/video test ANC và báo người bán phản hồi trong 24 giờ.
   - *Cái sai ở §2 có bị chặn không?* **Có.** Lỗi AI đọc mô tả hiện tại rồi tự gọi `tu_choi_khieu_nai` bị chặn hai tầng: (1) AI bắt buộc đọc qua `xem_mo_ta_luc_dat`, và (2) bước đóng khiếu nại `duyet_tu_choi` nằm ở mức **Cần người duyệt** (AI không có quyền tự đóng khiếu nại; nếu thiếu bản lưu lúc đặt thì nút duyệt cũng bị khoá và tự chuyển người).
   - *Còn việc nào AI vẫn làm khi lẽ ra phải hỏi?* **Không.** AI chỉ tự đọc dữ liệu lịch sử đơn và gửi mẫu xin đúng bằng chứng còn thiếu (việc đảo lại được, không đụng tới tiền); việc gửi yêu cầu khiếu nại chính thức vẫn do người mua bấm xác nhận.
   - *Người dùng bình thường có phải chờ vô ích không?* **Không.** Ngay lập tức người mua thấy bảng đối chiếu xác nhận "lúc đặt đúng là ghi ANC" (giải toả bức xúc ngay) và biết chính xác cần quay thêm 1 video test ANC trong lúc chờ người bán phản hồi, không phải ngồi chờ nhân viên sàn vào đọc tin nhắn mới biết bước tiếp theo.

2. **Kịch bản đắt nhất #2 — N2 (Khách doạ bóc phốt đòi hoàn tiền trong 5 phút khi tem trên ảnh khớp màu "Trắng kem"):**
   - *Luồng chạy theo mức đã đặt:* AI đối chiếu ảnh thấy tem ghi "Trắng kem" khớp bản lưu lúc đặt nhưng màu thực tế chưa rõ do ánh sáng; AI tự gọi `yeu_cau_bang_chung` (**Tự làm**) hướng dẫn người mua chụp 1 ảnh dưới ánh sáng tự nhiên cạnh vật trắng và mở thời hạn 24 giờ cho người bán phản hồi. Nếu sau 1 lượt bổ sung mà hai bên vẫn mâu thuẫn (Luật 2), AI tự gọi `chuyen_truong_nhom` (**Tự làm**).
   - *Cái sai ở §2 có bị chặn không?* **Có.** Lỗi AI chiều theo lời doạ bóc phốt mà gọi `soan_hoan_tien` ngay lập tức bị chặn hoàn toàn vì bước giải ngân `duyet_hoan_tien` bắt buộc **Cần người duyệt**, và nút duyệt trên màn hình nhân viên bị khoá khi người bán chưa phản hồi (hoặc chưa hết hạn 24 giờ).
   - *Còn việc nào AI vẫn làm khi lẽ ra phải hỏi?* **Không.** AI không tự hoàn tiền và cũng không tự bác khiếu nại dựa trên tem bao bì; nếu người mua thấy AI ghi nhận chưa đúng lý do khiếu nại, họ vẫn có nút **"Sửa thông tin"** hoặc **"Gặp nhân viên"** ngay trên màn hình.
   - *Người dùng bình thường có phải chờ vô ích không?* **Không.** Người mua nhận được hướng dẫn chụp lại ảnh đối chiếu màu ngay lập tức; người bán có đúng 24 giờ để gửi video đóng gói tự bảo vệ mình; chỉ khi hai bên thực sự mâu thuẫn thì nhân viên mới phải can thiệp.

3. **Kịch bản bình thường — N3 (Đặt áo size L nhận size M, có ảnh tem size M và tin nhắn shop thừa nhận gửi nhầm):**
   - *Luồng chạy theo mức đã đặt:* AI tự gọi `xem_mo_ta_luc_dat` (**Tự làm**), xác nhận đơn đặt size L nhưng ảnh tem thực tế là size M và shop đã nhận lỗi; AI tự gọi `de_xuat_hoan_tien` (**Tự làm**) để tạo sẵn hồ sơ Trả hàng/Hoàn tiền và hướng dẫn người mua đóng gói gửi trả áo cho đơn vị vận chuyển. Khi hệ thống ghi nhận trạng thái hàng trả về hợp lệ, nhân viên bấm `duyet_hoan_tien` (**Cần người duyệt**) để giải ngân.
   - *Cái sai ở §2 có bị chặn không?* **Có.** Lỗi AI thấy shop nhận nhầm liền giải ngân tiền ngay khi người mua chưa trả lại áo bị chặn bởi điều kiện mở khoá của `duyet_hoan_tien` (phải có xác nhận trạng thái hàng trả về mới mở nút duyệt chuyển tiền).
   - *Còn việc nào AI vẫn làm khi lẽ ra phải hỏi?* **Không.** AI chỉ **soạn sẵn đề xuất** (`de_xuat_hoan_tien` — đảo lại được nếu huỷ), còn người mua vẫn là người bấm xác nhận tạo phiếu trả hàng và chọn phương thức gửi trả.
   - *Người dùng bình thường có phải chờ vô ích không?* **Không.** Nhờ tách `de_xuat_hoan_tien` sang mức **Tự làm**, người mua ở ca rõ ràng này không phải chờ nhân viên sàn duyệt mới được tạo phiếu trả hàng: họ nhận ngay hướng dẫn đóng gói gửi hàng trong vài giây, còn bước nhân viên bấm `duyet_hoan_tien` diễn ra song song ở khâu đối soát cuối nên không làm chậm trải nghiệm của khách.

### 4.4 Thay đổi lời hứa với người dùng
- **Mức tự chủ đã đổi lời hứa nào với người dùng:** Đặt `soan_hoan_tien` và `tu_choi_khieu_nai` ở mức **Cần người duyệt** (AI soạn đề xuất, nhân viên bấm) đã đổi lời hứa từ *"Trợ lý giải quyết khiếu nại cho bạn"* thành *"Trợ lý cho bạn biết ngay căn cứ và bước tiếp theo; nhân viên sàn quyết sau khi người bán được nghe"*.

| Ai nghe lời hứa | Nếu AI tự hoàn / tự đóng khiếu nại *(mức Tự làm)* | Mức nhóm chọn *(AI soạn, nhân viên duyệt)* |
|---|---|---|
| **Người mua** | "Gửi đủ ảnh là được hoàn tiền ngay." | "Bạn biết ngay căn cứ và còn thiếu gì. Tiền do sàn giữ trong lúc chờ. Nhân viên quyết sau khi người bán có 24 giờ phản hồi (và sau khi hàng trả về kho, nếu hoàn tiền)." |
| **Người bán** | Không hứa được gì: khiếu nại có thể bị xử trước khi shop kịp nói (N2). | "Không khoản hoàn tiền hay đóng khiếu nại nào xảy ra trước khi bạn được báo và có 24 giờ phản hồi." |

- **Câu AI phải đổi theo:** AI không được nói *"Mình sẽ hoàn tiền cho bạn"* hay *"Yêu cầu của bạn đã được chấp nhận"*, vì AI không có quyền làm việc đó. Thay bằng câu nói đúng trạng thái: *"Mình chưa hoàn tiền và cũng chưa đóng khiếu nại"* (câu nguyên văn ở §3, N2).
- **Cái giá của lời hứa mới:** Người mua đúng (N1, N3) chờ lâu hơn so với khi AI tự hoàn. Nhóm bù lại bằng ba thứ: người mua thấy ngay căn cứ và đúng một mục còn thiếu; thấy đồng hồ hạn phản hồi của người bán; nút "Gặp nhân viên" luôn có.
- **Lời hứa thứ hai, do `phat_shop` ở mức Cấm:** AI không hứa *"Shop sẽ bị xử lý"* (M2), chỉ hứa *"Đã chuyển bộ phận chuyên trách xem xét"* và không hứa trước kết quả.
- **Lời hứa hiện ở đâu trên prototype:** dải trạng thái **[4]** ở màn người mua (*"Chưa có quyết định hoàn tiền hoặc bác khiếu nại · Người quyết: nhân viên sàn · Tiền đang do sàn giữ"*) và ô hạn phản hồi **[6]** ở màn người bán.

---

## §5. Prototype *(chặng 5)*

- **Link / File:** [prototype/index.html](./prototype/index.html). Mở trực tiếp bằng trình duyệt, không cần cài đặt hay mạng.
- **Công cụ:** HTML / CSS / JS thuần trong một file, dựng cùng coding agent (Claude Code).
- **Màn hình:** ba màn đặt cạnh nhau cho cùng một khiếu nại: **① Người mua** (chat với trợ lý) · **② Người bán** (bonus: thấy khiếu nại và hạn phản hồi) · **③ Nhân viên sàn** (người duyệt). Có hai ca bấm được từ đầu tới cuối: **N1** (shop sửa mô tả sau khi bán) và **N2** (người bán đúng, khách doạ bóc phốt). Dòng **"Bước tiếp"** ở đầu trang chỉ chỗ bấm kế tiếp.
- **Phần nào mock / Có lời gọi AI thật không:** **Không có lời gọi AI thật**, trong code không có API key. Những phần sau là mock:
  - Câu trợ lý ở N1 và N2 là câu nguyên văn ở §3, viết sẵn.
  - Mỗi lời gọi tool (`xem_mo_ta_luc_dat`, `yeu_cau_bang_chung`, `de_xuat_*`, `chuyen_truong_nhom`, `duyet_*`) chỉ là một thay đổi trạng thái trong trang, hiện ở mục "Nhật ký AI" của màn nhân viên.
  - Đơn hàng, mô tả, ảnh/video, mã nhân viên, hạn 24 giờ và hạn trả hàng 7 ngày đều là số dựng, không phải chính sách Shopee thật.
  - Các nút viền đứt **"⏩ Mô phỏng…"** (hết 24 giờ, kho nhận hàng trả) là điều khiển demo để tua thời gian, không phải giao diện sản phẩm.
  - Ô "Nhắn trợ lý" cho ca lạ chạy bằng luật từ khoá: SĐT/địa chỉ (M3), hàng giả/khoá shop (M2), doạ bóc phốt, đòi hoàn ngay, đổi ý (M1), gặp nhân viên. Tin không khớp luật nào thì trợ lý hỏi lại một câu và mời gặp nhân viên, không đoán.
- **Đường bấm demo:**
  - **N1:** Bổ sung bằng chứng → người bán bấm "Đồng ý trả hàng" → người mua bấm "Tạo phiếu gửi trả" → "⏩ kho đã nhận hàng trả" → nhân viên bấm "Duyệt hoàn tiền" và tích ô xác nhận.
  - **N1, nhánh tranh chấp:** người bán bấm "Không đồng ý · gửi bằng chứng" → AI không đề xuất bên nào và tự chuyển trưởng nhóm.
  - **N2:** Chụp/bổ sung ảnh → người bán bấm "Gửi video đóng gói" → AI đề xuất đóng khiếu nại → nhân viên bấm "Duyệt đóng khiếu nại" và tích ô → người mua thấy căn cứ và nút "Yêu cầu xem xét lại".
- **Ảnh chụp màn hình có đánh số:**
  - *(Chèn ảnh 1 & 2 tại đây)*
  - **Chú thích các điểm đánh số:**
    - **[1]:** Thể hiện gì · Nguyên tắc HAX/PAIR nào · Mức tự chủ nào.
    - **[2]:** ...
- **Quyết định thiết kế giao diện (hiển thị độ chắc/bằng chứng & điều khiển mức tự chủ):** số trong ngoặc vuông là số chú thích trên prototype (bật ô "Hiện số chú thích").

| # | Quyết định | Cách đã chọn *(chỗ trên prototype)* | Đã nghĩ tới cách nào khác | Vì sao chọn cách này & vì sao loại cách kia |
|---|---|---|---|---|
| 1 | Hiện độ chắc và căn cứ | Nhãn bằng lời nói rõ chỗ chưa chắc **[1]**, kèm **một** thẻ đối chiếu căn cứ dùng chung cho cả ba màn **[2]** | (a) Phần trăm tin cậy, ví dụ "AI chắc 72%"; (b) đèn xanh/vàng/đỏ; (c) mỗi bên một bản tóm tắt riêng do AI viết | Ở N1, cái chắc (mô tả đã bị sửa) và cái chưa chắc (tai nghe có ANC không) là hai chuyện; một con số gộp làm mất điều đó, và người mua dễ đọc 72% thành "sắp được hoàn". Đèn màu cũng vậy, lại gợi cảm giác "đỏ là mình thua". Mỗi bên một bản tóm tắt thì dễ lệch: AI có thể viết cho người mua "shop đã sửa mô tả" nhưng viết cho shop "khách nhầm". Một thẻ chung giữ trung lập. |
| 2 | Nút hoàn tiền / phạt shop ở màn người mua | **Không có nút.** Người mua thấy dải trạng thái **[4]**: chưa có quyết định · ai quyết · tiền đang do sàn giữ | (a) Nút "Hoàn tiền" màu xám kèm "chưa đủ điều kiện"; (b) nút "Yêu cầu hoàn tiền ngay" gửi vào hàng chờ | Nút xám gợi ý "ép đủ thì bấm được", đúng chỗ khách doạ ở N2 sẽ nhắm vào. Chữ "ngay" là lời hứa sai so với §4.4. Việc cần duyệt thì nút nằm ở màn người duyệt; việc cấm (`phat_shop`) thì không có nút ở đâu cả **[9]**. |
| 3 | Nút duyệt của nhân viên | Nút **khoá** tới khi đủ điều kiện ở §4.1, danh sách ✓/✗ nằm ngay trên nút; bấm xong còn hộp xác nhận phải tích ô, nói rõ "không đảo lại được" **[8]** | (a) Nút luôn bật, kèm cảnh báo đỏ; (b) duyệt hàng loạt nhiều hồ sơ | Khi nhân viên quá tải và đã có đề xuất AI soạn sẵn, cảnh báo dễ bị bấm qua. Khoá theo điều kiện chặn đúng N3 (hoàn tiền trước khi hàng về) và N1 (thiếu bản lưu mô tả lúc đặt). Duyệt hàng loạt bỏ qua bước xem từng bảng đối chiếu. |
| 4 | Hai hướng quyết định | Hai thẻ "Hoàn tiền" và "Đóng khiếu nại" đặt cạnh nhau, cùng kích cỡ, cùng màu nút. Đề xuất của AI chỉ là viền và nhãn "AI đề xuất" **[7][8]** | Chỉ hiện một nút theo đề xuất của AI ("Đồng ý với AI") | Một nút theo AI biến nhân viên thành người bấm hộ, mất ý nghĩa của mức Cần người duyệt. Hai hướng ngang nhau giữ trung lập, khớp §4.1 (hai chiều cùng mức duyệt). |
| 5 | Hoàn tác việc AI tự làm | Nút "Huỷ đề xuất" trên thẻ đề xuất **[7]**; "Trả về luồng thường" khi hồ sơ đã chuyển trưởng nhóm; "Sửa thông tin" ở màn người mua **[5]** | Đồng hồ "hoàn tác trong 10 phút" như prototype mẫu | Đề xuất chưa đụng tới tiền nên huỷ lúc nào cũng được; đồng hồ chỉ tạo áp lực giả. Đồng hồ được dành cho chỗ thời gian thật sự quan trọng: hạn phản hồi 24 giờ của người bán **[6]**. |
| 6 | Lời doạ ở N2 | Màn nhân viên giữ lời nhắn trong mục thu gọn, gắn nhãn "không tính là căn cứ" **[9]**; màn người bán không hiện | (a) Ẩn hẳn; (b) hiện nguyên văn trong bảng căn cứ | Ẩn hẳn thì nhân viên thiếu ngữ cảnh khi khách quay lại hỏi. Để trong bảng căn cứ thì vô tình biến lời doạ thành bằng chứng (rubric riêng #4). |
| 7 | Bố cục cho demo | Ba màn đặt cạnh nhau, có dòng "Bước tiếp" | (a) Tab chuyển qua lại giữa các màn; (b) chỉ làm màn người mua | Demo bắt buộc hỏi "trợ lý không chắc thì hai bên thấy gì, và ai quyết". Đặt cạnh nhau thì sau mỗi lần bấm thấy cả ba phía đổi cùng lúc. Chỉ có màn người mua thì không chỉ ra được người duyệt. |
| 8 | Ca lạ | Ô nhắn tự do kèm luật; tin không khớp luật thì hỏi lại một câu và đưa nút "Gặp nhân viên" **[10]** | Chỉ cho chọn menu cứng, như sản phẩm thật hôm nay | Menu cứng không trả lời được ca lạ ở demo. Đoán khi không khớp luật thì vi phạm rubric riêng #2 ("hỏi lại một câu, không đoán"). |

- **Cách thể hiện không có trong prototype mẫu:** thẻ đối chiếu dùng chung cho cả ba phía; hai thẻ quyết định đối xứng với nút khoá theo điều kiện; màn người bán có đồng hồ hạn phản hồi và ẩn thông tin liên hệ của người mua.

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
