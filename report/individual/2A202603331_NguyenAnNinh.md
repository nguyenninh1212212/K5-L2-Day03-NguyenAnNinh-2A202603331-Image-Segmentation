# Báo cáo thực hành Day 3 — Nguyễn An Ninh

- MSSV: 2A202603331 (theo tên thư mục).
- Ngày kiểm tra: 12/11/2026.
- Nền tảng: CVAT web, https://app.cvat.ai/jobs.
- Công cụ: CVAT web để tạo và xuất annotation; không có ghi chép đủ để xác nhận công cụ vẽ hoặc mô hình gợi ý cụ thể. Có dùng trợ lý AI để kiểm tra cấu trúc ZIP và hỗ trợ soạn báo cáo; các kết quả kiểm tra không thay thế đánh giá mask.

## 1. Bài đã nộp

Đã có các ZIP trong thư mục bài làm; chưa xác nhận đã push GitHub hoặc nộp LMS. Số ảnh dưới đây phản ánh export, chưa xác nhận chất lượng.

| Task | ZIP | Ảnh trong export | Kiểm tra cấu trúc | Điểm tối đa |
| --- | --- | --- | --- | --- |
| easy_semantic | easy_semantic.zip | 3/3 | Lỗi: label person ngoài schema | 20 |
| medium_instance | medium_instance.zip | 3/3 | OK, 34 annotation | 32 |
| hard_panoptic | hard_panoptic.zip | 2/2 | OK, 104 annotation; chưa thấy vegetation | 30 |
| cp1_holes | cp1_holes.zip | 1/1 | OK, 5 annotation | 3 |
| cp2_slice | cp2_slice.zip | 1/1 | OK, 17 annotation | 3 |
| cp3_thin | cp3_thin.zip | 1/1 | OK | 3 |
| cp4_curb | cp4_curb.zip | 1/1 | OK | 3 |
| cp5_occlusion | cp5_occlusion.zip | 1/1 | OK, 102 annotation | 3 |
| cp6_coverage | cp6_coverage.zip | 1/1 | OK | 3 |
| Tổng tối đa | | 14 ảnh | | 100 |

Kiểm bằng `python scripts/inspect_submissions.py --dir submissions`. OK chỉ xác nhận cấu trúc, không xác nhận biên, class, số vật hoặc điểm. Chưa tính điểm với ground truth.

## 2. Một quyết định trước khi dùng gợi ý

Không có ghi chép về thứ tự thao tác để xác định object Medium đầu tiên tự vẽ trước khi xem gợi ý. ZIP cuối cùng không đủ để suy ra trình tự này, vì vậy mục bằng chứng này còn thiếu.

Nguyên tắc dùng khi kiểm lại Medium: mỗi vật là một instance riêng, kể cả khi cùng class và sát nhau; mask theo phần nhìn thấy, không đoán vùng bị che. Đây là nguyên tắc kiểm bài, chưa phải bằng chứng về quyết định của object đầu tiên.

## 3. Một lỗi tìm thấy và trạng thái sửa

- Task: easy_semantic.
- Lỗi file quan sát được: tên ban đầu easy_sentamic.zip; export mới có label person ngoài schema.
- Đã sửa tên file thành easy_semantic.zip, giữ nguyên nội dung ZIP.
- Easy chỉ có road, sidewalk, building, vegetation, sky. Cần kiểm nhãn/annotation trên CVAT và xuất lại Segmentation mask 1.1 với đúng bộ nhãn. Không đổi người thành lớp nền chỉ để vượt kiểm tra.
- Chưa xác nhận đã Save/export sau khi sửa nhãn. Không sửa trực tiếp JSON/PNG trong ZIP.
- Bằng chứng hiện có là lỗi tên file đã sửa và lỗi bộ nhãn còn tồn tại; chưa có ghi chép xác minh một lần sửa biên/vùng/object.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Ba ca dưới đây được phân tích từ ảnh cp1_holes `000000144300.jpg` và quy tắc của bài khi rà soát báo cáo. Chưa xác nhận mask đã xử lý đúng các ca này.

| Ảnh/vị trí | Hai cách hiểu | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi |
| --- | --- | --- | --- |
| cp1_holes, kính chắn gió phía trước mô tô | Kính thuộc xe hay vùng nền do nhìn xuyên qua | Kính là chi tiết của xe; quy tắc cp1 không khoét kính tùy tiện | Giữ kính trong mask motorcycle; cần kiểm lại mask đã xuất |
| cp1_holes, giá đỡ màu đỏ dưới bánh sau | Chi tiết xe hay dụng cụ đỡ riêng | Giá đỡ tiếp xúc xe nhưng là dụng cụ bên ngoài, không phải bộ phận mô tô | Không đưa giá đỡ vào mask motorcycle; cần kiểm lại mask đã xuất |
| cp1_holes, người đứng phía sau yên xe | Vẽ toàn thân ước đoán hay chỉ phần nhìn thấy | Chỉ gán nhãn phần quan sát được; không suy ra biên sau vật che | Tách person khỏi motorcycle và không vẽ xuyên phần xe che; cần kiểm lại mask đã xuất |
