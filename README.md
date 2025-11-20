Mình giải thích từng trường trong record đó nhé:

{
  "license_key": "VIP-USER-999",
  "owner": "vip_user@gmail.com",
  "plan": "lifetime",
  "max_devices": 3,
  "max_sessions": 2,
  "expire_at": "2099-12-31T23:59:59Z",
  "created_at": "2025-01-01T00:00:00Z",
  "status": "active",
  "allowed_hwids": [],
  "note": "Lifetime, không giới hạn HWID"
}

Ý nghĩa từng field

license_key:
Chuỗi key mà user phải nhập / bạn lưu trong license.cfg.
→ Ở đây là: VIP-USER-999

owner:
Thông tin chủ sở hữu license (thường là email hoặc tên).
→ vip_user@gmail.com

plan:
Gói license, bạn tự định nghĩa ý nghĩa (ví dụ free, pro, lifetime, monthly...).
→ Ở đây là gói: lifetime

max_devices:
Số thiết bị tối đa được phép dùng license này (nếu bạn muốn kiểm soát theo HWID).
Hiện tại code mình viết chưa dùng giá trị này, nhưng sau này bạn có thể:

Lưu danh sách HWID từng lần active,

Nếu vượt quá max_devices thì chặn.

max_sessions:
Số phiên chạy đồng thời tối đa (ví dụ không cho chạy song song 5 app cùng key).
Cũng tương tự max_devices, hiện tại bạn chưa dùng, nhưng để đó cho tương lai.

expire_at:
Thời điểm hết hạn license, dạng ISO UTC.
→ "2099-12-31T23:59:59Z" nghĩa là đến cuối năm 2099 mới hết hạn (coi như vĩnh viễn).

created_at:
Thời điểm tạo license, chủ yếu để log/hiển thị, hiện tại code không dùng.
→ "2025-01-01T00:00:00Z"

status:
Trạng thái license.

"active" → được phép dùng.

Bạn có thể đặt "banned", "suspended", "expired"… nếu muốn chặn.
Code đang check:

status = str(lic.get("status", "")).lower()
if status != "active":
    # báo lỗi LICENSE_NOT_ACTIVE


allowed_hwids:
Danh sách HWID (device_id) được phép dùng license này.

[] rỗng nghĩa là không giới hạn máy → mọi HWID đều được.

Nếu bạn muốn chỉ cho 1–2 máy, ví dụ:

"allowed_hwids": [
  "54bf730fabc0e02a5b3f240c7f62aaddd9342aa1bcc41bf88a7175f1a1bb097d",
  "another_device_hash"
]


Khi đó code sẽ so device_id với list này, nếu không trùng → báo "DEVICE_NOT_ALLOWED".

note:
Ghi chú cho admin / để hiển thị cho user, tuỳ bạn dùng.
→ "Lifetime, không giới hạn HWID"
