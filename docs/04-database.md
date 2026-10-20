# Thiết kế cơ sở dữ liệu

> **Lưu ý:** nội dung khảo sát và luật nghiệp vụ là **kịch bản giả lập/đề xuất** nhằm phục vụ phân tích và thiết kế dự án, không phải khảo sát thực tế.

## 4.1. Sơ đồ ERD

```mermaid
erDiagram
    VAI_TRO ||--o{ NGUOI_DUNG : "gán cho"
    NHA_CUNG_CAP ||--o{ HANG_HOA : "cung cấp"
    HANG_HOA ||--o{ LO_HANG : "có nhiều lô"
    KHO ||--o{ TON_KHO : "chứa"
    LO_HANG ||--o{ TON_KHO : "nằm ở nhiều kho"
    NHA_CUNG_CAP ||--o{ PHIEU_NHAP : "giao hàng theo"
    KHO ||--o{ PHIEU_NHAP : "nhận hàng tại"
    NGUOI_DUNG ||--o{ PHIEU_NHAP : "tạo"
    NGUOI_DUNG o|--o{ PHIEU_NHAP : "duyệt"
    PHIEU_NHAP ||--o{ CT_PHIEU_NHAP : "gồm"
    LO_HANG ||--|| CT_PHIEU_NHAP : "phát sinh từ"
    DAI_LY ||--o{ DON_HANG : "đặt"
    DON_HANG ||--o{ CT_DON_HANG : "gồm"
    HANG_HOA ||--o{ CT_DON_HANG : "thuộc về"
    DON_HANG ||--o{ PHIEU_XUAT : "xuất theo"
    KHO ||--o{ PHIEU_XUAT : "xuất từ"
    NGUOI_DUNG ||--o{ PHIEU_XUAT : "tạo"
    PHIEU_XUAT ||--o{ CT_PHIEU_XUAT : "gồm"
    LO_HANG ||--o{ CT_PHIEU_XUAT : "xuất từ lô"
    DAI_LY ||--o{ CONG_NO : "phát sinh"
    DON_HANG o|--o{ CONG_NO : "làm căn cứ ghi (tùy chọn)"
    PHIEU_XUAT o|--o| CONG_NO : "ghi nợ khi giao thành công"
    PHIEU_TRA_HANG o|--o| CONG_NO : "điều chỉnh công nợ"
    DAI_LY ||--o{ PHIEU_TRA_HANG : "gửi yêu cầu"
    DON_HANG ||--o{ PHIEU_TRA_HANG : "thuộc về"
    PHIEU_TRA_HANG ||--o{ CT_TRA_HANG : "gồm"
    LO_HANG o|--o{ CT_TRA_HANG : "xác định khi tiếp nhận"
    HANG_HOA ||--o{ CT_TRA_HANG : "xác định mặt hàng trả"
    KHO o|--o{ PHIEU_TRA_HANG : "tiếp nhận tại (sau khi nhận hàng)"
    KHO ||--o{ PHIEU_KIEM_KE : "kiểm kê"
    NGUOI_DUNG ||--o{ PHIEU_KIEM_KE : "tạo"
    PHIEU_KIEM_KE ||--o{ CT_KIEM_KE : "gồm"
    LO_HANG ||--o{ CT_KIEM_KE : "kiểm theo lô"
    NGUOI_DUNG ||--o{ NHAT_KY : "sinh ra"
    DAI_LY o|--o{ NGUOI_DUNG : "có tài khoản"
    NGUOI_DUNG o|--o{ PHIEU_KIEM_KE : "duyệt"
    KHO ||--o{ PHIEU_DIEU_CHUYEN : "chuyển đi (kho nguồn)"
    KHO ||--o{ PHIEU_DIEU_CHUYEN : "chuyển đến (kho đích)"
    NGUOI_DUNG ||--o{ PHIEU_DIEU_CHUYEN : "tạo"
    PHIEU_DIEU_CHUYEN ||--o{ CT_DIEU_CHUYEN : "gồm"
    LO_HANG ||--o{ CT_DIEU_CHUYEN : "điều chuyển theo lô"

    NGUOI_DUNG {
        VARCHAR(30) ma_nguoi_dung PK
        VARCHAR(50) ten_dang_nhap
        VARCHAR(255) mat_khau_bam
        VARCHAR(30) ma_vai_tro FK
        VARCHAR(30) ma_dai_ly FK "nullable"
        VARCHAR(20) trang_thai
    }
    VAI_TRO {
        VARCHAR(30) ma_vai_tro PK
        VARCHAR(50) ten_vai_tro
        TEXT mo_ta_quyen
    }
    DAI_LY {
        VARCHAR(30) ma_dai_ly PK
        VARCHAR(150) ten_dai_ly
        VARCHAR(255) dia_chi
        VARCHAR(100) khu_vuc
        DECIMAL(15,2) han_muc_cong_no
        DECIMAL(15,2) cong_no_hien_tai
        DECIMAL(15,2) cong_no_tam_tinh
        DECIMAL(15,2) so_du_tra_truoc
        VARCHAR(20) trang_thai
    }
    NHA_CUNG_CAP {
        VARCHAR(30) ma_ncc PK
        VARCHAR(150) ten_ncc
        VARCHAR(255) dia_chi
        VARCHAR(20) dien_thoai
    }
    HANG_HOA {
        VARCHAR(30) ma_hang PK
        VARCHAR(150) ten_hang
        VARCHAR(30) don_vi_tinh
        VARCHAR(30) ma_ncc FK
        DECIMAL(15,2) don_gia_ban
        VARCHAR(20) trang_thai
    }
    KHO {
        VARCHAR(30) ma_kho PK
        VARCHAR(100) ten_kho
        VARCHAR(255) dia_chi
        VARCHAR(30) loai_kho
    }
    LO_HANG {
        VARCHAR(30) ma_lo PK
        VARCHAR(30) ma_hang FK
        DATE ngay_nhap
        DATE han_su_dung
        INT so_luong_ban_dau
    }
    TON_KHO {
        VARCHAR(30) ma_kho PK, FK
        VARCHAR(30) ma_lo PK, FK
        VARCHAR(30) trang_thai_ton PK
        INT so_luong_ton
    }
    PHIEU_NHAP {
        VARCHAR(30) ma_phieu_nhap PK
        VARCHAR(30) ma_ncc FK
        VARCHAR(30) ma_kho FK
        DATE ngay_nhap
        VARCHAR(30) ma_nguoi_tao FK
        VARCHAR(30) ma_nguoi_duyet FK "nullable"
        VARCHAR(20) trang_thai
    }
    CT_PHIEU_NHAP {
        VARCHAR(30) ma_phieu_nhap FK
        VARCHAR(30) ma_lo FK
        INT so_luong
        DECIMAL(15,2) don_gia_nhap
    }
    DON_HANG {
        VARCHAR(30) ma_don_hang PK
        VARCHAR(30) ma_dai_ly FK
        DATE ngay_dat
        DECIMAL(15,2) tong_tien
        DECIMAL(15,2) so_tien_tra_truoc_da_phan_bo
        VARCHAR(20) trang_thai
    }
    CT_DON_HANG {
        VARCHAR(30) ma_don_hang FK
        VARCHAR(30) ma_hang FK
        INT so_luong_dat
        DECIMAL(15,2) don_gia
    }
    PHIEU_XUAT {
        VARCHAR(30) ma_phieu_xuat PK
        VARCHAR(30) ma_don_hang FK
        VARCHAR(30) ma_kho FK
        DATE ngay_xuat
        VARCHAR(30) ma_nguoi_tao FK
        VARCHAR(20) trang_thai
        DATETIME ngay_giao "nullable"
    }
    CT_PHIEU_XUAT {
        VARCHAR(30) ma_phieu_xuat FK
        VARCHAR(30) ma_lo FK
        INT so_luong_xuat
    }
    CONG_NO {
        VARCHAR(30) ma_cong_no PK
        VARCHAR(30) ma_dai_ly FK
        VARCHAR(30) ma_don_hang FK "nullable"
        VARCHAR(30) ma_phieu_xuat FK "nullable, UNIQUE"
        VARCHAR(30) ma_phieu_tra FK "nullable, UNIQUE"
        VARCHAR(40) loai_giao_dich "Ghi nợ / Thanh toán / Điều chỉnh giảm (hàng trả) / Nộp trước"
        VARCHAR(30) hinh_thuc_thanh_toan "nullable"
        DECIMAL(15,2) so_tien
        DATETIME ngay_ghi_nhan
    }
    PHIEU_TRA_HANG {
        VARCHAR(30) ma_phieu_tra PK
        VARCHAR(30) ma_dai_ly FK
        VARCHAR(30) ma_don_hang FK
        VARCHAR(30) ma_kho_tiep_nhan FK "nullable trước khi tiếp nhận"
        DATE ngay_tra
        VARCHAR(500) ly_do
        VARCHAR(20) trang_thai
        VARCHAR(30) trang_thai_cong_no
    }
    CT_TRA_HANG {
        VARCHAR(30) ma_phieu_tra FK
        VARCHAR(30) ma_hang FK
        VARCHAR(30) ma_lo FK "nullable khi chưa xác minh lô"
        INT so_luong_tra
        VARCHAR(30) tinh_trang_hang
        VARCHAR(100) phuong_an_xu_ly "nullable đến khi xử lý"
    }
    PHIEU_KIEM_KE {
        VARCHAR(30) ma_phieu_kk PK
        VARCHAR(30) ma_kho FK
        DATE ngay_kiem_ke
        VARCHAR(30) ma_nguoi_tao FK
        VARCHAR(20) trang_thai
        VARCHAR(30) ma_nguoi_duyet FK "nullable"
    }
    CT_KIEM_KE {
        VARCHAR(30) ma_phieu_kk FK
        VARCHAR(30) ma_lo FK
        INT so_luong_he_thong
        INT so_luong_thuc_te
        INT chenh_lech
        VARCHAR(255) ly_do_chenh_lech "nullable"
    }
    NHAT_KY {
        VARCHAR(30) ma_nhat_ky PK
        VARCHAR(30) ma_nguoi_dung FK
        VARCHAR(100) hanh_dong
        VARCHAR(100) doi_tuong
        DATETIME thoi_diem
    }
    PHIEU_DIEU_CHUYEN {
        VARCHAR(30) ma_phieu_dc PK
        VARCHAR(30) ma_kho_nguon FK
        VARCHAR(30) ma_kho_dich FK
        DATE ngay_dieu_chuyen
        VARCHAR(30) ma_nguoi_tao FK
        VARCHAR(20) trang_thai
    }
    CT_DIEU_CHUYEN {
        VARCHAR(30) ma_phieu_dc FK
        VARCHAR(30) ma_lo FK
        INT so_luong
    }
```

<p align="center"><i>Hình 4.1. Sơ đồ ERD hệ thống quản lý kho và phân phối (đầy đủ 22 thực thể)</i></p>

## 4.2. Mô tả các thực thể

<p align="center"><b>Bảng 4.1. Mô tả các thực thể</b></p>

| Thực thể | Mô tả | Thuộc tính chính |
|---|---|---|
| NGUOI_DUNG | Tài khoản truy cập hệ thống | ma_nguoi_dung, ten_dang_nhap, mat_khau_bam, ma_vai_tro, ma_dai_ly (nullable - chỉ có ở tài khoản đại lý), trang_thai |
| VAI_TRO | Vai trò phân quyền | ma_vai_tro, ten_vai_tro, mo_ta_quyen |
| DAI_LY | Đại lý bán lẻ nhận hàng | ma_dai_ly, ten_dai_ly, dia_chi, khu_vuc, han_muc_cong_no, cong_no_hien_tai (công nợ chính thức), cong_no_tam_tinh (tổng giá trị phần hàng đã gửi nhưng chưa giao thành công), so_du_tra_truoc, trang_thai |
| NHA_CUNG_CAP | Nhà cung cấp hàng hóa | ma_ncc, ten_ncc, dia_chi, dien_thoai |
| HANG_HOA | Danh mục 600 mã hàng | ma_hang, ten_hang, don_vi_tinh, ma_ncc, don_gia_ban, trang_thai (đang kinh doanh / ngừng kinh doanh) |
| KHO | 3 kho của nhà phân phối | ma_kho, ten_kho, dia_chi, loai_kho |
| LO_HANG | Lô hàng theo từng lần nhập, mang hạn sử dụng riêng | ma_lo, ma_hang, ngay_nhap, han_su_dung, so_luong_ban_dau |
| TON_KHO | Tồn kho của một lô hàng tại một kho, tách theo khu vực: "Bán được" hoặc "Hàng lỗi" (hàng trả lại chờ xử lý theo BR-03) | ma_kho, ma_lo, trang_thai_ton, so_luong_ton |
| PHIEU_NHAP | Phiếu nhập hàng từ nhà cung cấp | ma_phieu_nhap, ma_ncc, ma_kho, ngay_nhap, ma_nguoi_tao, ma_nguoi_duyet (nullable), trang_thai |
| CT_PHIEU_NHAP | Chi tiết phiếu nhập | ma_phieu_nhap, ma_lo, so_luong, don_gia_nhap |
| DON_HANG | Đơn hàng của đại lý | ma_don_hang, ma_dai_ly, ngay_dat, tong_tien, so_tien_tra_truoc_da_phan_bo, trang_thai |
| CT_DON_HANG | Chi tiết đơn hàng | ma_don_hang, ma_hang, so_luong_dat, don_gia |
| PHIEU_XUAT | Phiếu xuất kho sinh từ đơn hàng | ma_phieu_xuat, ma_don_hang, ma_kho, ngay_xuat, ma_nguoi_tao, trang_thai (Đã xuất kho / Đã giao / Giao thất bại / Đã hủy / Đã hoàn kho), ngay_giao (nullable) |
| CT_PHIEU_XUAT | Chi tiết phiếu xuất, gắn với lô hàng cụ thể | ma_phieu_xuat, ma_lo, so_luong_xuat |
| CONG_NO | Biến động công nợ của đại lý (ghi nợ, thanh toán, điều chỉnh giảm hàng trả, nộp trước) | ma_cong_no, ma_dai_ly, ma_don_hang (nullable), ma_phieu_xuat (nullable, UNIQUE), ma_phieu_tra (nullable, UNIQUE), loai_giao_dich ("Ghi nợ" / "Thanh toán" / "Điều chỉnh giảm (hàng trả)" / "Nộp trước"), hinh_thuc_thanh_toan (nullable), so_tien, ngay_ghi_nhan |
| PHIEU_TRA_HANG | Yêu cầu trả hàng từ đại lý | ma_phieu_tra, ma_dai_ly, ma_don_hang, ma_kho_tiep_nhan (nullable trước khi tiếp nhận), ngay_tra, ly_do, trang_thai, trang_thai_cong_no ("Chờ xác nhận công nợ" / "Đã điều chỉnh công nợ" / "Từ chối điều chỉnh") |
| CT_TRA_HANG | Chi tiết hàng trả lại; ma_hang bắt buộc, ma_lo nullable đến khi xác minh lô; phương án xử lý được xác định theo từng dòng | ma_phieu_tra, ma_hang, ma_lo (nullable), so_luong_tra, tinh_trang_hang, phuong_an_xu_ly (nullable) |
| PHIEU_KIEM_KE | Phiếu kiểm kê kho định kỳ | ma_phieu_kk, ma_kho, ngay_kiem_ke, ma_nguoi_tao, trang_thai (Chờ duyệt / Đã điều chỉnh tồn / Từ chối), ma_nguoi_duyet (nullable) |
| CT_KIEM_KE | Chi tiết kiểm kê tồn bán được, ghi chênh lệch | ma_phieu_kk, ma_lo, so_luong_he_thong, so_luong_thuc_te, chenh_lech, ly_do_chenh_lech (nullable). Quy ước cố định: mỗi dòng đối chiếu TON_KHO với trang_thai_ton="Bán được"; không kiểm kê khu vực "Hàng lỗi" trong UC-TK-03. |
| NHAT_KY | Nhật ký thao tác hệ thống | ma_nhat_ky, ma_nguoi_dung, hanh_dong, doi_tuong, thoi_diem |
| PHIEU_DIEU_CHUYEN | Phiếu điều chuyển lô hàng giữa hai kho (chỉ tồn "Bán được") | ma_phieu_dc, ma_kho_nguon, ma_kho_dich, ngay_dieu_chuyen, ma_nguoi_tao, trang_thai |
| CT_DIEU_CHUYEN | Chi tiết điều chuyển theo lô | ma_phieu_dc, ma_lo, so_luong |

**Lưu ý về thiết kế:** thực thể `LO_HANG` tách riêng khỏi `HANG_HOA` vì: Nếu gộp tồn kho trực tiếp vào `HANG_HOA` thì hệ thống chỉ biết "mã hàng X còn 500 đơn vị" mà không biết 500 đơn vị đó thuộc những lô nào với hạn sử dụng nào - khi đó luật BR-01 (xuất theo FEFO) và BR-04 (cảnh báo cận hạn) đều không thể triển khai được.

**Quy ước về trạng thái tồn kho và công nợ.**
- *Khu vực tồn kho:* hàng lỗi/hàng trả lại không nằm ở kho vật lý thứ tư mà là một khu vực trong từng kho, đánh dấu bằng `TON_KHO.trang_thai_ton` ("Bán được" hoặc "Hàng lỗi"). Phiếu trả hàng lưu `ma_kho_tiep_nhan` sau khi kho nhận hàng; trước thời điểm tiếp nhận, trường này được để NULL. Mọi nghiệp vụ bán hàng (đặt hàng, xuất kho FEFO, điều chuyển, báo cáo tồn bán được) chỉ tính tồn "Bán được"; báo cáo "3 kho" vẫn là 3 kho vật lý.
- *Công nợ:* `DAI_LY.cong_no_tam_tinh` là tổng giá trị phần hàng đã gửi nhưng chưa giao thành công; `DAI_LY.cong_no_hien_tai` là công nợ chính thức. Khi một phiếu xuất giao thành công, hệ thống tạo giao dịch `CONG_NO` loại "Ghi nợ", tăng công nợ chính thức và giảm công nợ tạm tính đúng giá trị hàng của phiếu xuất đó (UC-XK-02/UC-CN-05); giao thất bại không tạo ghi nợ và không tự giải phóng phần tạm tính. Khi đơn bị từ chối/hủy, chỉ phần tạm tính chưa giao thành công mới được giải phóng. Hàng trả chỉ làm giảm công nợ chính thức sau khi kế toán xác nhận (UC-CN-04). Công nợ tạm tính được lưu dưới dạng tổng tại `DAI_LY.cong_no_tam_tinh`, nhưng giá trị đối soát bắt buộc được tính theo từng đơn và từng phiếu xuất: phần của đơn đã giao thành công được chuyển sang công nợ chính thức; phần phiếu giao thất bại chỉ được giải phóng sau khi hàng đã hoàn kho; phần đơn chưa xuất được giải phóng khi hủy. Không được giải phóng toàn bộ giá trị đơn nếu đơn đã có phiếu giao thành công, và mỗi phần giá trị chỉ được chuyển/giải phóng một lần. Không tạo bản ghi sổ cái `CONG_NO` cho đến khi phát sinh giao dịch chính thức. Kiểm tra hạn mức (BR-02, UC-DH-03): `cong_no_hien_tai + cong_no_tam_tinh + max(0, giá_trị_đơn_mới - số_dư_trả_trước_chưa_phân_bổ) ≤ han_muc_cong_no`. Số dư trả trước phải được phân bổ/giữ chỗ nguyên tử cho đơn khi gửi đơn để không dùng trùng cho nhiều đơn; theo dõi số đã phân bổ ở cấp đơn hàng.

## 4.3. Mô tả các mối quan hệ

<p align="center"><b>Bảng 4.2. Mô tả các mối quan hệ</b></p>

| Quan hệ | Lực lượng | Ý nghĩa |
|---|---|---|
| VAI_TRO - NGUOI_DUNG | 1 - n | Một vai trò gán cho nhiều người dùng; mỗi người dùng thuộc đúng một vai trò |
| NHA_CUNG_CAP - HANG_HOA | 1 - n | Một nhà cung cấp cung cấp nhiều mã hàng |
| HANG_HOA - LO_HANG | 1 - n | Một mã hàng có nhiều lô nhập ở các thời điểm khác nhau |
| KHO - TON_KHO | 1 - n | Một kho chứa tồn của nhiều lô hàng; khóa chính (ma_kho, ma_lo, trang_thai_ton) vì cùng một lô tại cùng một kho có thể vừa có tồn "Bán được" vừa có tồn "Hàng lỗi" |
| LO_HANG - TON_KHO | 1 - n | Một lô hàng có thể nằm ở nhiều kho sau khi điều chuyển, và ở hai khu vực (Bán được / Hàng lỗi) trong cùng một kho |
| NHA_CUNG_CAP - PHIEU_NHAP | 1 - n | Một nhà cung cấp giao hàng qua nhiều phiếu nhập |
| KHO - PHIEU_NHAP | 1 - n | Một kho nhận hàng từ nhiều phiếu nhập |
| NGUOI_DUNG (người tạo) - PHIEU_NHAP | 1 - n | Một người dùng có thể tạo nhiều phiếu nhập; mỗi phiếu nhập có đúng một người tạo |
| NGUOI_DUNG (người duyệt) - PHIEU_NHAP | 0..1 - n | Một phiếu nhập chưa duyệt chưa có người duyệt; một người dùng có thể duyệt nhiều phiếu |
| PHIEU_NHAP - CT_PHIEU_NHAP | 1 - n | Một phiếu nhập gồm nhiều dòng chi tiết |
| LO_HANG - CT_PHIEU_NHAP | 1 - 1 | Mỗi dòng chi tiết phiếu nhập tạo đúng một lô; mỗi lô phát sinh từ đúng một dòng chi tiết; cần ràng buộc UNIQUE(CT_PHIEU_NHAP.ma_lo) |
| DAI_LY - DON_HANG | 1 - n | Một đại lý đặt nhiều đơn hàng |
| DON_HANG - CT_DON_HANG | 1 - n | Một đơn hàng gồm nhiều mã hàng |
| DON_HANG - PHIEU_XUAT | 1 - n | Một đơn hàng có thể xuất làm nhiều lần từ nhiều kho |
| KHO - PHIEU_XUAT | 1 - n | Một kho xuất hàng qua nhiều phiếu xuất |
| NGUOI_DUNG (người tạo) - PHIEU_XUAT | 1 - n | Một người dùng có thể tạo nhiều phiếu xuất; mỗi phiếu xuất có đúng một người tạo |
| PHIEU_XUAT - CT_PHIEU_XUAT | 1 - n | Một phiếu xuất gồm nhiều dòng, mỗi dòng gắn một lô hàng cụ thể |
| DAI_LY - CONG_NO | 1 - n | Một đại lý có nhiều biến động công nợ |
| DON_HANG - CONG_NO | 0..1 - n | Bản ghi công nợ có thể không gắn với đơn hàng cụ thể (thanh toán tổng hợp) |
| PHIEU_XUAT - CONG_NO | 0..1 - 0..1 | Một phiếu xuất có tối đa một biến động ghi nợ; một bản ghi công nợ chỉ gắn tối đa một phiếu xuất; cần UNIQUE(CONG_NO.ma_phieu_xuat); bản ghi này chỉ phát sinh khi phiếu xuất được xác nhận giao thành công (UC-CN-05) |
| PHIEU_TRA_HANG - CONG_NO | 0..1 - 0..1 | Một phiếu trả hàng có tối đa một bản ghi điều chỉnh công nợ; cần UNIQUE(CONG_NO.ma_phieu_tra) |
| DAI_LY - PHIEU_TRA_HANG | 1 - n | Một đại lý có thể gửi nhiều yêu cầu trả hàng |
| KHO - PHIEU_TRA_HANG (kho tiếp nhận) | 0..1 - n | Khi mới gửi yêu cầu, phiếu chưa gắn kho; khi nhân viên kho tiếp nhận, ma_kho_tiep_nhan được gán một kho vật lý trong 3 kho hiện có |
| DON_HANG - PHIEU_TRA_HANG | 1 - n | Một đơn hàng có thể phát sinh nhiều lần trả hàng |
| PHIEU_TRA_HANG - CT_TRA_HANG | 1 - n | Một phiếu trả hàng gồm nhiều dòng chi tiết |
| LO_HANG - CT_TRA_HANG | 0..1 - n | ma_lo nullable: đại lý gửi yêu cầu chưa cần xác định lô; nhân viên kho xác định lô khi tiếp nhận; chỉ khi ma_lo đã xác định và kho tiếp nhận đã được gán mới ghi được hàng vào TON_KHO (khu vực "Hàng lỗi") |
| KHO - PHIEU_KIEM_KE | 1 - n | Một kho được kiểm kê nhiều lần |
| PHIEU_KIEM_KE - CT_KIEM_KE | 1 - n | Một phiếu kiểm kê gồm nhiều dòng chi tiết theo lô |
| NGUOI_DUNG (người tạo) - PHIEU_KIEM_KE | 1 - n | Mỗi phiếu kiểm kê có đúng một người tạo |
| NGUOI_DUNG - NHAT_KY | 1 - n | Một người dùng sinh nhiều bản ghi nhật ký |
| DAI_LY - NGUOI_DUNG | 0..1 - n | Tài khoản đại lý gắn với đúng một đại lý (ma_dai_ly); tài khoản nhân viên nội bộ để trống. Đây là cơ sở để đại lý chỉ xem được đơn hàng và công nợ của chính mình (NFR-03) |
| NGUOI_DUNG (người duyệt) - PHIEU_KIEM_KE | 0..1 - n | Phiếu kiểm kê chưa duyệt chưa có người duyệt; một người dùng có thể duyệt nhiều phiếu |
| KHO (kho nguồn) - PHIEU_DIEU_CHUYEN | 1 - n | Một kho là nguồn của nhiều phiếu điều chuyển |
| KHO (kho đích) - PHIEU_DIEU_CHUYEN | 1 - n | Một kho là đích của nhiều phiếu điều chuyển; kho nguồn phải khác kho đích |
| NGUOI_DUNG (người tạo) - PHIEU_DIEU_CHUYEN | 1 - n | Mỗi phiếu điều chuyển có đúng một người tạo |
| PHIEU_DIEU_CHUYEN - CT_DIEU_CHUYEN | 1 - n | Một phiếu điều chuyển gồm nhiều dòng chi tiết theo lô |
| LO_HANG - CT_DIEU_CHUYEN | 1 - n | Một lô hàng có thể được điều chuyển nhiều lần |

> **Quy ước lực lượng quan hệ:** `1 - n` nghĩa là bắt buộc hai đầu; `0..1` ở một đầu nghĩa là tùy chọn (khóa ngoại được phép NULL). Khi cùng hai thực thể có nhiều khóa ngoại (nhiều vai trò), mỗi vai trò được tách thành một dòng riêng. Lực lượng cụ thể ở từng đầu được xác định bởi ký hiệu crow's-foot trong ERD [Hình 4.1](#41-sơ-đồ-erd).
