# Session 11 - Ex3: Tối ưu tốc độ CI bằng Gradle Dependency Caching

## Mục tiêu
Bật `cache: 'gradle'` trong bước `actions/setup-java@v5` để khôi phục thư viện
Gradle từ cache thay vì tải lại từ Internet mỗi lần CI chạy.

## Bảng so sánh thời gian

| Run | Mô tả                         | Trạng thái cache            | Thời gian Job |
|-----|-------------------------------|-----------------------------|---------------|
| #4  | Lần đầu sau khi bật cache     | Cache not found (tạo mới)   | 58s           |
| #5  | Lần sau (push commit mới)     | Cache restored successfully | 51s           |

**Mức giảm:** ~7 giây (~12%) so với lần đầu.

## Phân tích
- Ở run #4, chưa có cache nên bước setup-java tải và lưu dependencies Gradle lần đầu.
- Ở run #5, bước setup-java hiện dòng "Cache restored successfully from key: gradle-...",
  khôi phục thư viện từ cache nên không phải tải lại → thời gian Job giảm.
- Với dự án nhỏ (ít dependency), mức giảm còn khiêm tốn; dự án càng nhiều thư viện,
  caching càng tiết kiệm thời gian rõ rệt.

## Cấu hình cache
```yaml
- name: Thiết lập môi trường Java JDK 17
  uses: actions/setup-java@v5
  with:
    java-version: '17'
    distribution: 'temurin'
    cache: 'gradle'
