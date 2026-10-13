# Session 12 - Ex5: Chẩn đoán & khắc phục Pipeline CI/CD

## Lỗi 1 — Permission Denied (Gradle Wrapper)
- Hiện tượng: `./gradlew: Permission denied` khi chạy test trên runner.
- Nguyên nhân: file `gradlew` không có quyền thực thi (+x).
- Khắc phục: thêm bước `run: chmod +x gradlew` trước bước `./gradlew test`.

## Lỗi 2 — Authentication Failed (Docker Registry)
- Hiện tượng: `docker login` báo "Password required / unauthorized".
- Nguyên nhân: truyền sai tên biến secret (gõ nhầm/không tồn tại).
- Khắc phục: dùng đúng `${{ secrets.REGISTRY_TOKEN }}` (đúng chính tả, đã khai báo trong repo).

## Lỗi 3 — Build Context Error (không thấy Dockerfile)
- Hiện tượng: build báo "failed to read dockerfile / Dockerfile not found".
- Nguyên nhân: không chỉ định đúng thư mục build và file Dockerfile.
- Khắc phục: trong `docker/build-push-action@v5` khai báo `context: .` và `file: ./Dockerfile`.

## Kết quả
Sau khi sửa cả 3 lỗi, pipeline chạy xanh toàn bộ (test -> build -> login -> push).
