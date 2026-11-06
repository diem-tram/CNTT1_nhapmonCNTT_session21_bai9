# Bài 9: Xây Dựng Prompt 3 Thành Phần Giải Mã Thông Báo Lỗi Trạm Sạc Cùng Trợ Lý AI
## Phần 1: thiết kế Prompt 3 thành phần chuẩn hóa 
**1.** Phân tích nguyên nhân vì sao câu hỏi *'AI ơi phần mềm trạm sạc của tôi bị hỏng rồi'* lại khiến AI đưa ra câu trả lời sai lệch (hiện tượng AI Hallucination)?
- thiếu ngữ cảnh: AI không biết ứng dụng đang chạy trên hệ điều hành nào, lỗi xảy ra ở phând cứng hay phần mềm
- thiếu nội dung lỗi: AI không nhận được thông báo lỗi hệ thống cụ, buộc AI phải tự đoán nguyên nhân dựa trên thông tin mình đưa
- Mô tả mơ hồ: dùng từ nghĩa quá rộng "hỏng" 
**Sửa** 
Context
Tôi đang quản lý hệ thống Kiosk nạp năng lượng thông minh VoltCharge chạy trên hệ điều hành Windows, trụ sạc nhanh số 4 đột ngột dừng hoạt động 
Raw Error
 hiện lên dòng lỗi 
ControllerDaemon failed to start.
System.UnauthorizedAccessException: Access to the path 'C:\Program Files\VoltCharge\data\session.log' is denied.
   at System.IO.FileStream.ValidateFileHandle(SafeFileHandle handle)
   at VoltCharge.Core.SessionManager.Initialize()
InnerException: None. Process terminated with exit code 5 (0x00000005).
Suggestion: Run the application with elevated administrator privileges or verify NTFS file permissions.
Instruction
hãy dịch và giải thích nguyên nhân gốc rễ của lỗi này một cách ngắn gọn và đơn giải . Đồng thời đưa ra ba bước xử lý tại chỗ trên Windows để khắc phục triệt để lỗi này
## Phần 2: giải mã lỗi và kịch bản khắc phục
Giải thích nguyên nhân gốc rễ lỗi UnauthorizedAccessException
dịch đơn giản: hệ thống từ chối truy cập: Không có quyền ghi/sửa file session.log tại đường dẫn C:\Program Files\VoltCharge\data".
### Quy trình 3 bước khắc phục sự cố tại chỗ trên Windows 11
1
Chạy thử với quyền Quản trị viên
Kiểm tra nhanh
Tắt ứng dụng trong Task Manager → Nhấp chuột phải vào file chạy của phần mềm → Chọn Run as administrator.
Verification: Ứng dụng khởi động thành công mà không văng lỗi exit code 5.

2
Cấu hình luôn chạy dưới quyền Administrator
Thiết lập cố định
Nhấp chuột phải vào file chạy → Chọn Properties → Tab Compatibility → Tích chọn Run this program as an administrator → Nhấn Apply và OK.
Verification: Lần mở ứng dụng tiếp theo sẽ tự động yêu cầu cấp quyền Admin từ hệ thống.

3
Phân quyền ghi file cho thư mục dữ liệu
Phân quyền NTFS
Truy cập thư mục C:\Program Files\VoltCharge\ → Nhấp chuột phải vào thư mục data → Properties → Tab Security → Nhấn Edit... → Chọn nhóm người dùng và tích chọn Allow ở mục Full control hoặc Write → Nhấn OK.
Verification: Dịch vụ ControllerDaemon ghi được file session.log bình thường khi khởi động lại máy.
### Xử lý bẫy
kỹ thuật viên tuyệt đối không gửi đoạn mã bí mật  API_KEY: volt_secret_token_889922 lên chatGBT/gemini Phải thực hiện che/thay thế thông tin này bằng chuỗi giả định (ví dụ: API_KEY: [REDACTED_SECRET_KEY]) trước khi copy-paste vào khung chat.
lý do: nguy cơ rò rỉ dữ liệu, rủi ro bảo mật hệ thống 