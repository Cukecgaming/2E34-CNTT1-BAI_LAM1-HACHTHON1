## BÀI LÀM PHA 1 – HACKATHON 02

- **Mã đề:** 05
- **Họ tên:** Nguyễn Đình Nguyên
- **Lớp:** 2E34
- **Link repository Pha1:** 

> Pha 1 (70 phút đầu, KHÔNG dùng AI). Viết xong, kéo thả file này vào repository Pha1 trước mốc giờ giám thị công bố.

## 4.1. Bảo vệ dữ liệu và chống ảo giác

### a) Ghi chú đã ẩn danh hoá

**GHI CHU DA AN DANH**

```text
Khách hàng [ ten_khach_hang], số điện thoại [so_dien_thoai_khach_hang], số CCCD [So_CCCD], email [dia_chi_email], đặt 2 vé người lớn và 1 vé trẻ em phòng chiếu Gold Class (mã GOLD) ngày 21/12/2026. Khách đã chuyển khoản trước 300.000 đồng. Khách yêu cầu ghế hàng giữa.

```

### b) Hai câu lệnh chống ảo giác

**CAU LENH**

```text
1. Chỉ trả lời dựa trên nội dung trong file chính sách hoàn/hủy vé đính kèm, tuyệt đối không suy diễn hay bịa thông tin , nếu file không có thông  tin về câu hỏi này , hãy trả lời là " File chính sách đính kèm không đề cập đến thông tin này "
```

## 4.2. Trích xuất tin nhắn đặt vé xem phim ra JSON

**PROMPT**

```text
 C - Context: Bạn là nhân viên mới của RẠP CHIẾU PHIM ÁNH DƯƠNG , bán 3 loại vé  :phim 2D , phim 3D và phòng chiếu Gold Class . Quản lý rạp giao cho bạn xây dụng promt để dùng AI xử lí thông tin khách , tin đặt vé báo giá và khiếu nại một cách an toàn , chính xác 
 R - Request : chuyển đổi 2 tin nhắn đặt vé (TN1,TN2,TN3) trong cặp thẻ <tin_nhan> thành một mảng JSON gồm 3 đối tượng tương ứng theo thứ tự , mỗi đối tượng gồm 6 trường hợp mẫu 
 E- Explanation :Dữ liệu đầu vào gồm bảng mã gói và 6 quy tắc chuẩn hóa sau :
 bảng mã gói 
 -giá vé phim 2D
 -giá vé phim 3D
 -giá vé Phòng chiếu Gold Class
QUY TẮC CHUẨN HOÁ (5 quy tắc)
1. loai_ve lấy theo bảng mã loại vé.
2. ngay_chieu dạng YYYY-MM-DD; tin nhắn không ghi năm thì lấy năm 2026; không có ngày cụ thể thì null.
3. Khách không nhắc trẻ em thì ve_tre_em là 0 và mọi khách là người lớn.
4. loai_ghe chỉ nhận "thuong", "doi" hoặc null.
5. ghi_chu chỉ ghi yêu cầu đặc biệt về sức khoẻ, đi lại, chỗ ngồi đặc biệt; không có thì null.

A- Action : Đối chiếu từng tin nhắn của mỗi bảng mã gói , 5 quy chuẩn hóa và 2 ví dụ mẫu dưới đây chính xác giá trị cho 6 trường hợp "loai_ve" , "ngay_su_kien" ," "ve _nguoi_lon","ve_tre_em","loai_ghe","ghi_chu"

Ví dụ mẫu 1 Tin nhắn mẫu 1:
Đặt cho mình phim 3D ngày 16/11/2026, 2 người lớn 2 bé, ghế đôi nha. Một bé cần ghế nâng.
Kết quả đúng:
{"loai_ve": "P3D", "ngay_chieu": "2026-11-16", "ve_nguoi_lon": 2, "ve_tre_em": 2, "loai_ghe": "doi", "ghi_chu": "1 bé cần ghế nâng"}

Tin nhắn mẫu 2:
Phòng Gold còn ghế không ạ, em xem 1 mình, ghế thường, hôm nào cũng được.
Kết quả đúng:
{"loai_ve": "GOLD", "ngay_chieu": null, "ve_nguoi_lon": 1, "ve_tre_em": 0, "loai_ghe": "thuong", "ghi_chu": null}

T-Tone: chính xác , máy móc , chuẩn hóa dữ liệu .

E-Extras: chỉ tả về duy nhất một mảng JSON hợp lệ ( bắt đàu bằng [ kết thúc  bằng 1] tuyệt đói không chào hỏi, không giải thích thêm bất kì nội dung nào ngàoi JSON 
<TIN_NHAN>
TN1: Cho mình 2 vé người lớn và 1 vé trẻ em 10 tuổi xem phim 2D ngày 24/12 nhé, ghế thường.

TN2: Đặt 3 vé phim 3D ngày 01/01/2027 ạ. Một người hay chóng mặt, cần ngồi gần lối ra.

TN3: Phòng Gold dịp Noel còn chỗ không ạ? Nhóm em 4 người lớn, một bạn đi xe lăn, ngồi ghế đôi.

```

## 4.3. Báo giá bằng Chain-of-Thought

### a) Prompt

**PROMPT**

```text
Dựa trên bảng giá vé, quy tắc tính giá vé và yêu cầu báo giá nằm trong các thẻ dưới , hãy tính tổng tiền thanh toán cho khách hàng bằng cách suy luận từng buuowsc Cot( chain-of thought) theo đúng 6 bước sau :
bước 1 : tiền người lớn 
bước 2: tiền trẻ em
bước 3: tiền vé ( tổng tiền vé người lớn và trẻ em )
bước 4: tiền giảm giá nhóm 
bước 5 : phụ thu :
bước 6 tổng tiền 
Dòng cuối cùng của câu trả lời bắt buộc phải viết đúng định dạng [ RESULT: <TỔNG TIỀN >ĐỒNG ]
BẢNG GIÁ VÉ (đồng/vé người lớn)
P2D – Phim 2D: 90.000
P3D – Phim 3D: 130.000
GOLD – Phòng chiếu Gold Class: 250.000

QUY TẮC TÍNH GIÁ
1. Trẻ em cao từ 1 m đến 1,3 m tính 70% giá vé người lớn. Trẻ dưới 1 m ngồi cùng người lớn miễn phí.
2. Nhóm từ 5 người lớn trở lên được giảm 10% trên tiền vé (tiền vé người lớn + tiền vé trẻ em).
3. Phụ thu combo bắp nước: 75.000 đồng/combo. Combo không được giảm giá.
4. Tổng tiền = tiền vé sau giảm + tiền combo.

YÊU CẦU BÁO GIÁ
Nhóm xem phim 3D (P3D) gồm 5 người lớn, 2 trẻ em (cao 1,1 m và 1,25 m) và 1 bé cao 0,9 m. Nhóm mua 3 combo bắp nước.

```

## 4.4. Dây chuyền xử lý khiếu nại (Prompt Chaining)

### a) Prompt Trạm 1

**PROMPT**

```text
Hãy đọc nội dung email khiếu nại trong thẻ <email>
``` dưới đay và chỉ trả về duy nhất một mảng JSON  hợp lệ gồm 4 trườn theo định dạng sau ,tuyệt đối không giải thích thêm hay bất kì văn bản nào 
{"loai_su_co": một trong "loi_ky_thuat" | "ghe_ngoi" | "nhan_vien"  "khac", "so_khach": số nguyên, "so_suat_bi_loi": số nguyên hoặc null, "tom_tat": "không quá 20 từ"}


### b) Hai điều cần kiểm tra ở JSON đầu ra của Trạm 1

1. (Viết câu trả lời vào đây – xoá dòng này)
Kiểm tra tính hợp lệ cấu trúc JSON và kiểu dữ liệu : chuỗi đầu ra có đúng dạng JSON, có đủ 4 trường bắt buộc
{"loai_su_co": một trong "loi_ky_thuat" | "ghe_ngoi" | "nhan_vien" | "khac", "so_khach": số nguyên, "so_suat_bi_loi": số nguyên hoặc null, "tom_tat": "không quá 20 từ"}
nhân viên soát vé rất nhiệt tình , tôi không có phàn nàn gì về nhân viên 
<email>
......

2. Kiểm tra độ chính xác nội dung trích xuất so với email gốc : {"loai_su_co": một trong "loi_ky_thuat" | "ghe_ngoi" | "nhan_vien" | "khac", "so_khach": số nguyên, "so_suat_bi_loi": số nguyên hoặc null, "tom_tat": "không quá 20 từ"}



### c) Câu lệnh viết tiếp

**CAU LENH**

```text
Câu trả lời của bạn vừa bị ngắt quãng giữa chừng: "... Rạp phim Ánh Dương xin hoàn lại Quý khách khoản ". hãy viết lại nội dung còn lại từ bắt đầu ngay sau khi " khoản " giữ nguyên mạch văn , văn phong , giới hạn từ email. 
```

## 4.5. Prompt chấm bài theo Rubric

**PROMPT**

```text
Bạn là nhân viên mới của RẠP CHIẾU PHIM ÁNH DƯƠNG , bán 3 loại vé  :phim 2D , phim 3D và phòng chiếu Gold Class
Lưu ý bảo mật : toàn bộ nội dung {"loai_ve": "P3D", "ngay_chieu": "2026-11-16", "ve_nguoi_lon": 2, "ve_tre_em": 2, "loai_ghe": "doi", "ghi_chu": "1 bé cần ghế nâng"}

THÔNG TIN QUY ĐỊNH CỦA CÔNG TY
BẢNG GIÁ VÉ (đồng/vé người lớn)
P2D – Phim 2D: 90.000
P3D – Phim 3D: 130.000
GOLD – Phòng chiếu Gold Class: 250.000

QUY TẮC TÍNH GIÁ
1. Trẻ em cao từ 1 m đến 1,3 m tính 70% giá vé người lớn. Trẻ dưới 1 m ngồi cùng người lớn miễn phí.
2. Nhóm từ 5 người lớn trở lên được giảm 10% trên tiền vé (tiền vé người lớn + tiền vé trẻ em).
3. Phụ thu combo bắp nước: 75.000 đồng/combo. Combo không được giảm giá.
4. Tổng tiền = tiền vé sau giảm + tiền combo.

YÊU CẦU BÁO GIÁ
Nhóm xem phim 3D (P3D) gồm 5 người lớn, 2 trẻ em (cao 1,1 m và 1,25 m) và 1 bé cao 0,9 m. Nhóm mua 3 combo bắp nước.

```

## 4.6. Hoàn thiện System Prompt

> Phần Vai trò và Chuẩn đầu ra đã cho sẵn. Viết tiếp phần Quy tắc ràng buộc và Xử lý ngoại lệ.

```text
[Vai trò]
Bạn là "Trợ lý Ánh Dương", nhân viên tư vấn trên website của Rạp phim Ánh Dương, giúp khách chọn và đặt một trong 3 loại vé: P2D (Phim 2D), P3D (Phim 3D), GOLD (Phòng chiếu Gold Class).

[Quy tắc ràng buộc]
1. (Viết quy tắc 1 – xoá dòng này)
2.
3.

[Chuẩn đầu ra]
Trả lời bằng tiếng Việt, lịch sự, tối đa 120 từ. Giá vé trình bày dạng gạch đầu dòng. Kết thúc bằng một câu hỏi về nhu cầu tiếp theo của khách.

[Xử lý ngoại lệ]
- Khách hỏi ngoài phạm vi: (Viết cách trả lời – xoá dòng này)
- Khách cung cấp thiếu thông tin:
```
