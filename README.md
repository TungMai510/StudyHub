# StudyHub — Diễn đàn học tập

Bản MVP gồm giao diện HTML/CSS/JavaScript và database PostgreSQL trên Supabase.

## Có sẵn
- Đăng ký/đăng nhập bằng email và mật khẩu riêng của diễn đàn.
- Hồ sơ tự tạo; vai trò mặc định là Học sinh.
- Đăng bài, phân loại bài viết, xem bảng tin.
- Bình luận, thích/bỏ thích bài viết.
- Giáo viên có thể ghim bài và xóa bài viết vi phạm.
- Người đăng có thể xóa bài của mình.
- Phân quyền và Row Level Security (RLS) ở database.

## 1. Tạo database
1. Tạo project tại https://supabase.com/.
2. Mở **SQL Editor** trong dashboard.
3. Mở file `schema.sql`, sao chép toàn bộ và chạy.
4. Vào **Project Settings → API** (hoặc mục API Keys) và lấy Project URL cùng publishable/anon key. Chỉ dùng key công khai `anon`/`publishable` ở frontend; tuyệt đối không đưa `service_role` key vào HTML.

## 2. Kết nối website
Mở `index.html`, tìm hai dòng:
```js
const SUPABASE_URL = "YOUR_SUPABASE_URL";
const SUPABASE_ANON_KEY = "YOUR_SUPABASE_ANON_KEY";
```
Thay bằng URL và public anon/publishable key của project.

## 3. Tạo tài khoản giáo viên
1. Giáo viên đăng ký tài khoản trên website như bình thường.
2. Xác nhận email (nếu Supabase yêu cầu).
3. Trong Supabase → SQL Editor, chạy lệnh dưới đây, thay email:
```sql
update public.profiles p
set role = 'teacher'
from auth.users u
where p.id = u.id and u.email = 'giaovien@truong.edu.vn';
```
Không cho người dùng tự chọn vai trò giáo viên lúc đăng ký để tránh giả mạo quyền.

## 4. Chạy thử
- Có thể mở `index.html` trực tiếp sau khi cấu hình.
- Để chạy ổn định, nên đưa thư mục lên hosting tĩnh như Vercel hoặc Netlify.
- Trong Supabase Authentication → URL Configuration, cấu hình Site URL và Redirect URLs cho domain website thực tế.

## Lưu ý triển khai
- Đây là bản MVP khởi đầu, chưa có tải tệp/hình ảnh, thông báo đẩy, tìm kiếm nâng cao, quản trị lớp học, báo cáo vi phạm hay kiểm duyệt tự động.
- Nếu bật xác nhận email, học sinh cần xác nhận email trước khi đăng nhập.
- Không thu thập mật khẩu Azota. Tài khoản ở đây độc lập với Azota.
- Với học sinh chưa đủ tuổi, nhà trường cần có sự đồng ý/đánh giá phù hợp của phụ huynh và chính sách bảo vệ dữ liệu cá nhân trước khi đưa vào sử dụng thật.
- Bật backup phù hợp trên Supabase và giới hạn quyền truy cập project cho người quản trị.
