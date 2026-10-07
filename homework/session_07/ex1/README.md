# Bài 1: Quản lý người dùng giới hạn và Truyền tải dữ liệu qua SFTP trên Windows
---

## 2. Các bước thực hiện trên VPS (Linux)

### Bước 1: Tạo tài khoản người dùng `sftp-user` (không cấp quyền sudo)
```bash
sudo adduser sftp-user
```
*Nhập mật khẩu mạnh khi được yêu cầu và xác nhận thông tin.*

### Bước 2: Tạo thư mục log giả lập và tệp tin log
```bash
sudo mkdir -p /var/log/app-backup/
sudo touch /var/log/app-backup/backup-check.log
sudo bash -c 'echo "Backup status: SUCCESS at $(date)" > /var/log/app-backup/backup-check.log'
```

### Bước 3: Phân quyền thư mục và tệp tin cho `sftp-user`
```bash
sudo chown -R root:sftp-user /var/log/app-backup
sudo chmod 750 /var/log/app-backup
sudo chmod 640 /var/log/app-backup/backup-check.log
```

---

## 3. Kiểm tra trên VPS

### Lệnh 1: Kiểm tra thông tin người dùng `sftp-user`
```bash
id sftp-user
```
**Kết quả mong đợi:**
- Hiển thị uid, gid thuộc nhóm `sftp-user`.
- Không nằm trong nhóm `sudo` (đảm bảo không có quyền quản trị).

### Lệnh 2: Kiểm tra quyền truy cập tệp tin
```bash
ls -l /var/log/app-backup/backup-check.log
```
**Kết quả mong đợi:**
- Quyền hiển thị: `-rw-r----- 1 root sftp-user ... /var/log/app-backup/backup-check.log`

### Lệnh 3: Xem trước nội dung tệp tin trên VPS
```bash
cat /var/log/app-backup/backup-check.log
```

---

## 4. Kết nối SFTP và tải tệp tin trên Windows

### Cấu hình kết nối (Bitvise SSH Client / WinSCP)
- **Host / Server:** `<IP_CỦA_VPS>`
- **Port:** `22` (hoặc port SSH tương ứng)
- **Username:** `sftp-user`
- **Authentication:** Password (nhập mật khẩu của `sftp-user`)

### Thao tác tải tệp
1. Nhấn **Log in** để kết nối vào máy chủ.
2. Mở cửa sổ **SFTP**.
3. Tại khung bên phải (Remote / VPS): Điều hướng tới `/var/log/app-backup/`.
4. Tìm tệp `backup-check.log` và kéo/thả sang khung bên trái (Local / Windows).
5. Mở tệp `backup-check.log` trên Windows để kiểm tra nội dung có khớp với file gốc trên VPS.

---

## 5. Hình ảnh minh chứng kết quả

### Ảnh 1: Kết quả kiểm tra lệnh trên VPS (`id sftp-user` và `ls -l`)
*(Chèn ảnh chụp màn hình terminal VPS tại đây)*
`![VPS Verification](images/vps_check.png)`

### Ảnh 2: Giao diện SFTP Client (Bitvise/WinSCP) kết nối thành công và tải file
*(Chèn ảnh chụp màn hình SFTP Client đã tải backup-check.log về máy tính cá nhân tại đây)*
`![SFTP Download](images/sftp_download.png)`
