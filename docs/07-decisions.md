# Nhật ký quyết định

> **Trạng thái:** tổng hợp các lựa chọn thiết kế đã xuất hiện trong báo cáo nguồn. Báo cáo chưa ghi nhận phương án thay thế đã cân nhắc hay người phê duyệt, nên không mục nào được đánh dấu là "đã chốt".

**Nguồn:** `BaoCao_DT04.md` (cột *Nguồn* ghi mục tương ứng).

**Quy ước trạng thái:**
- *Đề xuất* — có trong báo cáo như một lựa chọn thiết kế.
- *Chưa chốt* — báo cáo nêu nhiều phương án.
- *Mở* — là giá trị ước lượng, cần xác minh bằng khảo sát thật.

| ID | Quyết định | Lý do theo báo cáo | Trạng thái | Nguồn |
|---|---|---|---|---|
| D-01 | Tách `LO_HANG` khỏi `HANG_HOA` | Để biết tồn theo từng lô và hạn sử dụng; nếu gộp vào `HANG_HOA` thì không triển khai được `BR-01` (FEFO) và `BR-04` (cận hạn) | Đề xuất | 4.2, Chương 7 |
| D-02 | Hàng trả lại/hàng lỗi là khu vực trong từng kho (`TON_KHO.trang_thai_ton`), không phải kho vật lý thứ tư | Báo cáo "3 kho" vẫn là 3 kho vật lý; nghiệp vụ bán hàng chỉ tính tồn "Bán được" | Đề xuất | 4.2, BR-03 |
| D-03 | Tách công nợ tạm tính và công nợ chính thức; chỉ ghi nợ chính thức khi từng phiếu xuất giao thành công | Tránh cộng lặp giá trị đơn; mỗi phần giá trị chỉ chuyển hoặc giải phóng một lần | Đề xuất | 4.2, BR-02 |
| D-04 | Giữ chỗ số dư trả trước theo từng đơn (`DON_HANG.so_tien_tra_truoc_da_phan_bo`) | Tránh dùng trùng số dư trả trước cho nhiều đơn | Đề xuất | BR-02, UC-DH-01 |
| D-05 | FEFO áp dụng trong phạm vi một kho đã chọn; mỗi phiếu xuất thuộc đúng một kho | Phù hợp phạm vi `BR-01` ("cùng kho") và cho phép xuất một phần từ nhiều kho | Đề xuất | BR-01, UC-XK-01 |
| D-06 | Dùng CSDL quan hệ (PostgreSQL hoặc MySQL), không dùng NoSQL | Cần toàn vẹn dữ liệu khi nhiều giao dịch xuất/nhập diễn ra đồng thời; ERD có nhiều khóa ngoại chặt chẽ | Đề xuất (chưa chốt sản phẩm) | 2.4 |
| D-07 | Backend: Spring Boot hoặc ASP.NET Core; Frontend: ReactJS hoặc Vue.js; xác thực: JWT | Phân quyền theo vai trò, hỗ trợ transaction; giao diện nhiều bảng và lọc. Báo cáo ghi nhóm nên chọn theo thế mạnh đã học | Chưa chốt | 2.4 |
| D-08 | Ngưỡng cảnh báo cận hạn 30 ngày (`BR-04`) | Ước lượng phổ biến cho hàng tiêu dùng nhanh; báo cáo thừa nhận một ngưỡng chung không phù hợp cho mọi mã hàng | Mở | Ghi chú dưới Bảng 2.3 |
| D-09 | Tối thiểu 30 người dùng đồng thời (`NFR-02`) | Ước tính khoảng 215 tài khoản, tỷ lệ đồng thời giờ cao điểm 15–20% | Mở | Ghi chú dưới Bảng 3.2 |
| D-10 | Ngoài phạm vi: vận tải/định tuyến, nhân sự/tính lương, tích hợp phần mềm kế toán tổng thể | Giới hạn phạm vi đề tài | Đề xuất | 2.2 |

## Câu hỏi còn mở

- Nếu tách ngưỡng cận hạn theo nhóm hạn dùng, báo cáo gợi ý thêm `nhom_han_dung` và `nguong_canh_bao_ngay` vào `HANG_HOA` (chưa có trong ERD hiện tại).
- Chọn công nghệ cụ thể cho backend, CSDL, frontend (xem D-06, D-07).
