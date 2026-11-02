---
name: github
description: 'Quy trình làm việc với Git và GitHub trong VS Code: repository, branch, commit, push, pull, issue, pull request, review, merge, GitHub Actions, CI/CD và GitHub API. Dùng khi cần tạo, cập nhật, kiểm tra hoặc phát hành thay đổi trên GitHub.'
argument-hint: 'Mô tả repository, branch hoặc tác vụ GitHub cần thực hiện.'
user-invocable: true
---

# GitHub Workflow

## Khi sử dụng
- Thiết lập hoặc kiểm tra repository GitHub trong VS Code.
- Tạo và quản lý branch, commit, push, pull và tag.
- Tạo, cập nhật, review và merge Issue hoặc Pull Request.
- Thiết kế hoặc kiểm tra GitHub Actions và CI/CD.
- Làm việc với GitHub REST/GraphQL API.

## Nguyên tắc an toàn
1. Xác nhận repository, branch đích, phạm vi file và quyền thao tác trước khi thay đổi.
2. Không yêu cầu, ghi log hoặc commit password, token, SSH private key, dữ liệu bệnh nhân hoặc dữ liệu cá nhân không cần thiết.
3. Không dùng `git reset --hard`, `git checkout --`, force-push hoặc thao tác xóa nếu chưa có xác nhận rõ ràng.
4. Kiểm tra `git status`, diff và branch hiện tại trước commit hoặc push.
5. Không tuyên bố push, merge hoặc test thành công nếu chưa có output xác nhận.
6. Ưu tiên branch riêng và Pull Request thay vì commit trực tiếp vào `main`.

## Quy trình Git trong VS Code
1. Mở đúng workspace và Terminal tại thư mục repository.
2. Kiểm tra trạng thái:
   ```powershell
   git status
   git remote -v
   git branch --show-current
   ```
3. Tạo branch:
   ```powershell
   git switch -c <feature-or-fix-branch>
   ```
4. Xem thay đổi và stage đúng file:
   ```powershell
   git diff
   git add <path>
   git diff --cached
   ```
5. Commit message ngắn, mô tả mục đích:
   ```powershell
   git commit -m "docs: update project instructions"
   ```
6. Push branch:
   ```powershell
   git push -u origin <feature-or-fix-branch>
   ```
7. Tạo Pull Request vào `main`, mô tả Summary, Testing, Risks và rollback nếu cần.

## Issue và Pull Request
- Issue phải có title rõ ràng, context, reproduction steps, expected result, actual result, evidence và acceptance criteria khi phù hợp.
- Pull Request phải nêu mục tiêu, phạm vi file, test đã chạy, kết quả thực tế, risk và migration/deployment notes nếu có.
- Kiểm tra base branch, compare branch, changed files, review comments, required checks và merge conflicts.
- Chỉ merge khi required checks và review theo repository policy đã hoàn tất.
- Sau merge, kiểm tra branch, deployment hoặc workflow result theo yêu cầu của repository.

## Review checklist
- Đúng scope và không có file ngoài ý muốn.
- Không có secret hoặc thông tin nhạy cảm.
- Input validation, authorization, error handling và logging phù hợp.
- Test cover happy path, invalid input, duplicate/retry, failure/recovery và regression khi liên quan.
- Documentation và configuration đồng bộ với thay đổi.

## GitHub Actions và CI/CD
- Đọc workflow hiện có trước khi tạo workflow mới.
- Xác định event trigger, permissions tối thiểu, jobs, dependencies, cache, artifact, environment và required secrets.
- Pin action theo version/tag đáng tin cậy; không in secret ra log.
- Dùng `pull_request` cho validation PR và `push` cho branch được quy định.
- Kiểm tra YAML syntax, job dependency, exit code, artifact retention và failure notification.
- Không tự đoán tên secret, runner, environment hoặc deployment target; đánh dấu Clarification Required khi thiếu contract.

Mẫu workflow tối thiểu:
```yaml
name: CI
on:
  pull_request:
  push:
    branches: [main]

permissions:
  contents: read

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run project validation
        run: <replace-with-project-command>
```

## GitHub API
- Xác định REST hay GraphQL, endpoint/query, method, authentication scope, request schema, response schema, pagination, rate limit và error mapping.
- Dùng token qua secret store hoặc environment; không ghi token vào source, URL, issue, log hoặc chat.
- Kiểm tra status code, headers, body, retry/backoff và idempotency theo API contract.
- Với API chưa có contract, ghi rõ Assumption hoặc Clarification Required thay vì tự tạo expected result.

## Báo cáo kết quả
Phân biệt rõ:
- **Proposed**: mới đề xuất.
- **Written**: đã tạo hoặc sửa file.
- **Executed**: lệnh/workflow đã chạy và có output.
- **Verified**: bằng chứng xác nhận kết quả.

Khi không thể thực hiện do Git chưa cài, chưa đăng nhập, thiếu quyền hoặc thiếu repository, nêu chính xác blocker và hướng dẫn bước tiếp theo; không tạo success-shaped fallback.
