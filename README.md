# test-subtree-parent

- [test-subtree-child](https://github.com/richellin/test-subtree-child)
- [test-submodule-child](https://github.com/richellin/test-submodule-child)

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

### Submodule

```
# Subtree Parent Repository
git submodule add git@github.com:richellin/test-submodule-child.git childs/subtree-child/submodule-child
git commit -m "add submodule"
git push -u origin main
git subtree push --prefix=childs/subtree-child --squash subtree-child main

# Subtree Child Repository
git pull
# git submodule update --init --recursive
git submodule update

上記のようにParentのRoot Directoryで追加すると.gitmodulesが subtreeの中ではなくRoot Directoryにあるためエラーとなる。

# Subtree Parent Repository
# .gitmodulesをコピーして入れるようにする
cp .gitmodules childs/subtree-child

# pathを全て変更する
vi .gitmodules childs/subtree-child/submodule-child
childs/subtree-child/submodule-child　→ submodule-child

git commit -m "cp submodule in subtree child"
git push -u origin main
git subtree push --prefix=childs/subtree-child --squash subtree-child main

# Subtree Child Repository
git pull
git submodule update --init --recursive
```

### submodule childを変更する

```bash
# Subtree Parent Repository
cd childs/subtree-child/submodule-child/
git checkout main
git pull

cd ../../../
git add childs/subtree-child/submodule-child
git commit -m "update submodule"
git push -u origin main
git subtree push --prefix=childs/subtree-child --squash subtree-child main
```

## 考察

この仕組みのデメリットは

- 無理やりにsubtreeの中にsubmoduleを入れてあげなければいけない作業が発生
