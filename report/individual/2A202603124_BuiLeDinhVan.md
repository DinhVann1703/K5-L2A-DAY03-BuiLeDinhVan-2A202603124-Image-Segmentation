# Báo cáo thực hành Day 3 — điền trực tiếp trong fork của bạn

- Mã học viên theo lớp: 2A202603124 (MSSV: B22DCAT309)
- Ngày / CVAT local: 11/11/2026 / CVAT local Standard v2.74 (Docker)
- Công cụ đã dùng: Brush, Polygon, Magic Wand (SAM nuclio interactor)

## 1. Bài đã nộp

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | cp1_holes.zip | 1 / 1 | 3 |
| cp2_slice | cp2_slice.zip | 1 / 1 | 3 |
| cp5_occlusion | cp5_occlusion.zip | 1 / 1 | 3 |
| cp3_thin | cp3_thin.zip | 1 / 1 | 3 |
| cp4_curb | cp4_curb.zip | 1 / 1 | 3 |
| cp6_coverage | cp6_coverage.zip | 1 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

## 2. Quyết định trước khi dùng gợi ý

- **Ảnh, vị trí và object Medium đầu tiên tự vẽ:** Ảnh `000000181542.jpg` trong `medium_instance`, người phụ nữ mặc áo dài trắng đi bộ ở giữa vạch sang đường trung tâm ảnh.
- **Class và quy tắc tôi dùng để chọn biên:** Class `person`. Quy tắc chọn biên là vẽ Polygon ôm sát mép viền áo dài, tay và chân nhìn thấy được của người phụ nữ, không tô tràn sang vùng vạch sang đường (`road`) hay phần bánh xe máy đằng sau.
- **Nếu dùng gợi ý sau đó:** Gợi ý tự động của SAM cắt viền khá chuẩn phần áo dài nhưng bị lem nhẹ ở nếp tà áo giáp với vạch kẻ đường trắng. Tôi đã dùng Brush tinh chỉnh lại điểm nút viền tà áo bám sát cơ thể người walk.

## 3. Một lỗi tôi tìm thấy và sửa

- **Task/ảnh/vùng:** Task `cp3_thin` / ảnh `839f7736-abe28069.jpg` / vùng khung kim loại treo biển báo giao thông (Cross Island Pkwy).
- **Lỗi thuộc loại:** Biên / thiếu-thừa vùng.
- **Bằng chứng tôi nhìn thấy:** Thanh khung kim loại giữ biển báo và cột đèn rất mảnh, ban đầu vẽ Polygon bị lấn rộng sang vùng bầu trời (`sky`).
- **Quy tắc và hành động sửa:** Zoom cận cảnh (200%), hạ kích thước Brush xuống nét nhỏ để tô chính xác phần khung kim loại (`pole`/`traffic sign`), không bị lem sang nhãn `sky`.
- **Sau sửa đã Save và export lại chưa?:** Đã Save và export lại thành file `cp3_thin.zip`, chạy `inspect_submissions.py` kiểm tra báo `[OK]`.
- **Kết quả tự kiểm/GitHub Actions:** Đã chạy `scripts/inspect_submissions.py` kiểm tra cấu trúc 8 file ZIP thành công `[OK]`.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1. `cp1_holes` (Ảnh `000000144300.jpg` - Xe mô tô Honda 1100R) | 1) Khoét lỗ rỗng các khe giữa nan hoa bánh xe và đĩa phanh.<br>2) Tô kín toàn bộ bánh xe thành một khối mask `motorcycle`. | Quy tắc cp1_holes: Kính/khe nhỏ nằm trong cấu trúc vật thể không khoét lỗ tùy tiện trừ khi nhìn xuyên rõ background. | Quyết định tô phủ kín bánh xe và khung máy thuộc nhãn `motorcycle`, không khoét lỗ rỗng nhỏ li ti giữa các nan hoa. |
| 2. `cp5_occlusion` (Ảnh `000000336232.jpg` - Hàng xe ô tô đỗ trên dốc) | 1) Các xe bị ô tô khác che khuất một phần đuôi bị tách thành các mask lẻ.<br>2) Giữ nguyên định danh (1 instance ID) cho từng xe dù bị che một phần. | Quy tắc occlusion: Vật thể bị vật khác che khuất một phần vẫn duy nhất một instance ID theo phần nhìn thấy. | Quyết định gán nhãn đúng 1 instance ID cho từng chiếc ô tô bị che phần đầu/đuôi. |
| 3. `cp3_thin` (Ảnh `839f7736-abe28069.jpg` - Biển báo giao thông trên cao) | 1) Tô gộp cả bảng biển báo và khung sắt treo vào nhãn `traffic sign`.<br>2) Tách phần bảng thành `traffic sign` và khung kim loại/cột đỡ thành `pole`. | Quy tắc taxonomy: Phân biệt rõ bảng chức năng (`traffic sign`) và hệ thống cột/khung đỡ (`pole`). | Quyết định vẽ phần biển báo màu xanh lá là `traffic sign` và các thanh sắt/cột treo xung quanh là `pole`. |
