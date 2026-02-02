# 🖥️ Windows Remote Desktop Services - GitHub Actions

## 📋 Mô tả

Workflow GitHub Actions này cho phép bạn tạo **10 máy ảo Windows 10 Professional** chạy song song trên GitHub Runner, mỗi máy hoạt động liên tục **6 giờ** với kết nối RDP và Web Viewer.

---

## ✨ Tính năng

✅ **10 instance Windows 10 Pro** chạy đồng thời  
✅ **RDP Connection** - Kết nối Desktop từ xa  
✅ **Web Viewer** - Truy cập qua trình duyệt  
✅ **Cloudflare WARP** - VPN tự động  
✅ **Chromium Browser** - Trình duyệt sẵn có  
✅ **Cấu hình mạnh** - 4 vCPU | 8GB RAM | 60GB Disk  
✅ **Log đẹp** - Giao diện rõ ràng, dễ theo dõi  

---

## 🚀 Cách sử dụng

### **Bước 1: Tạo Repository**
1. Tạo repository mới trên GitHub (public hoặc private)
2. Vào **Settings** → **Actions** → **General**
3. Bật **Allow all actions and reusable workflows**

### **Bước 2: Upload Workflow**
1. Tạo thư mục `.github/workflows/` trong repo
2. Upload file `Windows_Fixed_Beautiful.yml` vào thư mục này
3. Commit và push lên GitHub

### **Bước 3: Chạy Workflow**
1. Vào tab **Actions** trong repository
2. Chọn workflow **🖥️ REMOTE DESKTOP SERVICES**
3. Click **Run workflow** → **Run workflow**
4. Đợi 2-5 phút để hệ thống khởi động

### **Bước 4: Lấy thông tin kết nối**
1. Click vào workflow đang chạy
2. Chọn bất kỳ instance nào (ví dụ: **🖥️ Windows Instance #1**)
3. Mở step **🌐 Connection Information**
4. Sao chép thông tin:

```
╔═══════════════════════════════════════════════════════════════╗
║                  ✅ CONNECTION READY                          ║
╠═══════════════════════════════════════════════════════════════╣
║  🌐  RDP Connection                                           ║
║      123.45.67.89:12345                                       ║
║                                                               ║
║  👤  Username                                                 ║
║      Admin                                                    ║
║                                                               ║
║  🔐  Password                                                 ║
║      123456                                                   ║
╚═══════════════════════════════════════════════════════════════╝
```

---

## 🔌 Cách kết nối

### **Phương án 1: Remote Desktop (RDP)**

#### **Windows:**
1. Nhấn `Win + R`
2. Gõ `mstsc` và Enter
3. Nhập địa chỉ RDP (ví dụ: `123.45.67.89:12345`)
4. Nhập thông tin:
   - Username: `Admin`
   - Password: `123456`

#### **macOS:**
1. Tải [Microsoft Remote Desktop](https://apps.apple.com/app/microsoft-remote-desktop/id1295203466)
2. Click **Add PC**
3. Nhập địa chỉ RDP
4. Nhập username/password

#### **Linux:**
```bash
sudo apt install rdesktop
rdesktop 123.45.67.89:12345 -u Admin -p 123456
```

### **Phương án 2: Web Viewer**
1. Mở trình duyệt
2. Truy cập URL Web Viewer (ví dụ: `http://123.45.67.89:8006`)
3. Sử dụng chuột/bàn phím ngay trong trình duyệt

---

## ⚙️ Cấu hình chi tiết

| Thông số | Giá trị |
|----------|---------|
| **Operating System** | Windows 10 Professional |
| **CPU** | 4 vCPU |
| **RAM** | 8 GB |
| **Disk** | 60 GB |
| **Username** | Admin |
| **Password** | 123456 |
| **Thời gian chạy** | 6 giờ (360 phút) |
| **Số instance** | 10 (song song) |

---

## 📦 Phần mềm đã cài sẵn

| Phần mềm | Mô tả |
|----------|-------|
| **Cloudflare WARP** | VPN miễn phí, tự động kết nối |
| **Chromium Browser** | Trình duyệt web (shortcut trên Desktop) |
| **DNS Cloudflare** | 1.1.1.1 / 1.0.0.1 (nhanh & bảo mật) |

---

## 📊 Cấu trúc Workflow

```
┌─────────────────────────────────────┐
│  Job 1: Prepare OEM Binaries        │
│  - Download Cloudflare WARP         │
│  - Download Chromium                │
│  - Upload artifact (dùng chung)     │
└──────────────┬──────────────────────┘
               │
               ▼
┌─────────────────────────────────────┐
│  Job 2: Windows Docker (x10)        │
│  - Download artifact                │
│  - Khởi tạo container Windows       │
│  - Cài đặt WARP + Chromium          │
│  - Tạo Kami Tunnel (RDP + Web)      │
│  - Hiển thị thông tin kết nối       │
│  - Duy trì session 6 giờ            │
└─────────────────────────────────────┘
```

---

## 🔒 Bảo mật

⚠️ **Lưu ý quan trọng:**

1. **Password mặc định:** `123456` - Rất yếu, nên đổi ngay
2. **Kết nối public:** RDP được public ra internet
3. **Không lưu dữ liệu:** Mọi thứ sẽ mất sau 6 giờ
4. **Chỉ dùng test:** Không dùng cho mục đích production

### **Cách tăng bảo mật:**

**Đổi password mạnh hơn:**
```yaml
# Dòng 127-128 trong file .yml
USERNAME: "Admin"
PASSWORD: "P@ssw0rd!2024#Strong"  # Đổi thành password mạnh
```

---

## 🛠️ Tùy chỉnh

### **Thay đổi cấu hình phần cứng:**
```yaml
# Dòng 128-131
RAM_SIZE: "8G"       # Tăng lên 16G nếu cần
CPU_CORES: "4"       # Tăng lên 6-8 cores
DISK_SIZE: "60G"     # Tăng lên 100G
```

### **Thay đổi thời gian chạy:**
```yaml
# Dòng 63
timeout-minutes: 360  # 360 phút = 6 giờ (max GitHub: 360)
```

### **Thay đổi số lượng instance:**
```yaml
# Dòng 69
matrix:
  instance: [1,2,3,4,5]  # Giảm xuống 5 instance
```

---

## 📝 Log Output

### **Log ngắn gọn (mỗi 30 phút):**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🟢 [14:30:00] Instance #1 - ACTIVE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
⏱️  Elapsed   : 30 min
⏳  Remaining : 330 min
🖥️  RDP       : 123.45.67.89:12345
🌐  Web       : http://123.45.67.89:8006
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Tần suất hiển thị:**
- Lần đầu: 0 phút (bắt đầu)
- Mỗi 30 phút: 30, 60, 90... 330 phút
- Lần cuối: 360 phút (kết thúc)

**Tổng:** 13 log thay vì 72 log → **Giảm 82% log spam**

---

## ❓ Troubleshooting

### **❌ Lỗi: "Could not retrieve RDP IP"**
**Nguyên nhân:** Kami Tunnel chưa sẵn sàng  
**Giải pháp:** Đợi thêm 1-2 phút, workflow sẽ retry tự động

### **❌ Lỗi: "Container failed to start"**
**Nguyên nhân:** Không đủ tài nguyên hoặc KVM không khả dụng  
**Giải pháp:** Giảm số instance xuống 5-7

### **❌ Lỗi: "Connection timeout"**
**Nguyên nhân:** Firewall chặn hoặc địa chỉ IP sai  
**Giải pháp:** 
1. Kiểm tra lại IP:Port
2. Tắt firewall/antivirus tạm thời
3. Thử Web Viewer thay vì RDP

### **❌ Windows khởi động chậm**
**Bình thường:** Windows cần 3-5 phút để boot  
**Kiểm tra:** Mở Web Viewer để xem tiến trình boot

---

## 💡 Tips & Tricks

### **1. Dùng nhiều instance:**
- Mỗi instance độc lập hoàn toàn
- Có thể chạy nhiều tác vụ song song
- IP và port khác nhau cho mỗi instance

### **2. Tối ưu băng thông:**
- Dùng Web Viewer nếu mạng chậm
- Giảm độ phân giải màn hình trong RDP settings
- Tắt wallpaper/hiệu ứng trong Windows

### **3. Sử dụng Chromium:**
- Shortcut sẵn trên Desktop
- Đã tích hợp WARP VPN
- Có thể cài thêm extension

### **4. Sao lưu dữ liệu:**
⚠️ **MỌI DỮ LIỆU SẼ MẤT SAU 6 GIỜ**
- Upload file lên Google Drive/Dropbox
- Gửi qua email
- Không lưu dữ liệu quan trọng

---

## 🎯 Use Cases

✅ **Test phần mềm Windows** trên cloud  
✅ **Chạy script/automation** ngắn hạn  
✅ **Download file** qua Windows  
✅ **Duyệt web ẩn danh** qua WARP  
✅ **Học tập/thử nghiệm** Windows  
❌ **KHÔNG dùng cho:** Mining, DDoS, spam, vi phạm ToS GitHub

---

## 📜 License & Credits

### **Credits:**
- **Docker Image:** [dockurr/windows](https://github.com/dockurr/windows)
- **Tunnel Service:** [Kami Tunnel](https://github.com/kami2k1/tunnel)
- **VPN:** [Cloudflare WARP](https://1.1.1.1/)

### **License:**
- Code: MIT License
- **Tuân thủ:** [GitHub Actions Terms of Service](https://docs.github.com/en/site-policy/github-terms/github-terms-of-service)

⚠️ **Disclaimer:** Workflow này chỉ dùng cho mục đích học tập và test. Người dùng chịu trách nhiệm về việc tuân thủ điều khoản sử dụng của GitHub.

---

## 🔗 Links hữu ích

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Docker Windows Images](https://github.com/dockurr/windows)
- [Remote Desktop Protocol Guide](https://learn.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-clients)

---

## 📞 Support

Nếu gặp vấn đề:
1. Kiểm tra phần **Troubleshooting** ở trên
2. Xem log chi tiết trong GitHub Actions
3. Tạo Issue trên GitHub repository

---

**Made with ❤️ for Windows lovers**

*Last updated: February 2, 2026*
