## Git Tutorial (Practical Cheat Sheet)

### 1) Install & Configure
- Install: https://git-scm.com/downloads
- Configure identity:
```
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
```

### 2) Create or Clone a Repo
```
git init               # start new repo
git clone <url>        # clone existing repo
```

### 3) Daily Basics
```
git status             # what changed?
git add <file|.>       # stage changes
git commit -m "msg"    # commit staged changes
git log --oneline --graph --decorate --all
```

### 4) Branching & Merging
```
git switch -c feature/awesome     # create & switch
git switch main                   # switch back
git merge feature/awesome         # merge into current
```

### 5) Rebase (Clean History)
```
git switch feature/awesome
git fetch origin
git rebase origin/main
# resolve conflicts → git add <fixed files>
git rebase --continue
```

### 6) Remotes, Pull, Push
```
git remote -v
git remote add origin <url>
git pull --rebase origin main
git push -u origin main
```

### 7) Stash Work-in-Progress
```
git stash            # stash tracked changes
git stash list
git stash pop        # re-apply and drop
```

### 8) Undo/Reset
```
git restore <file>                 # discard unstaged local changes
git restore --staged <file>        # unstage
git reset --hard <commit>          # reset branch (DANGEROUS)
```

### 9) Tags & Releases
```
git tag v1.0.0
git push origin v1.0.0
```

### 10) .gitignore
Create `.gitignore` to exclude files:
```
build/
.env
*.log
```

### 11) Pull Requests (PR) Flow
1. Create feature branch
2. Commit small, focused changes
3. Push branch and open PR
4. Request review, address feedback
5. Squash merge to keep history clean

### 12) Signing Commits (Optional)
```
git config --global commit.gpgSign true
```

### 13) Helpful Aliases
```
git config --global alias.lg "log --oneline --graph --decorate --all"
git config --global alias.s "status -sb"
```

### 14) Common Patterns
- Feature branches: `feature/<name>`
- Bugfix branches: `fix/<issue-id>`
- Hotfix to production: `hotfix/<id>`

### 15) Tips
- Commit early, commit often with clear messages
- Rebase local branches to keep linear history
- Never rebase shared branches unless everyone agrees



### Flutter-specific Git Tips

- Add Flutter/Android/iOS artifacts to `.gitignore`:

```
# Flutter
.dart_tool/
.flutter-plugins
.flutter-plugins-dependencies
.packages
pubspec.lock # consider committing for apps, ignore for packages
build/

# Android
**/android/**/local.properties
**/android/**/key.properties

# iOS
**/ios/Pods/
**/ios/Runner.xcworkspace/
**/ios/**/*.xcuserstate
**/ios/**/*.xcuserdata/
```

- Use branches per feature: `feature/ui-login`, `feature/http-client`, etc.
- Run `flutter format` and `flutter analyze` before committing; consider a pre-commit hook.
- Keep secrets out of git; use `.env` + runtime config or CI secrets.
