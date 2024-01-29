# Các câu lệnh có thể hữu ích với bạn trong những trường hợp "khó khăn"

## Thay đổi base branch của một branch

Trong trường hợp bạn nhận ra đã tạo một brach từ một base branch khác ngoài ý muốn. Bạn có thể sử dụng command dưới đây để thay đổi base branch.

```bash
git rebase --onto {new_base_branch} {old_base_branch}
```

## Xem base branch gần nhất với nhánh hiện tại

Trong trường hợp bạn cần xác định base parent gần nhất với nhánh hiện tại, bạn có thể sử dụng command dưới đây.

```$
git show-branch -a \
| grep '\*' \
| grep -v `git rev-parse --abbrev-ref HEAD` \
| head -n1 \
| sed 's/.*\[\(.*\)\].*/\1/' \
| sed 's/[\^~].*//'
```

hoặc bạn có thể sử dụng command sau:

```bash
git show-branch | grep '*' | grep -v "$(git rev-parse --abbrev-ref @)" | head -1 | awk -F'[]~^[]' '{print $2}'
```

## Xoá commit với reset

Khi bạn muốn xoá commit ở dưới local và remote, bạn có thể thực hiện các bước dưới đây:

- Xác định số lượng "số lượng" commit mà bạn muốn thay đổi / xoá

```bash
git log --oneline
git reset HEAD~n (thay đổi giá trị n - số lượng mà bạn muốn)
```
- Thực hiện commit change dưới local và remote

```bash
git push {remote} {branch} -f
```

## Xóa branch trên local và remote

- Local

```bash
$ git branch -d {localBranchName}
```

- Remote

```bash
$ git push origin --delete {remoteBranchName}
```

## Thực hiện pull và replace local

```bash
$ git fetch --all & \
  git reset --hard {origin}/{branch_name}
```

## Thực hiện rollback sau khi git reset HEAD~n

```bash
$ git reflog
```

```bash
$ git reset --hard HEAD@{n}
```

## Thực hiện replace bằng 1 branch khác

Lấy toàn bộ code của branch khác thay thế cho branch hiện tại

```bash
$ git reset --hard {branch_name}
```

## Pull submodules together

Thực hiện pull các submodules có trong repository

```bash
$ git submodule update --init --recursive
```
