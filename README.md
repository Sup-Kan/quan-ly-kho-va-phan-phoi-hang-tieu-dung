# Hệ thống Quản lý kho và Phân phối hàng tiêu dùng

Repository này lưu trữ tài liệu phân tích, thiết kế và các tài sản phát triển cho đề tài **Hệ thống Quản lý kho và Phân phối hàng tiêu dùng**. Mục tiêu là tổ chức tài liệu kỹ thuật thành các phần nhỏ, có liên kết rõ ràng, dễ cập nhật và quản lý bằng Git thay vì chỉ duy trì một báo cáo Markdown lớn.

> **Trạng thái hiện tại:** repository đang ở giai đoạn tổ chức tài liệu phân tích và thiết kế. Báo cáo nguồn đã có; các tài liệu trong `docs/` đang được tách và hoàn thiện. Không xem các chức năng, kiến trúc hoặc yêu cầu được mô tả là phần mềm đã triển khai hay đã kiểm thử nếu chưa có mã nguồn và bằng chứng tương ứng.

## Tổng quan dự án

Theo kịch bản trong báo cáo nguồn, hệ thống hướng tới một nhà phân phối hàng tiêu dùng cấp 1 với quy mô dự kiến:

- **3 kho hàng**.
- Khoảng **600 mã hàng hóa**.
- Khoảng **200 đại lý bán lẻ**.

Các số liệu và tình huống khảo sát trong báo cáo là bối cảnh giả lập phục vụ phân tích ban đầu, chưa được xác nhận qua khảo sát thực tế. Chúng cần được xác minh trước khi dùng làm yêu cầu chính thức.

### Mục tiêu chính

- Quản lý tập trung danh mục hàng hóa, kho, đại lý và nhà cung cấp.
- Theo dõi tồn kho theo kho và theo lô hàng; hỗ trợ nguyên tắc **FEFO** (*First Expired, First Out*).
- Quản lý đơn hàng, duyệt đơn, xuất kho và xác nhận giao hàng.
- Kiểm soát hạn mức và theo dõi công nợ đại lý, bao gồm thanh toán và số dư trả trước.
- Quản lý kiểm kê, điều chuyển kho, hàng trả lại/hàng lỗi và cảnh báo hàng cận hạn.
- Cung cấp báo cáo tồn kho, công nợ và doanh số.
- Quản lý tài khoản, phân quyền và nhật ký các thao tác quan trọng.

### Ngoài phạm vi dự kiến

- Quản lý vận tải và tối ưu tuyến giao hàng.
- Quản lý nhân sự và tính lương.
- Tích hợp với phần mềm kế toán tổng thể.

Chi tiết mục tiêu và phạm vi được trình bày trong [Tổng quan dự án](docs/01-overview.md).

## Điều hướng tài liệu

| Tài liệu | Nội dung |
|---|---|
| [Tổng quan dự án](docs/01-overview.md) | Bối cảnh, mục tiêu, phạm vi, tác nhân, nguyên tắc nghiệp vụ và định hướng công nghệ. |
| [Yêu cầu hệ thống](docs/02-requirements.md) | Yêu cầu chức năng, phi chức năng, luật nghiệp vụ và điều kiện chấp nhận. |
| [Kiến trúc hệ thống](docs/03-architecture.md) | Kiến trúc ứng dụng, thành phần, luồng dữ liệu, xác thực và phân quyền. |
| [Thiết kế cơ sở dữ liệu](docs/04-database.md) | ERD, thực thể, thuộc tính, khóa và các quan hệ dữ liệu. |
| [Triển khai](docs/05-implementation.md) | Cấu trúc mã nguồn, môi trường, cấu hình và hướng dẫn chạy ứng dụng. |
| [Kiểm thử](docs/06-testing.md) | Chiến lược kiểm thử, test case, kết quả thực thi và lỗi đã phát hiện. |
| [Nhật ký quyết định](docs/07-decisions.md) | Các quyết định thiết kế, phương án đã cân nhắc, lý do và trạng thái. |
| [Mục lục tài liệu](docs/README.md) | Điểm truy cập chung đến tài liệu trong thư mục `docs/`. |
| [Báo cáo phân tích nguồn](BaoCao_DT04_RaSoat_C.md) | Bản báo cáo tổng hợp hiện có, dùng làm nguồn để đối chiếu khi tách tài liệu. |

Một số tài liệu được liên kết ở trên có thể chưa có nội dung đầy đủ. Liên kết được tạo trước để thống nhất cấu trúc; hãy cập nhật trạng thái và nội dung từng tài liệu khi hoàn thành.

## Cấu trúc repository

```text
quan-ly-kho-va-phan-phoi-hang-tieu-dung/
├── README.md                         # Trang giới thiệu repository
├── BaoCao_DT04_RaSoat_C.md # Báo cáo nguồn tổng hợp (gọi tắt: BaoCao_DT04)
├── docs/
│   ├── README.md                     # Mục lục tài liệu kỹ thuật
│   ├── 01-overview.md                # Tổng quan dự án
│   ├── 02-requirements.md             # Yêu cầu hệ thống
│   ├── 03-architecture.md            # Kiến trúc hệ thống
│   ├── 04-database.md                # Thiết kế cơ sở dữ liệu
│   ├── 05-implementation.md          # Hướng dẫn triển khai
│   ├── 06-testing.md                 # Kiểm thử
│   ├── 07-decisions.md               # Nhật ký quyết định
│   ├── diagrams/                     # Sơ đồ Mermaid/Draw.io và tài sản sơ đồ
│   ├── images/                       # Ảnh minh họa, ảnh giao diện
│   └── schemas/                      # Schema, DDL và tài liệu dữ liệu
├── src/                              # Mã nguồn ứng dụng (khi được bổ sung)
└── tests/                            # Bộ kiểm thử (khi được bổ sung)
```

Các thư mục `src/`, `tests/`, `diagrams/`, `images/` và `schemas/` là vị trí tổ chức dự kiến; sự tồn tại của thư mục không có nghĩa là mã nguồn, kiểm thử hay tài sản đã được tạo đầy đủ.

## Định hướng công nghệ

Báo cáo nguồn đưa ra một số phương án ban đầu:

| Thành phần | Phương án đang xem xét | Trạng thái |
|---|---|---|
| Backend | Java Spring Boot hoặc ASP.NET Core | Chưa chốt |
| Cơ sở dữ liệu | PostgreSQL hoặc MySQL | Chưa chốt; định hướng dùng CSDL quan hệ |
| Frontend | ReactJS hoặc Vue.js | Chưa chốt |
| Xác thực | JWT (JSON Web Token) | Đề xuất, cần xác nhận trong thiết kế kiến trúc |

Không nên tạo hướng dẫn cài đặt hoặc lệnh chạy ứng dụng dựa trên các lựa chọn trên cho đến khi công nghệ thực tế được quyết định và cấu hình của dự án được thiết lập.