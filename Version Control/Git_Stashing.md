Git stashing is a way to temporarily save (or "shelve") changes in your working directory and staging area without committing them.

👉 Think of it like putting your unfinished work in a safe box, so you can switch branches or work on something else without losing your progress. Later, you can take the changes back out and continue working.

🔹 Why use git stash?

When you have uncommitted changes but need to switch branches quickly.

To pull new updates from remote without committing incomplete code.

To keep your working directory clean temporarily.

🔹 Common Commands:

Save your changes

git stash


or with a message:

git stash save "work in progress"


List stashed changes

git stash list


Apply the latest stash (keep stash in list)

git stash apply


Apply and remove from stash

git stash pop


Drop a stash (delete specific stash)

git stash drop stash@{0}


Clear all stashes

git stash clear


📌 Example workflow:

# You’re working on a file but need to switch branches
git stash        # save unfinished work
git checkout main
git pull        
git checkout feature-branch
git stash pop    # get back your work
