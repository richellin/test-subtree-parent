# test-subtree-parent

## 手順

### ターゲットを登録
```
# 追加
git remote add subtree-child git@github.com:richellin/test-subtree-child.git

# 確認
git remote
```

### ソースコードを取得
```
# 取得 + コミット
git subtree add --prefix=childs/subtree-child --squash subtree-child main

以下のように履歴が追加される。
commit f9c9a56aa7780e24b3d701cf3bc9a51a6a74c11f (HEAD -> main)
Merge: e10533f f70be75
Author: richellin <email>
Date:   Mon Jun 7 20:21:37 2021 +0900

    Merge commit 'f70be75236cefa31552e8d878c7ef990a2e371c7' as 'childs/subtree-child'

commit f70be75236cefa31552e8d878c7ef990a2e371c7
Author: richellin <email>
Date:   Mon Jun 7 20:21:37 2021 +0900

    Squashed 'childs/subtree-child/' content from commit 56c99be

    git-subtree-dir: childs/subtree-child
    git-subtree-split: 56c99bef61f4405709dd2a92289b91cb5fa6c451

# 確認
ls -al childs/subtree-child

# リモートへPush
git push origin main
```

### 親から子供に反映

```
git subtree push --prefix=childs/subtree-child --squash subtree-child main
```
