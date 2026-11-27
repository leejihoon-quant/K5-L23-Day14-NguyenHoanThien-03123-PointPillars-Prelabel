# Báo cáo Day 14 — Nguyễn Hoàn Thiện — 2A202603123

- Mã học viên dùng với `--student`: `123`
- `seed_tag` trong `frames_A.txt`: `393590736f`

## 1. Dự đoán trước sweep (bước 5b)

- **Tham số tôi nghĩ giảm công sửa nhiều nhất:** `score_thr = 0.3` (hoặc `0.4`) và `min_points = 5`.
- **Vì sao (nhìn ảnh BEV tập A: box thừa nhiều hay box sót nhiều?):** Trên ảnh BEV tập A (`s01f29`: 36 box, `s04f25`: 23 box, `s05f21`: 35 box; tổng 94 box pre-label so với 42 box thực tế), lượng **box thừa (`False Positive`) rất nhiều** — đặc biệt là các `Cyclist` ảo ở bụi cây/lề đường (`18` Cyclist ở `s01f29`) và hộp tự đè lên nóc xe ego, trong khi số vật thật bị sót rất ít (`4` hộp). Do đó tăng `score_thr` lên `0.3–0.4` kết hợp `min_points = 5` sẽ cắt mạnh các hộp nhiễu có điểm tin cậy thấp và ít điểm LiDAR, giảm đáng kể công xóa mà chưa làm mất nhiều vật thật.

## 2. Tập A: sửa tay và tune

| Cấu hình | Xóa | Thêm | Sửa | Chi phí |
|---|---:|---:|---:|---:|
| Gốc | | | | |
| `best_config.json` | | | | |

- Tham số đã đổi trong `best_config.json`: ...
- Kết quả leave-one-out (dán các dòng `scripts/pp_sweep.py` in ra): ...
- Cấu hình có ổn định trên từng frame không? Vì sao tôi vẫn chọn (hoặc không chọn) nó: ...

## 3. Trước/sau (bước 8)

![so sánh](../../d14_123/compare/s01f29.jpg)

- `scripts/pp_compare.py` báo giảm (hoặc tăng) ...% công sửa trên tập A.
- Dự đoán ở mục 1 đúng hay sai? Giải thích bằng số box xóa/thêm/sửa: ...

## 4. Tập B

Số đo từ `scripts/pp_prelabel.py --frames d14_123/frames_B.txt --config d14_123/best_config.json --gt d14_123/ref_B.json`:

| Xóa | Thêm | Sửa | Chi phí |
|---:|---:|---:|---:|
| | | | |

- Loại box tốn công sửa nhất ở tập B: ...

## 5. Giới hạn

- Nêu ít nhất 2 giới hạn (ví dụ: chỉ 3 frame tune; model KITTI không có Truck/Bus; chi phí là công thức, chưa phải thời gian sửa thật): ...
