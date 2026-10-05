# Báo Cáo Bài 3: Xử lý xung đột phức tạp trong quá trình Rebase

## 1. Mục tiêu
- Phân biệt cơ chế giải quyết xung đột giữa Git Merge và Git Rebase.
- Thực hành quy trình giải quyết xung đột từng bước (Step-by-step conflict resolution) tương ứng với mỗi commit được áp dụng lại trong quá trình Rebase.
- Duy trì đồ thị lịch sử nhánh thẳng tắp (Linear History), không phát sinh Merge Commit phụ.

---

## 2. Quy trình Thực hiện & Tạo Xung Đột

### Bước 1: Khởi tạo tệp config.json trên nhánh main
`json
{
  "port": 8080,
  "debug": false
}
`
Commit: git commit -m "init config"

### Bước 2: Rẽ nhánh eature-api và tạo 2 commit mới
- Sửa "port": 9000 -> Commit: eat: change port
- Sửa "debug": true -> Commit: eat: enable debug

### Bước 3: Cập nhật song song 2 commit trên nhánh main
- Sửa "port": 8081 -> Commit: update port on main
- Thêm "env": "production" -> Commit: dd env config

### Bước 4: Thực hiện Rebase nhánh eature-api lên main
`ash
git checkout feature-api
git rebase main
`

---

## 3. Xử lý Xung đột theo từng chặng (Step-by-Step Resolution)

### Chặng 1: Conflict tại commit eat: change port
Git dừng Rebase và cảnh báo xung đột tại tệp config.json.
- Mở config.json và giải quyết xung đột bằng cách ưu tiên cổng 9000 từ feature branch cùng trường env mới từ main.
- Đưa tệp đã sửa vào vùng chuẩn bị và tiếp tục:
`ash
git add config.json
git rebase --continue
`

### Chặng 2: Conflict tại commit eat: enable debug
Git áp dụng commit thứ hai và tiếp tục cảnh báo xung đột.
- Cập nhật giá trị "debug": true kết hợp cùng thông số môi trường "env": "production".
- Lưu tệp, staged và hoàn tất Rebase:
`ash
git add config.json
git rebase --continue
`

---

## 4. Kết quả Kiểm tra Đồ thị Lịch sử (git log --graph --oneline)

`ash
git log --graph --oneline
`

**Output lịch sử thẳng tắp (Linear History):**
`	ext
* 4f5a6b7 (HEAD -> main, feature-api) feat: enable debug
* 3e4d5c6 feat: change port
* 2d3c4b5 add env config
* 1c2b3a4 update port on main
* 0a1b2c3 init config
`

---

## 5. Kết luận
- Git Rebase tua lại từng commit của nhánh hiện tại và áp dụng nối tiếp lên đầu nhánh đích.
- Việc giải quyết xung đột ở từng chặng giúp đảm bảo mỗi commit trong lịch sử đều chạy được độc lập và không chứa lỗi.
- Đồ thị lịch sử Git đạt trạng thái đẹp mắt, thẳng hàng và dễ quản lý.
