# GIẢI PHÁP HYBRID WORKFLOW VÀ VERSION HISTORY CHO DỰ ÁN "SỐ HÓA BẢO TÀNG PTIT"


## PHẦN 1: BÁO CÁO PHÂN TÍCH — LUỒNG DỮ LIỆU & XỬ LÝ XUNG ĐỘT PHIÊN BẢN

### 1. Sơ đồ luồng dữ liệu từ Local SSD lên Cloud (Hybrid Workflow)

Hệ thống làm việc lai (Hybrid Workflow) kết hợp giữa tốc độ xử lý cục bộ và tính an toàn của điện toán đám mây hoạt động theo luồng sau:

* **Bước 1 (Ghi nhận tại Local):** Khi Designer thực hiện chỉnh sửa bản vẽ 3D hiện vật và nhấn lệnh `Ctrl + S`, phần mềm thiết kế sẽ ghi trực tiếp dữ liệu xuống ổ cứng cục bộ (Local SSD) của máy tính, đảm bảo tốc độ phản hồi tức thời cho thao tác đồ họa nặng.
* **Bước 2 (Kiểm tra và Đóng gói ngầm):** Ứng dụng đồng bộ Cloud (như Google Drive for Desktop hoặc OneDrive) chạy ngầm trong hệ thống liên tục giám sát thư mục dự án. Ngay khi phát hiện tệp tin thay đổi trạng thái (`modified`), ứng dụng sẽ tự động đóng gói các gói dữ liệu thay đổi đó.
* **Bước 3 (Đẩy dữ liệu lên Cloud Storage):** Dữ liệu được truyền tải an toàn qua kết nối Internet lên máy chủ đám mây, tạo ra một bản sao lưu (Backup) mới nhất mà không yêu cầu người dùng phải thao tác thủ công thủ công hay gửi file qua các ứng dụng chat.

### 2. Cơ chế xử lý khi có xung đột phiên bản (File Version Conflict)

* **Nguyên nhân phát sinh:** Xung đột xảy ra khi hai thành viên trong nhóm (hoặc bạn A trên hai thiết bị khác nhau) cùng chỉnh sửa một file bản vẽ 3D trong điều kiện ngoại tuyến (mất kết nối Internet), sau đó đồng thời kết nối lại và đẩy dữ liệu lên cùng một đường dẫn trên Cloud.
* **Cách hệ thống xử lý tự động:**
* Hệ thống phát hiện trùng lặp mã định danh file và xung đột mốc thời gian cập nhật.
* Để ngăn chặn tình trạng ghi đè làm mất dữ liệu của một bên, hệ thống **không gộp tự động**, mà sẽ giữ nguyên bản hiện tại trên Cloud đồng thời tạo ra một bản sao lưu phụ với tên định danh kèm theo thông báo **"Conflict Version" (Phiên bản xung đột)** và tên người dùng thực hiện.
* Trưởng nhóm hoặc thành viên phụ trách sẽ mở cả hai file để kiểm tra phần thay đổi, tiến hành hợp nhất (Merge) nội dung cần thiết và xóa bản xung đột thừa.



---

## PHẦN 2: QUY TRÌNH THỰC THI 3 BƯỚC CHO MỘT DESIGNER

Để giải quyết triệt để thói quen chỉ lưu file trên Local SSD hoặc gửi qua Zalo, đồng thời phòng tránh rủi ro hỏng ổ cứng "Bad Sector", dưới đây là 3 bước thiết lập phần mềm đồng bộ chuẩn dành cho bạn A:

1. **Bước 1: Cài đặt và cấu hình ứng dụng Cloud Workspace chuyên dụng**
* Tải và cài đặt ứng dụng quản lý đồng bộ chính thức (ví dụ: Google Drive for Desktop) lên máy tính cá nhân.
* Đăng nhập bằng tài khoản được cấp phép của dự án Số hóa Bảo tàng PTIT.


2. **Bước 2: Thiết lập không gian làm việc trực tiếp trong thư mục đồng bộ**
* Di chuyển toàn bộ thư mục chứa các file đồ họa 3D hiện vật nằm hoàn toàn bên trong phân vùng ổ đĩa ảo do ứng dụng Cloud quản lý (ví dụ: thư mục `Google Drive/SoHoaBaoTang_PTIT/ThietKe_3D`).
* Từ thời điểm này, mọi thao tác tạo mới hay nhấn `Ctrl + S` của bạn A sẽ được hệ thống nhận diện và tự động đồng bộ ngầm lên đám mây.


3. **Bước 3: Xử lý tình huống ngoại lệ (Chống đứt đoạn đồng bộ khi mất kết nối / Sập nguồn)**
* *Giải pháp kỹ thuật:* Các ứng dụng đồng bộ chuẩn đều tích hợp **Cơ sở dữ liệu hàng đợi cục bộ (Local Sync Queue)**. Khi máy tính của bạn A đột ngột mất kết nối Internet hoặc bị sập nguồn ngay sau khi nhấn `Ctrl + S`, các thay đổi vẫn được lưu an toàn trong bộ nhớ đệm hàng đợi trên ổ Local SSD.
* Ngay khi thiết bị có kết nối mạng trở lại hoặc khởi động lại, hệ thống sẽ tự động đọc hàng đợi và tiếp tục tiến trình đẩy dữ liệu lên Cloud mà không làm đứt đoạn hay thất thoát bất kỳ gói dữ liệu nào.