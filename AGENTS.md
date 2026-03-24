AGENTS.md
1. Tổng quan dự án (Project Overview)
Dự án này là một plugin mở rộng (LTI 1.3 Tool) dành cho hệ thống Canvas LMS
. Plugin này hoạt động như một ứng dụng full-stack (gồm thư mục client và server) và cần được tích hợp vào hệ sinh thái mã nguồn của canvas-lms
.
2. Ràng buộc về kiến trúc (Architectural Constraints)
Để đảm bảo tính tương thích, mã nguồn của plugin PHẢI được đặt và vận hành trong cấu trúc monorepo của Canvas
:
Đường dẫn đích: gems/plugins/[tên-plugin-của-bạn]
.
Môi trường thực thi: Tác nhân cần thực hiện các thay đổi trực tiếp trong máy ảo (VM) bảo mật của Google Cloud
.
3. Thiết lập môi trường (Environment Setup)
Tác nhân AI cần thực hiện các bước sau để chuẩn bị môi trường chạy thử trong VM:
3.1 Cài đặt và cấu hình PostgreSQL
Vì Jules vận hành trong một VM đám mây sạch, hãy cài đặt Postgres bằng các lệnh sau
:
Cài đặt gói: sudo apt-get update && sudo apt-get install -y postgresql postgresql-contrib
Khởi động dịch vụ: sudo service postgresql start
Khởi tạo Database:
Tạo người dùng và cơ sở dữ liệu dựa trên các biến môi trường trong tệp .env
.
Sử dụng các thư viện như ltijs với plugin Sequelize để kết nối PostgreSQL
.
3.2 Thiết lập cấu trúc thư mục Plugin
Jules cần di chuyển mã nguồn hiện tại vào đúng vị trí trong canvas-lms
:
Kéo mã nguồn Canvas (nếu chưa có): git clone https://github.com/instructure/canvas-lms.git ../canvas
Chuyển plugin vào gems:
mkdir -p ../canvas/gems/plugins/[tên-plugin]
cp -a . ../canvas/gems/plugins/[tên-plugin]
Đăng ký Gem: Quay lại thư mục gốc của Canvas và chạy bundle install để tích hợp plugin vào hệ thống
.
4. Lệnh Build và Kiểm thử (Build and Test Commands)
Jules sẽ tự động thực hiện các lệnh sau để xác nhận thay đổi
:
Cài đặt phụ thuộc Backend: bundle install (Ruby/Rails) hoặc npm install (Node.js).
Cài đặt phụ thuộc Frontend: cd client && npm install
.
Chạy kiểm thử: bundle exec rspec hoặc npm test. Jules phải sửa lỗi nếu các bài kiểm thử này thất bại trước khi tạo Pull Request
.
5. Quy ước LTI 1.3 & Bảo mật
Xác thực: Sử dụng thuật toán RS256 để ký JWT
.
Các điểm cuối (Endpoints): Đảm bảo plugin cung cấp đúng các URL cho OIDC Login, LTI Launch và JWKS
.
Dữ liệu nhạy cảm: Mọi thông tin như LTI_KEY và thông số kết nối DB phải được lấy từ biến môi trường hoặc tệp .env
.
6. Chỉ dẫn khác cho Tác nhân
Báo cáo: Cung cấp giải trình chi tiết về kế hoạch thực hiện trước khi bắt đầu
.
Review: Mọi mã nguồn tạo ra cần tuân thủ tiêu chuẩn code style của dự án
.
Pull Request: Tác nhân nên tự động tạo PR trên GitHub sau khi hoàn thành và vượt qua các bài kiểm thử
.
