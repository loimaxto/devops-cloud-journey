


git pull
git pull -rebase ( use to avoid merge commit)

### Temporarilly change
git stash ( hide working changes to move to other branches)
git stash pop

**Use cases**
- When you want to temporarily save current working branch to move to other branches
- When you want to temporarily save current file version to test new modify

### Undoing commit
git commit --amend ( merge new modify to the latest commit)
git revert <commit-id> (create new commit to previous commit)
git reset HEAD~1 ( back to the latest commit)