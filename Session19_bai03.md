# Bài 3: Khắc phục sự cố rò rỉ thông tin đăng nhập (Infostealer)

## 1. Phân tích IPO

| Thành phần | Nội dung |
|---|---|
| **Input** | Facebook/Gmail liên tục mất quyền; thông báo đăng nhập lạ; máy chậm, tiến trình/extension lạ; có forwarding/filter lạ trong Gmail; token/SSH key lạ trên GitHub |
| **Process** | 4 bước cấp cứu (mục 2) |
| **Output** | Máy sạch, mật khẩu mới, không còn phiên/token lạ, 2FA đã bật |

> Infostealer đánh cắp cả **cookie/token**, nên chỉ đổi mật khẩu là chưa đủ, phải **revoke** thêm.

## 2. Quy trình 4 bước

1. **Ngắt kết nối:** tắt Wi-Fi, rút mạng máy nghi nhiễm; không đăng nhập thêm tài khoản nào.
2. **Đổi mật khẩu từ thiết bị khác:** dùng thiết bị sạch, mạng khác; đổi theo thứ tự Gmail → Facebook → GitHub → ngân hàng/ví; bật 2FA.
3. **Revoke token/phiên:** đăng xuất mọi thiết bị, xóa token/key/ứng dụng lạ.
4. **Làm sạch và xác minh:** quét mã độc hoặc cài lại OS; xóa mật khẩu lưu trong trình duyệt; kiểm tra lại Security log.

## 3. Triển khai

### 3.1 Đổi mật khẩu từ thiết bị khác
1. Dùng điện thoại/máy sạch, trình duyệt ẩn danh, mạng 4G.
2. **Gmail:** `myaccount.google.com/security` → đổi mật khẩu → "Thiết bị của bạn" → đăng xuất thiết bị lạ → xóa forwarding/filter lạ → bật xác minh 2 bước.
3. **Facebook:** `Trung tâm tài khoản → Mật khẩu và bảo mật` → đổi mật khẩu, chọn "Đăng xuất khỏi thiết bị khác".
4. Mật khẩu mới dài ≥ 14 ký tự, mỗi dịch vụ một mật khẩu riêng (dùng trình quản lý mật khẩu).

### 3.2 Revoke Token trên GitHub
| Hạng mục | Đường dẫn | Thao tác |
|---|---|---|
| Phiên đăng nhập | `Settings → Sessions` | Revoke tất cả |
| Personal Access Token | `Settings → Developer settings → Personal access tokens` | Delete token nghi ngờ |
| OAuth Apps | `Settings → Applications` | Revoke ứng dụng lạ |
| SSH keys | `Settings → SSH and GPG keys` | Xóa key cũ, tạo key mới trên máy sạch |
| 2FA | `Settings → Password and authentication` | Bật 2FA, tạo lại recovery codes |
| Kiểm tra | `Settings → Security log` | Rà soát hành động lạ |

```bash
ssh-keygen -t ed25519 -C "email@example.com"   # tạo SSH key mới trên máy sạch
```

## 4. Minh chứng nộp bài
- [ ] Ảnh trang Sessions sau khi revoke
- [ ] Ảnh danh sách token/OAuth Apps sau khi dọn dẹp
- [ ] Ảnh 2FA đã bật

> Che (blur) token và thông tin cá nhân trước khi chụp; không commit token lên repo.