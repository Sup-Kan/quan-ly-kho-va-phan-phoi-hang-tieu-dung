# Tổng quan dự án

## 1. Giới thiệu

**Hệ thống Quản lý kho và Phân phối hàng tiêu dùng** là đề tài phân tích
và thiết kế hệ thống thông tin dành cho một nhà phân phối hàng tiêu dùng
cấp 1. Dự án hướng đến việc thay thế phương thức quản lý chủ yếu bằng
Excel bằng một hệ thống web tập trung, hỗ trợ quản lý hàng hóa, hoạt
động của nhiều kho, đơn đặt hàng từ đại lý và công nợ.

Bối cảnh nghiệp vụ dùng trong bản phân tích hiện tại gồm:

-   **3 kho hàng** cần theo dõi và phối hợp tồn kho.
-   Khoảng **600 mã hàng hóa**.
-   Khoảng **200 đại lý bán lẻ**.
-   Dữ liệu tồn kho và công nợ hiện được mô tả là quản lý bằng các tệp Excel riêng, gây khó khăn khi đối chiếu và tổng hợp.

> **Trạng thái thông tin:** Các số liệu và tình huống khảo sát trong tài liệu nguồn là bối cảnh/kịch bản giả lập phục vụ phân tích ban đầu, chưa được xác nhận bằng khảo sát thực địa. 
> Cần xác minh với đơn vị sử dụng trước khi xem đây là yêu cầu chính thức.

## 2. Bối cảnh và vấn đề cần giải quyết

Theo kịch bản nghiệp vụ trong tài liệu phân tích, phương thức quản lý hiện tại có một số hạn chế chính:

1.  **Khó đảm bảo tính nhất quán của tồn kho:** Dữ liệu được cập nhật bằng Excel, có thể xảy ra nhập thiếu, nhập sai hoặc cập nhật chậm giữa các kho.
2.  **Chưa áp dụng nhất quán nguyên tắc FEFO:** Hàng có hạn sử dụng gần hơn có thể bị tồn đọng trong khi hàng cùng mã ở nơi khác đã hết.
3.  **Theo dõi công nợ còn thủ công:** Hạn mức của từng đại lý được theo dõi riêng, thiếu cơ chế kiểm tra tự động trước khi tiếp nhận đơn hàng.
4.  **Thiếu số liệu tổng hợp kịp thời:** Báo cáo tồn kho hợp nhất có thể bị trễ và khó đối chiếu giữa các kho.
5.  **Khó truy vết thay đổi:** Khi dữ liệu được chia sẻ qua tệp Excel, việc xác định ai đã sửa dữ liệu và sửa vào thời điểm nào có thể không đầy đủ.

Hệ thống được đề xuất nhằm tập trung dữ liệu và chuẩn hóa các quy trình nghiệp vụ nêu trên.

## 3. Mục tiêu dự án

Các mục tiêu chính của hệ thống:

-   Quản lý tập trung danh mục hàng hóa, đại lý, kho và nhà cung cấp.
-   Theo dõi tồn kho theo từng kho và tổng hợp số liệu trên toàn hệ thống.
-   Quản lý nhập kho theo từng lô hàng, lưu hạn sử dụng để hỗ trợ xuất kho theo **FEFO** (*First Expired, First Out* - tức lô có hạn sử dụng gần nhất được ưu tiên xuất trước).
-   Quản lý quy trình đặt hàng, duyệt đơn, xuất kho và xác nhận giao hàng.
-   Kiểm tra hạn mức công nợ của đại lý trước khi tiếp nhận đơn vào trạng thái chờ duyệt.
-   Theo dõi thanh toán, công nợ phát sinh và điều chỉnh công nợ liên quan đến hàng trả lại.
-   Tiếp nhận hàng trả lại/hàng lỗi và tách chúng khỏi tồn kho bán được cho đến khi có quyết định xử lý.
-   Hỗ trợ kiểm kê, điều chuyển hàng giữa các kho và cảnh báo hàng cận hạn.
-   Cung cấp báo cáo tồn kho, công nợ và doanh số.
-   Quản lý tài khoản, phân quyền theo vai trò và ghi nhật ký các thao tác quan trọng.

## 4. Phạm vi hệ thống

### 4.1. Trong phạm vi

Hệ thống dự kiến bao gồm các nhóm chức năng:

1.  **Quản lý danh mục:** Hàng hóa, đại lý, kho và nhà cung cấp.
2.  **Quản lý nhập kho:** Lập và duyệt phiếu nhập; ghi nhận lô hàng và hạn sử dụng.
3.  **Quản lý đơn hàng và xuất kho:** Tiếp nhận đơn, kiểm tra điều kiện, duyệt đơn, xuất kho theo FEFO, xác nhận giao hàng và xử lý hủy đơn.
4.  **Quản lý tồn kho:** Tra cứu tồn theo kho, tồn hợp nhất, kiểm kê, điều chuyển và cảnh báo hàng cận hạn.
5.  **Quản lý công nợ:** Thiết lập hạn mức, tra cứu công nợ, ghi nhận thanh toán và điều chỉnh liên quan.
6.  **Quản lý hàng trả lại/hàng lỗi:** Tiếp nhận yêu cầu trả hàng, ghi nhận hàng vào khu vực/trạng thái hàng lỗi và theo dõi quyết định xử lý.
7.  **Báo cáo thống kê:** Báo cáo tồn kho, công nợ đại lý và doanh số theo mã hàng.
8.  **Quản trị hệ thống:** Đăng nhập, quên/đổi mật khẩu, quản lý tài khoản, phân quyền, nhật ký hệ thống và sao lưu/phục hồi dữ liệu.

### 4.2. Ngoài phạm vi

Theo tài liệu phân tích hiện tại, hệ thống **không bao gồm**:

-   Quản lý vận tải và tối ưu định tuyến giao hàng.
-   Quản lý nhân sự và tính lương.
-   Tích hợp với phần mềm kế toán tổng thể của doanh nghiệp.

Các nội dung ngoài phạm vi chỉ được xem xét nếu có yêu cầu thay đổi được
thống nhất sau này.

## 5. Đối tượng sử dụng

Hệ thống xác định sáu tác nhân chính:

| Mã | Tác nhân | Vai trò chính |
|---|---|---|
| `AC-01` | Đại lý | Đặt hàng, tra cứu công nợ và gửi yêu cầu trả hàng |
| `AC-02` | Nhân viên kho | Thực hiện nghiệp vụ nhập và xuất kho theo phiếu |
| `AC-03` | Thủ kho | Duyệt phiếu, chủ trì kiểm kê, quản lý lô hàng và hạn sử dụng |
| `AC-04` | Kế toán công nợ | Theo dõi công nợ, hạn mức tín dụng và ghi nhận thanh toán |
| `AC-05` | Trưởng phòng Kinh doanh | Duyệt hạn mức theo phân quyền, xem báo cáo tồn kho và doanh số |
| `AC-06` | Quản trị hệ thống | Quản lý tài khoản, phân quyền, nhật ký và sao lưu/phục hồi dữ liệu |

Quyền thao tác cụ thể của từng vai trò cần được quy định trong tài liệu
yêu cầu và thiết kế phân quyền; bảng trên chỉ tóm tắt trách nhiệm ở mức
tổng quan.

## 6. Nguyên tắc nghiệp vụ nổi bật

### 6.1. Xuất kho theo FEFO

- Hệ thống cần quản lý riêng từng lô hàng và hạn sử dụng. Khi xuất hàng,
hệ thống ưu tiên lô có hạn sử dụng gần nhất trong phạm vi kho và mã hàng
phù hợp, theo luật nghiệp vụ được đề xuất trong tài liệu nguồn.

### 6.2. Kiểm soát hạn mức công nợ

- Trước khi đơn hàng được tiếp nhận vào trạng thái chờ duyệt, hệ thống kiểm tra hạn mức của đại lý. 
Việc tính toán cần xét công nợ hiện tại, nghĩa vụ công nợ tạm tính và số dư trả trước khả dụng chưa 
được phân bổ đểtránh sử dụng trùng số dư. Công nợ chính thức chỉ được ghi nhận theo nghiệp vụ giao 
hàng thành công và các quy tắc cấn trừ tương ứng.

### 6.3. Tách hàng trả lại/hàng lỗi khỏi tồn kho bán được

- Hàng trả lại hoặc hàng lỗi phải được ghi nhận ở khu vực/trạng thái riêng, không tự động cộng vào lượng hàng có thể bán. Chỉ sau khi có quyết định xử lý phù hợp mới cập nhật trạng thái tồn kho.

### 6.4. Truy vết thay đổi tồn kho

- Các thao tác thay đổi số lượng tồn kho cần ghi nhận người thực hiện và thời điểm thực hiện để hỗ trợ kiểm tra, đối soát và kiểm toán nghiệp vụ.

### 6.5. Cảnh báo hàng cận hạn

- Báo cáo đề xuất ngưỡng cảnh báo **30 ngày trước hạn sử
dụng**.

## 7. Yêu cầu phi chức năng ở mức tổng quan

Các mục tiêu phi chức năng được đề xuất trong tài liệu nguồn:

-   **Hiệu năng:** thời gian phản hồi tra cứu tồn kho dưới 2 giây với quy mô 600 mã hàng.
-   **Đồng thời:** hỗ trợ tối thiểu 30 người dùng đồng thời.
-   **Bảo mật:** mật khẩu được lưu dưới dạng băm và quyền truy cập được phân theo vai trò.
-   **Truy vết:** ghi lại người thực hiện và thời điểm đối với thao tác thay đổi tồn kho.
-   **Sao lưu:** sao lưu dữ liệu tự động hằng ngày.
-   **Khả dụng:** sử dụng được trên các trình duyệt phổ biến, không yêu cầu người dùng cài đặt ứng dụng máy khách.

## 8. Định hướng công nghệ

Tài liệu nguồn đề xuất phát triển theo mô hình web với các lựa chọn công nghệ sau:

| Thành phầnphần | Định hướng đề xuất | Trạng thái |
|---|---|---|
| `Backend` | Java Spring Boot hoặc ASP.NET Core | Chưa chốt |            
| `Cơ sở dữ liệu` | PostgreSQL hoặc MySQL | Chưa chốt; định hướng sử dụng CSDL quan hệ |
| `Frontend` | ReactJS hoặc Vue.js | Chưa chốt |
| `Xác thực` | JWT (JSON Web Token) | Chưa chốt |

## 9. Phương pháp phân tích và thiết kế

Định hướng thực hiện gồm:

-   Khảo sát/phỏng vấn các nhóm tác nhân để xác minh quy trình và luật nghiệp vụ.
-   Phân tích hệ thống theo hướng đối tượng (OOAD), sử dụng UML để mô tả chức năng và hành vi.
-   Thiết kế mô hình dữ liệu quan hệ và các ràng buộc giữa thực thể.
-   Thiết kế giao diện người dùng trước khi triển khai.
-   Sử dụng Draw.io cho sơ đồ, Figma cho thiết kế giao diện và Git để quản lý phiên bản tài liệu.

## 10. Kết quả phân tích, thiết kế hiện có

Theo báo cáo, các sản phẩm phân tích và thiết kế đã được mô tả gồm:

-   Phân rã chức năng thành **8 nhóm chức năng**.
-   Mô hình ERD gồm **22 thực thể**.
-   Danh sách tổng thể gồm **34 Use Case**, kèm các đặc tả và sơ đồ hành vi liên quan trong báo cáo.
-   Đề xuất các luật nghiệp vụ trọng tâm về FEFO, kiểm soát công nợ, xử lý hàng trả lại/hàng lỗi, cảnh báo cận hạn và nhật ký thay đổi tồn kho.

## 11. Hạn chế và các điểm cần xác minh

Trước khi chốt yêu cầu và chuyển sang triển khai, cần:

1.  Khảo sát thực tế để xác nhận quy mô kho, hàng hóa, đại lý và quy trình đang vận hành.
2.  Xác minh các luật nghiệp vụ đang được xây dựng từ kịch bản giả lập, đặc biệt là cách tính/giữ chỗ số dư trả trước, ghi nhận công nợ khi giao hàng nhiều đợt và xử lý hàng trả lại.
3.  Xác định ngưỡng cảnh báo cận hạn phù hợp với từng nhóm hàng thay vì mặc định dùng chung 30 ngày.
4.  Xác nhận yêu cầu tải đồng thời và hiệu năng bằng dữ liệu vận hành hoặc kiểm thử tải.
5.  Chốt công nghệ, kiến trúc triển khai, cơ chế xác thực và phân quyền.
6.  Xây dựng và thực thi bộ kiểm thử cho các luồng chính, luồng ngoại lệ và các ràng buộc dữ liệu.
7.  Đối chiếu mô hình trạng thái nghiệp vụ với thiết kế cơ sở dữ liệu khi triển khai.

## 12. Tài liệu liên quan

-   [Yêu cầu chức năng, phi chức năng và luật nghiệp vụ.](02-requirements.md)
-   [Kiến trúc phần mềm và các thành phần triển khai](03-architecture.md)
-   [Mô hình dữ liệu, thực thể và quan hệ](04-database.md)
-   [Cấu trúc mã nguồn, cấu hình và hướng dẫn chạy hệ thống](05-implementation.md)
-   [Chiến lược, test case và kết quả kiểm thử](06-testing.md)
-   [Các lựa chọn thiết kế, lý do và trạng thái quyết định](07-decisions.md)

> Các liên kết trên trỏ đến những tài liệu dự kiến trong repository. Nếu
> một tài liệu chưa được tạo hoặc chưa có nội dung, hãy xem đó là phần
> đang chờ hoàn thiện.