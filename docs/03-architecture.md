# Kiến trúc hệ thống

> **Lưu ý:** nội dung khảo sát và luật nghiệp vụ là **kịch bản giả lập/đề xuất** nhằm phục vụ phân tích và thiết kế dự án, không phải khảo sát thực tế.

## Công nghệ đề xuất

<p align="center"><b>Bảng 2.4. Công nghệ dự kiến triển khai</b></p>

| Thành phần | Lựa chọn đề xuất | Lý do |
|---|---|---|
| Backend | Java Spring Boot (hoặc ASP.NET Core - tùy thế mạnh của nhóm) | Có sẵn cơ chế phân quyền theo vai trò (FR-12), hỗ trợ transaction - cần thiết khi trừ tồn kho nhiều lô cùng lúc trong UC-XK-01 |
| Cơ sở dữ liệu | PostgreSQL hoặc MySQL (quan hệ) | ERD ở Chương 4 có 22 thực thể ràng buộc khóa ngoại chặt chẽ (đặc biệt LO_HANG - TON_KHO - CT_PHIEU_XUAT); hệ quản trị CSDL quan hệ đảm bảo toàn vẹn dữ liệu khi nhiều giao dịch xuất/nhập kho diễn ra đồng thời - cơ sở dữ liệu phi quan hệ (NoSQL) không phù hợp cho bài toán này |
| Frontend | ReactJS (hoặc Vue.js) | Phù hợp giao diện nhiều bảng biểu, lọc/tra cứu tương tác (Chương 6) |
| Xác thực | JWT (JSON Web Token) | Đơn giản hóa việc phân biệt quyền giữa 6 tác nhân (NFR-03) |


## Xác thực và phân quyền

- Phân quyền theo vai trò cho 6 tác nhân (FR-12); mật khẩu lưu dạng băm (NFR-03); xác thực đề xuất dùng JWT.
- Tài khoản đại lý gắn với một đại lý qua `NGUOI_DUNG.ma_dai_ly` để đại lý chỉ xem đơn hàng và công nợ của chính mình; tài khoản nhân viên nội bộ để trống trường này (mục 4.3, xem [04-database.md](04-database.md)).
- Luồng tài khoản: đăng nhập (UC-HT-01), quên mật khẩu (UC-HT-02) và đổi mật khẩu (UC-HT-03), hai use case sau là `extend` của UC-HT-01; quản lý tài khoản/phân quyền (UC-QT-01) và nhật ký hệ thống (UC-QT-02) xem tại [02-requirements.md](02-requirements.md).
- Sao lưu dữ liệu tự động hằng ngày (NFR-05, UC-QT-03).

## Luồng nghiệp vụ và tương tác

## 5.4. Activity Diagram

```mermaid
flowchart TD
    A([Bắt đầu]) --> B[Đại lý mở chức năng Đặt hàng]
    B --> C[Hệ thống hiển thị danh mục hàng hóa<br/>kèm tồn khả dụng và hạn mức công nợ]
    C --> D[Đại lý chọn mã hàng, nhập số lượng]
    D --> E{Số lượng đặt<br/>≤ tồn khả dụng?}
    E -- Không --> E1[Hiển thị số lượng tối đa có thể đặt]
    E1 --> D
    E -- Có --> F[Hệ thống tính tổng giá trị đơn hàng]
    F --> G{"Công nợ chính thức + công nợ tạm tính<br/>+ max(0, giá trị đơn - trả trước khả dụng) ≤ hạn mức?"}
    G -- Không --> G1[Từ chối gửi đơn,<br/>hiển thị hạn mức còn lại]
    G1 --> H([Kết thúc])
    G -- Có --> I[Đại lý xác nhận gửi đơn]
    I --> J[Hệ thống tạo đơn ở trạng thái Chờ duyệt]
    J --> K["Giữ chỗ min(trả trước khả dụng, giá trị đơn); cộng max(0, giá trị đơn - trả trước đã phân bổ) vào cong_no_tam_tinh"]
    K --> M[Gửi thông báo cho Trưởng phòng Kinh doanh]
    M --> L([Kết thúc])
```
<p align="center"><i>Hình 5.8. Activity Diagram - Đại lý đặt hàng</i></p>


```mermaid
flowchart TD
    A([Bắt đầu]) --> B[Thủ kho chọn đơn hàng đã duyệt]
    B --> B1[Thủ kho chọn kho xuất; hệ thống hiển thị tồn Bán được của các kho]
    B1 --> C[Hệ thống liệt kê mã hàng và số lượng cần xuất]
    C --> C0[Chuẩn bị danh sách lô còn tồn Bán được tại kho đã chọn,<br/>sắp xếp theo hạn sử dụng tăng dần]
    C0 --> D[Chọn lô tiếp theo chưa xét trong danh sách]
    D --> D0{Còn lô chưa xét<br/>trong danh sách?}
    D0 -- Không --> X[Thông báo không đủ tồn khả dụng tại kho đã chọn;<br/>đề nghị xuất một phần hoặc điều chuyển/chọn kho khác]
    X --> X1([Kết thúc - chưa tạo phiếu xuất])
    D0 -- Có --> E{Lô tiếp theo<br/>đã hết hạn sử dụng?}
    E -- Có --> E1[Loại lô khỏi danh sách,<br/>cảnh báo thủ kho]
    E1 --> D
    E -- Không --> F[Phân bổ số lượng xuất từ lô này]
    F --> G{Đã đủ số lượng<br/>cần xuất?}
    G -- Chưa --> D
    G -- Đủ --> H[Hiển thị bảng đề xuất phân bổ lô]
    H --> I[Thủ kho xác nhận<br/>hoặc điều chỉnh lô]
    I --> J[Tạo phiếu xuất,<br/>trừ tồn kho từng lô, ghi nhật ký]
    J --> K[Cập nhật đơn hàng<br/>sang Đã xuất kho]
    K --> L([Kết thúc])
```
<p align="center"><i>Hình 5.9. Activity Diagram - Xuất kho theo FEFO</i></p>


```mermaid
flowchart TD
    A([Bắt đầu]) --> B{Đại lý đã gửi yêu cầu<br/>trả hàng online?}
    B -- Có --> C[Nhân viên kho chọn yêu cầu<br/>từ danh sách chờ]
    B -- Chưa --> D[Nhân viên kho nhập mã đơn hàng gốc]
    D --> E[Hệ thống hiển thị mã hàng,<br/>lô hàng đã giao trong đơn]
    C --> E
    E --> F[Chọn lô hàng trả, nhập số lượng,<br/>tình trạng hàng, lý do trả]
    F --> G{Số lượng nhận ≤ số lượng đã giao<br/>trừ số lượng đã trả/đã nhận trước đó?}
    G -- Không --> G1[Từ chối,<br/>hiển thị số lượng tối đa còn được trả]
    G1 --> F
    G -- Có --> H[Nhân viên kho xác nhận và chọn kho tiếp nhận]
    H --> I{Đã xác định lô và số lượng<br/>cho tất cả dòng hàng?}
    I -- Chưa --> I1[Ghi nhận các dòng đã đủ lô và số lượng vào Hàng lỗi;<br/>các dòng thiếu lô giữ trạng thái Chờ xác minh]
    I1 --> I2[Hoàn tất lần tiếp nhận hiện tại;<br/>lưu các dòng còn thiếu để xử lý tiếp sau]
    I2 --> L([Kết thúc lần tiếp nhận này])
    I -- Rồi --> J[Ghi ma_kho_tiep_nhan;<br/>cộng tồn Hàng lỗi cho các dòng đủ điều kiện; ghi nhật ký]
    J --> K[Gửi thông báo cho Kế toán công nợ<br/>chỉ với giá trị dòng đã xác minh đủ điều kiện]
    K --> L
```
<p align="center"><i>Hình 5.10. Activity Diagram - Ghi nhận hàng trả lại</i></p>


## 5.5. Sequence Diagram

```mermaid
sequenceDiagram
    actor DL as Đại lý
    participant HT as Hệ thống
    participant TK as CSDL Tồn kho
    participant CN as CSDL Công nợ

    DL->>HT: Mở chức năng Đặt hàng
    HT->>TK: Truy vấn tồn khả dụng
    TK-->>HT: Danh sách hàng + tồn
    HT->>CN: Truy vấn hạn mức và công nợ hiện tại
    CN-->>HT: Hạn mức còn lại
    HT-->>DL: Hiển thị danh mục kèm tồn và hạn mức
    DL->>HT: Chọn hàng, nhập số lượng, xác nhận gửi
    HT->>HT: Kiểm tra tồn khả dụng (EF-01)
    HT->>CN: Kiểm tra cong_no_hien_tai (BR-02)
    alt Vượt hạn mức
        CN-->>HT: Từ chối
        HT-->>DL: Thông báo vượt hạn mức (EF-02)
    else Trong hạn mức
        CN-->>HT: Đồng ý
        HT->>HT: Tạo đơn hàng, trạng thái Chờ duyệt
        HT->>CN: Giữ chỗ min(trả trước khả dụng, giá trị đơn), cộng phần còn phải thu vào cong_no_tam_tinh
        HT-->>DL: Xác nhận đã gửi đơn
        HT->>HT: Gửi thông báo cho Trưởng phòng Kinh doanh
    end
```
<p align="center"><i>Hình 5.11. Sequence Diagram - Đại lý đặt hàng</i></p>

```mermaid
sequenceDiagram
    actor TK as Thủ kho
    participant HT as Hệ thống
    participant LO as CSDL Lô hàng
    participant TON as CSDL Tồn kho
    participant NK as Nhật ký hệ thống

    TK->>HT: Chọn đơn hàng đã duyệt và kho xuất
    HT->>LO: Truy vấn các lô còn tồn Bán được tại kho đã chọn,<br/>sắp xếp theo hạn sử dụng (BR-01)
    LO-->>HT: Danh sách lô kèm hạn sử dụng
    HT->>HT: Phân bổ số lượng xuất theo FEFO
    HT-->>TK: Hiển thị bảng đề xuất phân bổ lô
    TK->>HT: Xác nhận (hoặc điều chỉnh lô - AF-01)
    HT->>TON: Trừ tồn kho từng lô tương ứng
    TON-->>HT: Xác nhận đã cập nhật
    HT->>NK: Ghi nhật ký người thực hiện, thời điểm (BR-05)
    HT->>HT: Cập nhật đơn hàng sang Đã xuất kho
    HT-->>TK: Xác nhận tạo phiếu xuất thành công
```
<p align="center"><i>Hình 5.12. Sequence Diagram - Xuất kho theo FEFO</i></p>


<!-- ## Thiết kế giao diện

Giao diện được thiết kế theo nguyên tắc mỗi vai trò chỉ thấy các chức năng thuộc quyền của mình (theo FR-12).

**Giao diện đăng nhập.** Form nhập tên đăng nhập và mật khẩu, sau khi xác thực thành công hệ thống điều hướng đến dashboard tương ứng vai trò.

*Hình 6.1. Giao diện đăng nhập*

**Giao diện đặt hàng của đại lý.** Hiển thị danh mục hàng hóa kèm tồn khả dụng, và đặc biệt là **thanh hạn mức công nợ còn lại** đặt ở vị trí dễ thấy - giải quyết trực tiếp phản ánh của đại lý trong khảo sát rằng họ không biết hạn mức còn lại cho đến khi bị từ chối đơn.

*Hình 6.2. Giao diện đặt hàng của đại lý*

**Giao diện phiếu xuất kho.** Hiển thị bảng đề xuất phân bổ lô hàng theo FEFO, mỗi dòng ghi rõ mã lô và hạn sử dụng để thủ kho đối chiếu trước khi xác nhận.

*Hình 6.3. Giao diện phiếu xuất kho*

**Giao diện theo dõi công nợ đại lý.** Bảng danh sách 200 đại lý kèm hạn mức, công nợ hiện tại và tỷ lệ sử dụng hạn mức, có tô màu cảnh báo khi đại lý gần chạm hạn mức.

*Hình 6.4. Giao diện theo dõi công nợ đại lý*

**Giao diện báo cáo tồn kho hợp nhất.** Hiển thị tồn kho của 600 mã hàng theo từng kho và cột tổng hợp 3 kho, có bộ lọc theo hàng cận hạn sử dụng.

*Hình 6.5. Giao diện báo cáo tồn kho hợp nhất*

> Báo cáo nguồn chỉ có chú thích Hình 6.1–6.5, **chưa có file ảnh hay wireframe** đi kèm. Thiết kế giao diện dự kiến dùng Figma. -->