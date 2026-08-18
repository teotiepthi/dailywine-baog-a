# Gấu Mèo — Quy tắc chọn: làm trực tiếp hay bắn qua Routine

## Bối cảnh

Trước đây `gaumeo_bot.py` là bot Python viết tay trên máy Mac — tự nó không code
được, nên mọi việc code phải bắn ra ngoài cho một phiên Claude Code khác làm hộ.
Routines (scheduled trigger tạo phiên mới) là cách làm việc đó mà bill vào Max
thay vì tốn API key riêng.

Giờ Gấu Mèo là một phiên Claude Code thật, chạy qua Channels — tự đọc/sửa file,
tự chạy lệnh ngay tại chỗ, không cần "nhờ" ai. Đường vòng qua Routines không còn
là bắt buộc, nhưng vẫn có giá trị cho một trường hợp cụ thể: **việc dài hơi làm
tắc chat**.

## Quyết định: kết hợp cả hai, chia theo quy mô việc

| | Routines | Sửa trực tiếp trong phiên Channels |
|---|---|---|
| Tắc chat khi đang chạy? | Không — bắn job đi, chat tiếp bình thường | Có — Gấu Mèo bận thì tin nhắn khác phải chờ |
| Sửa file cục bộ trên Mac? | Không — chỉ sửa repo đã đẩy lên GitHub | Có — sửa được bất kỳ file nào trên máy |
| Duyệt kết quả | Qua PR, a.Tèo tự duyệt/merge | Sửa thẳng, không có bước duyệt |
| Giới hạn | 15 lần/ngày, chung quota Max | Không giới hạn riêng, vẫn chung quota Max |

## Quy tắc phân loại (Gấu Mèo tự áp dụng, không hỏi lại a.Tèo)

**Việc nhỏ (vài giây) → làm ngay trong phiên Channels:**
- Đổi 1 dòng config, 1 giá trị, 1 tham số
- Xem log, xem status, chẩn đoán lỗi
- Restart service / process
- Trả lời câu hỏi, tra cứu nhanh
- Sửa 1 file, thay đổi cục bộ không cần review

**Việc lớn (vài phút trở lên) → bắn qua Routine:**
- Thêm tính năng mới
- Sửa nhiều file / nhiều bước liên tiếp
- Bất kỳ việc nào sẽ khiến chat bị treo lâu trong lúc code chạy

Nguyên tắc chốt: nếu việc **xong trong vài giây và không cần a.Tèo duyệt trước
khi áp dụng** → làm ngay. Nếu việc **tốn thời gian đáng kể hoặc cần đẩy lên
GitHub để review qua PR** → tạo Routine (`create_trigger` / `create_session`)
để chat không bị nghẽn, rồi báo lại a.Tèo khi xong.

## Lưu ý

- Quota Max là chung cho cả hai đường — không đường nào "rẻ" hơn đường nào.
- Việc code trên máy Mac của a.Tèo (file cục bộ, không nằm trong repo GitHub)
  chỉ có thể làm trực tiếp trong phiên Channels — Routine không với tới được.
