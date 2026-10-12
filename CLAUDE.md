# CLAUDE.md

## Tổng quan
Compare-Single-model-and-mutilmodel-detection so sánh hiệu quả giữa **một mô hình phát hiện một loại đối tượng** và **một mô hình phát hiện nhiều lớp đối tượng cùng lúc** bằng YOLO (Ultralytics). Bài toán dùng là nhận dạng biển báo/đèn giao thông (15 lớp: Green Light, Red Light, Speed Limit 10…120, Stop) trên bộ dữ liệu "Self-Driving Cars".

## Cấu trúc
- `traffic-signs-detection-using-yolov8.ipynb` — notebook tham chiếu của tác giả pkdarabi (Kaggle) về huấn luyện YOLOv8 trên bộ dữ liệu này: pretrained, huấn luyện, validate, lưu mô hình.
- `Test.ipynb` — tải bộ dữ liệu `pkdarabi/cardetection` bằng `kagglehub`.
- `pkdarabi/cardetection/versions/5/` — bộ dữ liệu YOLO-format (~9.9k file: `car/{train,valid,test}/{images,labels}`, `data.yaml`, `video.mp4`), nguồn Roboflow, giấy phép CC BY 4.0.
- `runs/detect/train/` — kết quả một lần huấn luyện `yolov8n.pt` (30 epoch, imgsz 640): đường cong P/R/F1, confusion matrix, `results.csv`, ảnh batch mẫu. `args.yaml` còn đường dẫn máy huấn luyện (`/home/ubuntu/SonND/...`).
- `yolov8n.pt`, `yolo11n.pt` — trọng số pretrained (Ultralytics, AGPL-3.0, tải lại tự động được).

## Môi trường
Python 3.x với `ultralytics`, `kagglehub`, `opencv-python`, `matplotlib`; GPU khuyến nghị. Ví dụ huấn luyện:
```bash
yolo detect train model=yolov8n.pt data=pkdarabi/cardetection/versions/5/car/data.yaml epochs=30 imgsz=640
```
Lưu ý `data.yaml` dùng đường dẫn tương đối `../train/images` — chạy từ đúng thư mục hoặc đổi sang đường dẫn tuyệt đối.

## Quy tắc khi làm việc
- Bộ dữ liệu là của bên thứ ba (CC BY 4.0): giữ nguyên `README.dataset.txt`/`data.yaml` để còn ghi công, không xóa thông tin tác giả.
- Không commit `labels.cache` hay thư mục `runs/` mới nếu không cần (`*.cache` đã nằm trong `.gitignore`; `runs/detect/train` hiện có là kết quả cố ý giữ lại).
- Repo mới có nhánh "đơn lớp" chưa đủ: README ghi mục tiêu so sánh nhưng chưa có thí nghiệm mô hình đơn lớp riêng — khi bổ sung hãy lưu kết quả vào thư mục `runs/` riêng, đặt tên rõ (`single`, `multi`).
- Dung lượng .git ~126MB do dataset; cẩn thận khi clone/đẩy thêm ảnh.
