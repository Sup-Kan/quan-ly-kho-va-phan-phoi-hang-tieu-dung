# Kiểm thử

> **Trạng thái:** chưa có test case, chưa có kết quả thực thi; `tests/` hiện còn trống.

## Các điểm cần kiểm thử mà báo cáo nguồn đã nêu

Danh sách dưới đây chỉ trích lại yêu cầu/ràng buộc có sẵn trong báo cáo:

| Nhóm | Điểm cần kiểm thử | Nguồn |
|---|---|---|
| Luồng use case | Luồng chính, luồng thay thế (`AF-*`) và luồng ngoại lệ (`EF-*`) của 34 use case | Mục 5.3, Chương 7 |
| Luật nghiệp vụ | `BR-01` (FEFO), `BR-02` (hạn mức công nợ), `BR-03` (hàng trả/lỗi), `BR-04` (cận hạn), `BR-05` (nhật ký tồn kho) | Bảng 2.3 |
| Ràng buộc dữ liệu | `UNIQUE(CT_PHIEU_NHAP.ma_lo)`, `UNIQUE(CONG_NO.ma_phieu_xuat)`, `UNIQUE(CONG_NO.ma_phieu_tra)`; kho nguồn khác kho đích khi điều chuyển | Bảng 4.2 |
| Hiệu năng | Tra cứu tồn kho dưới 2 giây với 600 mã hàng (`NFR-01`); tối thiểu 30 người dùng đồng thời (`NFR-02`, giá trị đề xuất) | Bảng 3.2 |

## Hạn chế đã ghi nhận

- Báo cáo ghi rõ một số trạng thái ngoại lệ và quy trình đối soát liên nghiệp vụ cần hoàn thiện, kiểm thử; **ma trận truy vết giữa use case và test chưa tồn tại** (chỉ được nhắc ở Chương 7).
- Các ngưỡng `BR-04` (30 ngày) và `NFR-02` (30 người dùng) chưa được xác nhận nên kết quả kiểm thử dựa trên chúng chỉ có giá trị tham khảo.

## Các mục sẽ bổ sung

- Chiến lược kiểm thử và phạm vi
- Test case và ma trận truy vết
- Kết quả thực thi và lỗi đã phát hiện
