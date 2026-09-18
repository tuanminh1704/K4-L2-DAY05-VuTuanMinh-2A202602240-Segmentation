# Báo cáo Day 5 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên VLearn. Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: 2A202602240
- Ngày / CVAT local: 17/09/2026 / CVAT
- Công cụ đã dùng: CVAT

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh VLearn đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task            | File ZIP đúng tên     | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --------------- | --------------------- | -----------------: | ---------------------------: |
| easy_semantic   | `easy_semantic.zip`   |              3 / 3 |                           20 |
| medium_instance | `medium_instance.zip` |              3 / 3 |                           32 |
| hard_panoptic   | `hard_panoptic.zip`   |              2 / 2 |                           30 |
| cp1_holes       | `cp1_holes.zip`       |              1 / 1 |                            3 |
| cp2_slice       | `cp2_slice.zip`       |              1 / 1 |                            3 |
| cp5_occlusion   | `cp5_occlusion.zip`   |              1 / 1 |                            3 |
| cp3_thin        | `cp3_thin.zip`        |              1 / 1 |                            3 |
| cp4_curb        | `cp4_curb.zip`        |              1 / 1 |                            3 |
| cp6_coverage    | `cp6_coverage.zip`    |              1 / 1 |                            3 |
| **Tổng tối đa** |                       |         |                      **100** |

Tất cả 9 task đã được hoàn thành và đã export thành các file ZIP tương ứng từ CVAT. Các file ZIP được giữ nguyên tên theo yêu cầu của bài.

Nếu export lỗi, ghi task, dữ liệu đã Save đến đâu và lỗi đã báo coach.

## 2. Một quyết định trước khi dùng gợi ý

* Ảnh, vị trí và object Medium đầu tiên tự vẽ: Ảnh Medium instance đầu tiên, chiếc xe nằm ở khu vực phía trước của ảnh.
* Class và quy tắc tôi dùng để chọn biên: `car`. Tôi vẽ mask theo phần pixel nhìn thấy thực tế của xe, bám sát đường viền thân xe và không bao gồm phần nền, bóng đổ hoặc khu vực không thuộc xe.
* Nếu dùng gợi ý sau đó: Vùng gợi ý cơ bản bao phủ đúng object nhưng một số vị trí ở biên chưa sát với đường viền thực tế. Tôi giữ phần đúng và sửa lại các vùng mask bị thừa để biên bám theo phần nhìn thấy của xe.
* Quyết định gán nhãn: Tôi ưu tiên phần vật thể quan sát được trong ảnh thay vì suy đoán phần bị che khuất.

## 3. Một lỗi tôi tìm thấy và sửa

* Task/ảnh/vùng: `medium_instance` — một ảnh trong task, vùng các xe đứng gần nhau.
* Lỗi thuộc loại: gộp-tách.
* Bằng chứng tôi nhìn thấy: Hai xe có cùng class nhưng là hai vật thể riêng biệt và bị dính mask vào nhau.
* Quy tắc và hành động sửa: Tôi tách hai xe thành hai instance riêng, mỗi instance chỉ bao phủ phần pixel nhìn thấy của xe và không ăn sang xe bên cạnh hoặc nền.
* Sau sửa đã Save và export lại chưa? Có. Tôi đã sửa annotation trên CVAT, Save lại task và export lại file `medium_instance.zip`.
* Kết quả tự kiểm: Chưa có điểm chính thức. Self-check chỉ được dùng để kiểm tra cấu trúc file nếu có.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.


| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1. Ảnh `easy_semantic` — khu vực ranh giữa lòng đường và vỉa hè | Có thể xác định ranh theo sự thay đổi màu/sắc độ của mặt đường hoặc theo mép vỉa/bó vỉa | Tôi quan sát vị trí mép vỉa và chức năng của khu vực; màu sắc có thể thay đổi do ánh sáng và bóng đổ nên không dùng màu sắc làm tiêu chí duy nhất | Tôi chọn ranh `road`–`sidewalk` theo mép vỉa/bó vỉa và chỉ gán phần diện tích thực sự thuộc từng vùng |
| 2. Ảnh `medium_instance` — khu vực có hai xe nằm sát nhau | Có thể gộp hai xe thành một mask vì chúng ở rất gần và phần biên có thể bị chạm nhau; hoặc tách thành hai instance riêng | Quan sát hình dạng và đường viền cho thấy đây là hai xe riêng biệt, cùng class nhưng là hai vật thể vật lý khác nhau | Tôi tách thành hai instance riêng, mỗi mask chỉ bao phủ phần nhìn thấy của từng xe và không gộp hai xe vào cùng một instance |
| 3. Ảnh `hard_panoptic` — phía trên giữa ảnh, khu vực ống/cột nhô lên khỏi mái tòa nhà | Có thể xem phần ống/cột là một phần của `building`, hoặc xem vùng này thuộc `sky` vì nó nằm trên nền trời và có hình dạng rất mảnh | Quan sát thấy phần ống/cột nối trực tiếp với mái của tòa nhà nên được xem là cấu trúc của tòa nhà; vùng màu xanh bao quanh và phía sau vẫn là `sky` | Tôi gán phần cấu trúc nhô lên cùng `building` và giữ phần nền trời xung quanh là `sky`, không để hai vùng mask chồng lên nhau |