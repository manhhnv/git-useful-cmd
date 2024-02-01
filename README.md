# Các câu lệnh có thể hữu ích với bạn trong những trường hợp "khó khăn"

- [Các câu lệnh có thể hữu ích với bạn trong những trường hợp "khó khăn"](#các-câu-lệnh-có-thể-hữu-ích-với-bạn-trong-những-trường-hợp-khó-khăn)
  - [Thay đổi base branch của một branch](#thay-đổi-base-branch-của-một-branch)
  - [Xem base branch gần nhất với nhánh hiện tại](#xem-base-branch-gần-nhất-với-nhánh-hiện-tại)
  - [Xoá commit với reset](#xoá-commit-với-reset)
  - [Xóa branch trên local và remote](#xóa-branch-trên-local-và-remote)
  - [Thực hiện pull và replace local](#thực-hiện-pull-và-replace-local)
  - [Thực hiện rollback sau khi git reset HEAD~n](#thực-hiện-rollback-sau-khi-git-reset-headn)
  - [Thực hiện replace bằng 1 branch khác](#thực-hiện-replace-bằng-1-branch-khác)
  - [Pull submodules together](#pull-submodules-together)
  - [Di chuyển n commit sang nhánh mới](#di-chuyển-n-commit-sang-nhánh-mới)


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

## Di chuyển n commit sang nhánh mới

Giả sử bạn đang ở nhánh A và thực hiện push 5 commit lên remote.

Bây giờ, yêu cầu đặt ra bạn hãy chuyển 5 commit đó sang nhánh mới và xoá 

5 commit đó khỏi nhánh hiện tại.

Có nhiều cách để làm điều này như: kết hợp reset, stash hay kết hợp cherry-pick và revert

Hướng dẫn sau đây sẽ thực hiện điều trên bằng cách kết hợp cherry-pick và revert

Tại nhánh cũ bạn cần tạo một nhánh mới

```bash
$ git checkout -b {new_branch}
```

Thực hiện xem log commit

```bash
$ git log
```

Xác định các commit cần di chuyển sang nhánh mới

```bash
$ git cherry-pick {commit_hash1} {commit_hash2},...
```

Thực hiện cập nhật nhánh mới lên remote

```bash
$ git push origin {new_branch}
```

Chuyển qua nhánh cũ

```bash
$ git checkout {old_branch}
```

Xác định commit hash trước khi commit các commit trên

Thực hiện xoá commit

```bash
$ git revert {commit_hash}
```

Thực hiện đồng bộ local và remote cho branch cũ

```bash
$ git push origin {old_branch} --force-with-lease
```
