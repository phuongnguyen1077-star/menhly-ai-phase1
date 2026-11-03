# MỆNH LÝ AI — Phase 1 (Foundation)

## Chạy
```bash
cp .env.example .env            # đặt DB_PASSWORD
docker compose up --build       # db + redis + api (tự migrate)
make test                       # toàn bộ test trong Docker (gồm test API FastAPI)
```
Không dùng Docker: cần PostgreSQL ≥14 và `psql`:
```bash
export DATABASE_URL="postgresql://user:pw@host/menhly"
cd backend && PYTHONPATH=src python -m menhly.db.migrate
PYTHONPATH=src python -m menhly.knowledge.ingest_cli /path/book.txt --title "TV34" --school tu_vi_nam_phai
TEST_DATABASE_URL="postgresql://user:pw@host/postgres" pytest -q
```
## Trạng thái kiểm thử (đã chạy trong sandbox)
23 passed, 1 skipped (`test_api.py` — FastAPI không cài được ở sandbox; chạy trong Docker).
Docker/compose **chưa được chạy** (sandbox không có daemon); `docker compose config` hợp lệ.

## Thư viện tri thức hiện có
| Nguồn | Đoạn | Trang |
|---|---|---|
| TV34 Tuyển Tập Mệnh Lý Thiên Cơ – Tử Vi Nam Phái (Lê Quang Lăng) | 1.227 | chưa có (PENDING_VERIFICATION) |
| Tử Vi Đẩu Số Toàn Thư (Hi Di Trần Đoàn; Vũ Tài Lục dịch & bình chú) | 247 | 1–169 |

Nạp thêm tài liệu (tự nhận diện & đổi bảng mã VNI cũ sang Unicode):
`python -m menhly.knowledge.ingest_cli FILE --title T --school S [--page-re 'REGEX_SỐ_TRANG'] [--upper-headings]`
Bản Unicode của Toàn Thư: `data/corpus/tu_vi_dau_so_toan_thu_vu_tai_luc.utf8.txt` (giữ nguyên chính tả cũ của nguồn, vd "tòan").
