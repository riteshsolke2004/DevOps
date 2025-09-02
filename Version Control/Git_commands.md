🖥️ Git Bash Commands – Complete Guide
🔹 Setup & Configuration

git --version → Check installed Git version

git config --global user.name "Your Name" → Set username

git config --global user.email "you@example.com" → Set email

git config --list → View current Git config

🔹 Getting Started

git init → Initialize a new Git repository

git clone <repo-url> → Clone (download) a remote repository

git help <command> → Get help for a Git command

🔹 Staging & Committing

git status → Show current status of repo (staged/unstaged files)

git add <file> → Add specific file to staging area

git add . → Add all files to staging area

git reset <file> → Unstage a file (keep changes)

git commit -m "message" → Save snapshot with message

git commit -am "message" → Add & commit tracked files in one step

🔹 Branching

git branch → List all branches

git branch <name> → Create a new branch

git checkout <branch> → Switch to a branch

git checkout -b <branch> → Create & switch to a new branch

git merge <branch> → Merge another branch into current

git branch -d <branch> → Delete branch

🔹 Working with Remotes

git remote -v → List remote connections

git remote add origin <url> → Add a remote repo

git push origin <branch> → Push branch to remote

git push -u origin <branch> → Push and set upstream branch

git pull origin <branch> → Fetch & merge changes from remote

git fetch origin → Download remote changes (without merging)

🔹 Undoing Changes

git checkout -- <file> → Discard local changes in a file

git reset --hard HEAD → Reset repo to last commit (delete all changes)

git reset --soft HEAD~1 → Undo last commit but keep changes staged

git revert <commit-id> → Create a new commit that undoes changes

🔹 Logs & History

git log → Show commit history

git log --oneline → Compact commit history

git show <commit-id> → Show details of a commit

git diff → Show unstaged changes

git diff --staged → Show staged changes

🔹 Tags

git tag → List tags

git tag <name> → Create a new tag

git push origin <tag> → Push tag to remote

🔹 Stash (Temporary Save)

git stash → Save changes temporarily

git stash list → View stashed changes

git stash pop → Apply latest stash & remove it

git stash apply → Apply stash but keep it in stash list

🔹 Advanced / Cleanup

git rm <file> → Remove file from repo & working directory

git mv <old> <new> → Rename or move a file

git clean -f → Remove untracked files

git reflog → Show history of all HEAD movements

✅ Key Takeaway:
Git commands can be grouped into:

Setup

Staging & Committing

Branching

Remote Collaboration

Undo & History

Tags & Stash
