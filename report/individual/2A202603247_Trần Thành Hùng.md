# Báo cáo cá nhân — Ngày 2 Object Detection

## 1. Thông tin học viên

- Họ tên: Trần Thành Hùng
- MSSV: 2A202603247
- Email: 26ai.huntt@vinuni.edu.vn
- Lớp: L2B
- Link repo bài làm: https://github.com/Hung-1812/K5-L2B-DAY02-TranThanhHung-2A202603247-ObjectDetection
- Link Colab đã chạy: https://colab.research.google.com/drive/1_Br7VmqWYxKXNf1DaG6BBrvkU8W3OOx9?usp=sharing

## 2. Schema và mini guideline

Schema cố định: `car=0`, `truck=1`, `bus=2`, `van=3`. Guideline phân biệt xe con, xe tải có cabin/thùng, xe buýt cỡ lớn và xe thân liền khối cao như van/minivan; xe bị che/cắt vẫn gán theo phần nhìn thấy nếu nhận diện được, không đoán lớp khi mơ hồ.

Các tình huống đã ghi trong guideline:

1. `drive_006`: xe thân cao có thể là van hoặc car .
2. `drive_008`: xe buýt khớp nối và xe tải thùng — mỗi xe một box, lần lượt `bus` và `truck`.
3. `drive_009`: các xe buýt che khuất nhau — vẽ box riêng nếu nhận diện được .


## 3. Annotation và self-QC

- Số file nhãn train: 50 file `.txt`.
- Kết quả checker: đạt cấu trúc 50/50; chạy `python tools/check_yolo_labels.py` cho kết quả “ĐẠT cấu trúc”.
- Danh sách ảnh đã rà bằng mắt , các ảnh đã rà : 1,2,3,5,6,7,9,10,11,12,14,15.
- Lỗi phát hiện khi self-QC và cách sửa: một vài box đánh bị thiếu hoặc bị thừa , gán nhãn cho các xe ở quá xa bị mờ không nhìn rõ . Đã sửa lại một số các box bị thiếu hoặc thừa , xóa các nhãn cho các xe ở xa bị mờ không nhìn rõ
- Commit khóa nhãn ban đầu: Đã commit trên git

## 4. Gold QC và rework

| Chỉ số | Kết quả hiện có |
|---|---:|
| TP | 471 |
| FP | 208 |
| FN | 182 |
| Precision | 0.6937 |
| Recall | 0.7213 |
| F1 | 0.7072 |
| mean_iou_of_matches | 0.8075 |
| passed | false |

Kết quả này chưa đạt điều kiện train vì F1 thấp hơn 0.80, dù mean IoU đạt ngưỡng 0.70. Không có rework

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch: Log `args.yaml` ghi `device: cpu`.
- Tham số đọc được từ `args.yaml`: `yolo26n.pt`, 50 epochs, `imgsz=640`, batch tự động (`-1`)
- Checkpoint được ghi trong kết quả: 

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.4640 |
| Recall | 0.2382 |
| mAP50 | 0.2751 |
| mAP50–95 | 0.1841 |


Trả lời câu hỏi :

1. Confusion matrix chuẩn hóa gợi ý `bus` và `van` có recall thấp nhất: ô đường chéo của hai lớp bằng 0 trong matrix hiện có. Ở [drive_053.jpg], xe buýt lớn có box dự đoán `bus` nhưng cũng có box `car` chồng lên thân xe; đây là ví dụ cần xem lại cùng nhãn gốc, không đủ để kết luận riêng nguyên nhân recall thấp.
2. Khi mAP50 cao hơn nhiều so với mAP50–95, cần rà độ định vị box: cạnh box có ôm sát thân xe không, có lấn nền/xe bên cạnh không và box có lệch khỏi phần vật thể nhìn thấy không. IoU cao hơn đòi hỏi box chính xác hơn.
3. Lỗi có thể liên quan guideline là nhầm `bus` với `car` hoặc `van`. Confusion matrix hiện có cho thấy nhiều `bus` thật bị dự đoán thành `car`, và `van` thật bị dự đoán thành `bus`; guideline cũng ghi các ca van/minivan mơ hồ ở `drive_006` và xe che khuất ở `drive_009`. Đây là tín hiệu để kiểm tra, không chứng minh guideline là nguyên nhân duy nhất.
4. Không dùng 10 ảnh eval để train để tránh rò rỉ dữ liệu và giữ đánh giá có ý nghĩa.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Annotation QC | F1 hiện tại 0.7072, dưới ngưỡng 0.80 | Đọc kết quả evaluator và cần rà ảnh có mismatch | Chưa có rework/kết quả chạy lại |


## 7. Tự đánh giá theo rubric

- Định dạng nhãn Yolo : đạt 50/50.
- Đầy đủ vật thể, chất lượng box và phân lớp: gán đủ xe thuộc schema, kể cả xe che/cắt khung có thể nhận diện ở gần , chưa gán các xe mở , nhiễu , ở xa
- Mini guideline: đã có định nghĩa lớp, quy tắc box và các tình huống mơ hồ
- Gold-set QC: chưa đạt ngưỡng F1 >= 0.80
- Pipeline YOLO: có metrics và ảnh dự đoán cục bộ


## 8. Công cụ AI và tài liệu tham khảo

