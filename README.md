# ClinicHub – Đặt lịch phòng khám

[![CI Pipeline](https://github.com/DuongQuocMinhAnh/IN421107-Nhom11/actions/workflows/ci.yml/badge.svg)](https://github.com/DuongQuocMinhAnh/IN421107-Nhom11/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Python Version](https://img.shields.io/badge/Python-3.11+-blue.svg)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.110+-009688.svg)](https://fastapi.tiangolo.com/)
[![Streamlit](https://img.shields.io/badge/Streamlit-1.32+-FF4B4B.svg)](https://streamlit.io/)

---

## 1. Tên Đề Tài & Giới Thiệu
- **Tên đề tài:** ClinicHub – Đặt lịch phòng khám
- **Mã học phần:** IN421107 - Nhóm 11
- **Kiến trúc áp dụng:**
  - **Backend:** Clean Architecture 4 lớp (Domain, Application, Infrastructure, Presentation) với **FastAPI**.
  - **Frontend:** **Streamlit Client** (giao diện tương tác chia theo vai trò người dùng).
  - **Cơ sở dữ liệu:** **PostgreSQL** & quản lý schema bằng **Alembic**.

---

## 2. Hướng Dẫn Chạy Bằng Docker

### Yêu cầu tiên quyết
- Cài đặt sẵn [Docker Desktop](https://www.docker.com/products/docker-desktop/) và Docker Compose (v2.x+).

### Các bước khởi chạy
1. **Thiết lập biến môi trường:**
   Tạo file cấu hình `.env` từ file mẫu:
   ```bash
   cp .env.example .env
   ```

2. **Khởi chạy toàn bộ hệ thống bằng Docker Compose:**
   Lệnh này sẽ build và chạy cùng lúc 3 container: Cơ sở dữ liệu (`db`), REST API (`api`), Giao diện người dùng (`ui`):
   ```bash
   docker compose up -d --build
   ```

3. **Kiểm tra trạng thái các container:**
   ```bash
   docker compose ps
   ```

4. **Dừng và dọn dẹp hệ thống khi cần:**
   ```bash
   docker compose down -v
   ```

---

## 3. Đường Dẫn Swagger & Các Cổng Dịch Vụ

Khi các container khởi chạy thành công, truy cập hệ thống qua các địa chỉ sau:

| Dịch vụ | Địa chỉ URL | Mô tả |
| :--- | :--- | :--- |
| **Swagger UI (Interactive API Docs)** | [http://localhost:8000/docs](http://localhost:8000/docs) | Tài liệu kiểm thử API trực quan |
| **ReDoc (Alternative Docs)** | [http://localhost:8000/redoc](http://localhost:8000/redoc) | Tài liệu API dạng ReDoc |
| **Streamlit Web App** | [http://localhost:8501](http://localhost:8501) | Giao diện tương tác người dùng |
| **PostgreSQL Database** | `localhost:5432` | Cổng kết nối CSDL nội bộ |

---

## 4. Tài Khoản Mẫu Cho Từng Vai Trò

Dưới đây là các tài khoản mặc định được seed sẵn trong môi trường phát triển:

| Vai trò (Role) | Tên đăng nhập / Email | Mật khẩu mặc định | Quyền hạn chính |
| :--- | :--- | :--- | :--- |
| **Admin (Quản trị viên)** | `admin@clinichub.vn` | `Admin@123456` | Quản lý người dùng, phân quyền bác sĩ, cài đặt hệ thống |
| **Doctor (Bác sĩ)** | `doctor@clinichub.vn` | `Doctor@123456` | Xem lịch khám, tiếp nhận bệnh nhân, kê đơn & cập nhật bệnh án |
| **Patient (Bệnh nhân)** | `patient@clinichub.vn` | `Patient@123456` | Đặt lịch khám, theo dõi kết quả khám bệnh, lịch sử đơn thuốc |

> **Lưu ý an toàn:** Hãy đổi mật khẩu ngay nếu triển khai hệ thống lên môi trường production thực tế.

---

## 5. Cách Chạy Kiểm Thử (Testing)

Dự án sử dụng **pytest** hỗ trợ kiểm thử đơn vị (Unit Tests) và kiểm thử tích hợp (Integration Tests).

### Chạy kiểm thử bên trong môi trường Docker
```bash
docker compose exec api pytest -v --cov=src --cov-report=term-missing
```

### Chạy kiểm thử trên máy cục bộ (Local Environment)
1. Kích hoạt môi trường ảo Python và cài đặt dependencies:
   ```bash
   cd backend
   python -m venv venv
   # Kích hoạt trên Windows PowerShell:
   .\venv\Scripts\Activate.ps1
   # Hoặc Linux/macOS:
   # source venv/bin/activate
   pip install -r requirements.txt
   ```

2. **Chạy toàn bộ test suites:**
   ```bash
   pytest
   ```

3. **Chạy riêng Unit Test (dùng Mock Repository):**
   ```bash
   pytest tests/unit
   ```

4. **Chạy Integration Test (kiểm thử các endpoints qua TestClient):**
   ```bash
   pytest tests/integration
   ```

---

## 6. Thông Tin Thành Viên Thực Hiện

| STT | Họ và Tên | MSSV | Vai trò | Nhiệm vụ đảm nhiệm |
| :---: | :--- | :---: | :--- | :--- |
| **1** | Dương Quốc Minh Anh | *(MSSV)* | Tác giả & Phát triển toàn bộ (Solo Fullstack) | Thiết kế kiến trúc Clean Architecture, xây dựng Backend REST API (FastAPI, Alembic, PostgreSQL), triển khai Frontend (Streamlit), cấu hình Docker, CI/CD, kiểm thử (Unit/Integration Test) và tài liệu dự án |

---

## 7. Cấu Trúc Thư Mục Tổng Quan

```text
clinichub/
├── .github/
│   └── workflows/
│       └── ci.yml                 # Pipeline GitHub Actions: chạy pytest, kiểm tra coverage & build Docker
├── backend/                       # Hệ thống lõi REST API áp dụng Clean Architecture 4 lớp
│   ├── alembic/                   # Quản lý các phiên bản migration cơ sở dữ liệu PostgreSQL
│   ├── src/
│   │   ├── domain/                # Tầng 1: Entities, Enums, Domain Exceptions (không phụ thuộc thư viện ngoài)
│   │   ├── application/           # Tầng 2: Use Cases / Services, Repository Interfaces, DTOs (Pydantic)
│   │   ├── infrastructure/        # Tầng 3: PostgreSQL Models, Repositories thực thi, Argon2id, JWT, QR
│   │   └── presentation/          # Tầng 4: FastAPI Routers (Controllers), DI Container, Exception Handlers
│   ├── tests/
│   │   ├── unit/                  # Kiểm thử đơn vị cho Domain & Application (dùng mock repository)
│   │   └── integration/           # Kiểm thử tích hợp các endpoint API bằng TestClient
│   ├── Dockerfile                 # Đóng gói môi trường chạy FastAPI
│   └── requirements.txt           # Danh sách thư viện Backend (fastapi, sqlalchemy, argon2-cffi, pyjwt, qrcode...)
├── frontend/                      # Giao diện người dùng tương tác (Streamlit Client)
│   ├── pages/                     # Các màn hình chức năng phân theo vai trò (Patient, Doctor, Admin)
│   ├── services/
│   │   └── api_client.py          # Lớp giao tiếp HTTP với Backend, tự động gắn JWT & xử lý lỗi ProblemDetails
│   ├── app.py                     # Điểm khởi chạy ứng dụng Streamlit, quản lý đăng nhập & điều hướng
│   ├── Dockerfile                 # Đóng gói môi trường chạy Streamlit
│   └── requirements.txt           # Danh sách thư viện Frontend (streamlit, httpx, pillow)
├── docker-compose.yml             # Khởi chạy đồng thời 3 container: db (PostgreSQL), api (FastAPI), ui (Streamlit)
├── .env.example                   # Mẫu biến môi trường (chuỗi kết nối DB, khóa bí mật JWT)
└── README.md                      # Tài liệu dự án
```
