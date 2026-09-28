# Thông Tin Deploy — Checkpoint 5

> **Không ghi giá trị API key, token hoặc Redis URL thật vào tài liệu công khai này.**

## Thông Tin Học Viên

| Mục | Nội dung |
|-----|----------|
| Họ và tên | Đỗ Đình Hoàn |
| Mã học viên | 2A202602377 |
| Repo | https://github.com/dinhhoan-04/K4-L3A-DAY12-DoDinhHoan-2A202602377-CloudServicesAndDeployment |

## Service

| Mục | Nội dung |
|-----|----------|
| Public URL | https://pleasant-insight-production.up.railway.app |
| Platform | Railway |
| Ngày deploy | 2026-09-29 |

## Biến Môi Trường Đã Set Trên Cloud

| Biến | Đã set | Ghi chú |
|------|--------|---------|
| `PORT` | Railway tự gán | Không hardcode trong image. |
| `AGENT_API_KEY` | Có | Đặt trong Railway Variables; không nằm trong repo. |
| `REDIS_URL` | Có | Reference tới `Redis.REDIS_URL` trong cùng Railway project. |
| `RATE_LIMIT_PER_MINUTE` | Có | `10` |
| `MONTHLY_BUDGET_USD` | Có | `10.0` |
| `LOG_LEVEL` | Có | `INFO` |

## Lệnh Kiểm Tra

```powershell
curl.exe -i https://pleasant-insight-production.up.railway.app/health
curl.exe -i https://pleasant-insight-production.up.railway.app/ready

$body = @{ question = 'Hello' } | ConvertTo-Json -Compress
Invoke-WebRequest -Uri 'https://pleasant-insight-production.up.railway.app/ask' `
  -Method Post -ContentType 'application/json' -Body $body
```

## Kết Quả Chạy Thật

- `GET /health` → `200 OK` với `{"status":"ok","service":"day12-agent","version":"1.0.0"}`.
- `GET /ready` → `200 OK` với `{"status":"ready","redis":true}`.
- `POST /ask` không có `X-API-Key` → `401 Unauthorized` với `{"detail":"invalid or missing API key"}`.
- Kiểm tra có API key và rate limit được thực hiện cục bộ bằng secret trong Railway Variables; secret không được ghi vào repository.

## Ảnh Chụp Màn Hình

- `screenshots/dashboard.png` — Railway project có service `pleasant-insight` và `Redis` online.
- `screenshots/health.png` — kết quả public URL `/health` trả `200 OK`.
