# git-multi-branch-operations
Playground for git multi-branch operations

# Reflog

```sh
echo "First line" > feature1/file1.txt
echo "First line" > feature1/file2.txt
git add feature1/
git commit -m "First commit for the feature1"
git push origin feature-branch
```

==

```sh
git checkout main
echo "First line" > main_feature/file1.txt
echo "First line" > main_feature/file2.txt
git add main_feature/
git commit -m "First commit for the main_feature"
git push origin main
```

==

```sh
echo "Second line" >> main_feature/file2.txt
git add main_feature/
git commit -m "Second commit for the main_feature"
git push origin main
```

==

```sh
git checkout feature-branch
echo "Second line" >> feature1/file2.txt
git add feature1/
git commit -m "Second commit for the feature1"
git push origin feature-branch
```

Merge to main
==

```sh
git rebase origin/main
git checkout main
git merge feature-branch
git push origin main
```

Fixes
==

```sh
git checkout -b fixes
echo "Brand new file2 content - fix" > file2.txt
git add file2.txt
git commit -m "Fixed file2.txt in fixes branch"
git push origin fixes
```

==

```sh
git checkout main
echo "Second line" >> file3.txt
git add file3.txt
git commit -m "Extended file3.txt in main"
git push origin main
```

==

```sh
echo "First line" > file6.txt
git add file6.txt
git commit -m "Added new file in main"
git push origin main
```

==

```sh
git checkout fixes
# Added " + some fixes" into first line of file3.txt
git add file3.txt
git commit -m "Fixed file3.txt (conflicting with main)"
git push origin fixes
```

Updating not yet finished conflicting feature "fixes"
==

```sh
git checkout main
git checkout -b tmp_main
git rebase origin/fixes
# Resolve conflict in file3.txt
git add file3.txt
git rebase --continue
git checkout fixes
git merge tmp_main
git push origin fixes
```

Main branch and fixes lives their lifes
==

```sh
echo "Small fix" >> file5.txt
git add file5.txt
git commit -m "Added non conflicting fix"
git push origin fixes
git checkout main
```

==

```sh
echo "Second line" >> main_feature/file1.txt
git add main_feature/
git commit -m "Added new non conflictin feature in mian"
git push origin main
```

Final merging to main and deleting fixes branch
Note: Check what happens with just `git merge` without rebasing it first
==

```sh
git checkout fixes
git checkout -b tmp_fixes
git rebase origin/main
# Resolve conflicts
git add file3.txt
git rebase --continue

git checkout main
git merge tmp_fixes
git commit -m "Merged fixes into main"
git push origin main

git branch -D tmp_main
git branch -D tmp_fixes
git branch -D fixes
git push origin --delete fixes
```
