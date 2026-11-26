# BÀI LÀM PHA 2 – HACKATHON 02

- **Mã đề:** 05
- **Họ tên nguyễn đình nguyên
- **Lớp:** HCM-KS26-CNTT1
- **Link repository Pha2:**
- **Công cụ AI đã dùng (ChatGPT/Claude/Gemini):** 

> Pha 2 (20 phút cuối). Mỗi mục: mở cuộc trò chuyện mới, gửi nguyên văn prompt đã viết trong bai_lam_pha1.md, dán câu trả lời và link chia sẻ vào đây. Xong, kéo thả file này vào repository Pha2.

## 4.2. Trích xuất tin nhắn đặt vé xem phim ra JSON

**KET QUA AI**

```text
(Dán câu trả lời của AI vào đây (bấm nút Copy dưới câu trả lời) – xoá dòng này)
 - Context:
Bạn là nhân viên mới của RẠP CHIẾU PHIM ÁNH DƯƠNG. Rạp bán 3 loại vé: phim 2D, phim 3D và phòng chiếu Gold Class. Quản lý giao bạn xử lý tin nhắn đặt vé của khách một cách chính xác và an toàn. Nhiệm vụ này chỉ là trích xuất dữ liệu, không báo giá, không trả lời khách.

R - Request:
Chuyển 3 tin nhắn (TN1, TN2, TN3) trong cặp thẻ <tin_nhan> thành một mảng JSON gồm 3 đối tượng, đúng thứ tự tin nhắn. Mỗi đối tượng có đúng 6 trường: "loai_ve", "ngay_chieu", "ve_nguoi_lon", "ve_tre_em", "loai_ghe", "ghi_chu".

E - Explanation:
<bang_ma_loai_ve>
- Phim 2D → "P2D"
- Phim 3D → "P3D"
- Phòng chiếu Gold Class → "GOLD"
</bang_ma_loai_ve>

<quy_tac>
1. "loai_ve" lấy theo bảng mã loại vé.
2. "ngay_chieu" dạng YYYY-MM-DD. Tin nhắn không ghi năm thì lấy năm 2026. Không có ngày cụ thể (kể cả cụm mơ hồ như "dịp Noel", "cuối tuần", "hôm nào cũng được") thì null.
3. Khách không nhắc trẻ em thì "ve_tre_em" là 0 và mọi khách là người lớn.
4. "loai_ghe" chỉ nhận "thuong", "doi" hoặc null (khách không nhắc).
5. "ghi_chu" chỉ ghi yêu cầu đặc biệt về sức khoẻ, đi lại, chỗ ngồi đặc biệt; không có thì null. Không ghi tuổi trẻ em, giá, hay câu hỏi về chỗ trống vào "ghi_chu".
</quy_tac>

A - Action:
Đối chiếu từng tin nhắn với bảng mã và 5 quy tắc, tham khảo 2 ví dụ dưới đây, rồi điền chính xác giá trị cho 6 trường.

<vi_du>
Tin nhắn: Đặt cho mình phim 3D ngày 16/11/2026, 2 người lớn 2 bé, ghế đôi nha. Một bé cần ghế nâng.
Kết quả: {"loai_ve": "P3D", "ngay_chieu": "2026-11-16", "ve_nguoi_lon": 2, "ve_tre_em": 2, "loai_ghe": "doi", "ghi_chu": "1 bé cần ghế nâng"}

Tin nhắn: Phòng Gold còn ghế không ạ, em xem 1 mình, ghế thường, hôm nào cũng được.
Kết quả: {"loai_ve": "GOLD", "ngay_chieu": null, "ve_nguoi_lon": 1, "ve_tre_em": 0, "loai_ghe": "thuong", "ghi_chu": null}
</vi_du>

T - Tone:
Chính xác, máy móc, chuẩn hóa dữ liệu.

E - Extras:
Chỉ trả về duy nhất một mảng JSON hợp lệ, bắt đầu bằng [ và kết thúc bằng ]. Số lượng vé là số nguyên (không có dấu nháy). Không dùng code fence, không chào hỏi, không giải thích.

<tin_nhan>
TN1: Cho mình 2 vé người lớn và 1 vé trẻ em 10 tuổi xem phim 2D ngày 24/12 nhé, ghế thường.

TN2: Đặt 3 vé phim 3D ngày 01/01/2027 ạ. Một người hay chóng mặt, cần ngồi gần lối ra.

TN3: Phòng Gold dịp Noel còn chỗ không ạ? Nhóm em 4 người lớn, một bạn đi xe lăn, ngồi ghế đôi.
</tin_nhan>
```

**Link trò chuyện:**

## 4.3. Báo giá bằng Chain-of-Thought

**KET QUA AI**

```text
(Dán câu trả lời của AI vào đây (bấm nút Copy dưới câu trả lời) – xoá dòng này)
```

**Link trò chuyện:**

### b) Kiểm tra kết quả AI

(Ghi kết luận AI đúng hay sai; nếu sai, ghi bước sai đầu tiên và kết quả đúng của bước đó – xoá dòng này)

## 4.4. Dây chuyền xử lý khiếu nại – kết quả Trạm 1

**KET QUA AI**

```text
(Dán câu trả lời của AI vào đây (bấm nút Copy dưới câu trả lời) – xoá dòng này)
C - Context:
Bạn là nhân viên bán vé của RẠP CHIẾU PHIM ÁNH DƯƠNG, chịu trách nhiệm báo giá chính xác cho khách theo bảng giá và quy tắc của rạp.

R - Request:
Dựa trên nội dung trong các thẻ <bang_gia>, <quy_tac> và <yeu_cau>, tính tổng tiền thanh toán cho khách.

E - Explanation:
<bang_gia>
Giá vé người lớn (đồng/vé):
- P2D (Phim 2D): 90.000
- P3D (Phim 3D): 130.000
- GOLD (Phòng chiếu Gold Class): 250.000
</bang_gia>

<quy_tac>
1. Trẻ cao từ 1 m đến 1,3 m tính 70% giá vé người lớn. Trẻ cao trên 1,3 m tính như người lớn. Trẻ dưới 1 m ngồi cùng người lớn thì miễn phí.
2. Nhóm từ 5 người lớn trở lên được giảm 10% trên tiền vé (tiền vé người lớn + tiền vé trẻ em).
3. Combo bắp nước: 75.000 đồng/combo. Combo không được giảm giá.
4. Tổng tiền = tiền vé sau giảm + tiền combo.
</quy_tac>

<yeu_cau>
Nhóm xem phim 3D (P3D) gồm 5 người lớn, 2 trẻ em (cao 1,1 m và 1,25 m) và 1 bé cao 0,9 m (ngồi cùng người lớn). Nhóm mua 3 combo bắp nước.
</yeu_cau>

A - Action:
Suy luận từng bước (Chain-of-Thought) theo đúng 6 bước sau. Mỗi bước ghi công thức và kết quả:
Bước 1: Tiền vé người lớn.
Bước 2: Tiền vé trẻ em (liệt kê từng bé, bé miễn phí ghi 0 đồng).
Bước 3: Tổng tiền vé = bước 1 + bước 2.
Bước 4: Tiền giảm giá nhóm (nếu đủ điều kiện) và tiền vé sau giảm.
Bước 5: Tiền combo.
Bước 6: Tổng tiền thanh toán = tiền vé sau giảm + tiền combo.

T - Tone:
Chính xác, ngắn gọn, đúng từng bước, không thêm lời chào.

E - Extras:
Dòng cuối cùng của câu trả lời bắt buộc đúng khuôn sau, số nguyên không có dấu chấm hoặc dấu phẩy ngăn cách:
[RESULT: <số tiền> đồng]
Kết quả kỳ vọng (để kiểm tra)
Bước	Tính	Kết quả
1	5 × 130.000	650.000
2	130.000 × 70% = 91.000; 2 bé × 91.000 = 182.000; bé 0,9 m = 0	182.000
3	650.000 + 182.000	832.000
4	5 người lớn → giảm 10% × 832.000 = 83.200; còn 832.000 − 83.200	748.800
5	3 × 75.000	225.000
6	748.800 + 225.000	973.800

Dòng cuối kỳ vọng: [RESULT: 973800 đồng]

Hai điều cần lưu ý:

Điều kiện "ngồi cùng người lớn" cho bé 0,9 m là điều tôi thêm vào để quy tắc 1 áp dụng được. Nếu đề gốc không cho phép thêm, bạn bỏ cụm đó đi, nhưng khi đó model có thể phân vân.
Khuôn [RES
```

**Link trò chuyện:**

## 4.5. Prompt chấm bài theo Rubric

**KET QUA AI**

```text
(Dán câu trả lời của AI vào đây (bấm nút Copy dưới câu trả lời) – xoá dòng này)
```

**Link trò chuyện:**
