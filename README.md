# LOG ANALYTICS

## Tổng quan
Dự án này cung cấp pipeline phân tích log NASA, bao gồm các bước thu thập, tiền xử lý và trực quan hóa dữ liệu log. Giải pháp sử dụng Python, Jupyter Notebook và các công nghệ Big Data như Kafka, Spark Streaming, Cassandra, Hadoop HDFS, NiFi.

## Cấu trúc thư mục
- `data/`: Chứa file log gốc (ví dụ: `actual_log.txt`).
- `docker compose/`: Cấu hình Docker Compose để khởi chạy các dịch vụ.
- `nasa_logs_analytics_pineline/`:
  - `log_listeners.ipynb`: Notebook thu thập và lắng nghe dữ liệu log.
  - `log_preprocessing.ipynb`: Notebook tiền xử lý, làm sạch dữ liệu log.
  - `log_visualizer.ipynb`: Notebook trực quan hóa kết quả phân tích log.

## Hướng dẫn sử dụng
1. **Clone repository**
2. **Cài đặt môi trường**
   - Sử dụng Docker Compose (xem `docker compose/docker-compose.yml`).
   - Hoặc cài đặt các package Python cần thiết thủ công.
3. **Chuẩn bị dữ liệu**
   - Đặt file log vào thư mục `data/`.
4. **Chạy các notebook**
   - Mở các notebook trong thư mục `nasa_logs_analytics_pineline/` bằng Jupyter Notebook hoặc Jupyter Lab.
   - Thực hiện tuần tự các bước trong từng notebook để xử lý và phân tích log.

## Yêu cầu
- Python 3.8 trở lên
- Jupyter Notebook hoặc Jupyter Lab
- Docker (tùy chọn, khuyến nghị dùng cho môi trường production)

## Giấy phép
MIT License
