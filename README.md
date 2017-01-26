# git-svn 

Create the svn repo
```
svnadmin create $HOME/svn_repos
svn mkdir --parents file:///$HOME/svn_repos/test_git_svn/{trunk,branches,tags} -m "create project test_svn_git"
```

Checkout a git and svn working folder
```
svn co file:///$HOME/svn_repos/test_git_svn/trunk test_svn
git svn clone -s file:///$HOME/svn_repos/test_git_svn test_git
```

# file ops
Add a file in svn -> git
```
cd $HOME/test_svn
touch a
svn add a
svn commit -m "add a"

cd $HOME/test_git
git svn rebase
```

Add a file in git- > svn
```
cd $HOME/test_git
touch b
git add b 
git commit -m "add b"
git svn dcommit 

cd $HOME/test_svn
svn update
```

# branches
branch svn -> git
```
cd $HOME/test_svn
svn copy  ^/test_git_svn/trunk ^/test_git_svn/branches/branch_a -m "create branch a"
svn switch ^/test_git_svn/branches/branch_a
svn info

cd $HOME/test_git
git svn fetch
git checkout -b branch_a origin/branch_a
```

branch git -> svn
```
cd $HOME/test_git
git checkout master
git svn branch branch_b -m "branch b"
git checkout -b branch_b origin/branch_b
git svn info
touch c
git add c 
git commit -m "adding c"
git svn dcommit
git svn info

cd $HOME/test_svn
svn switch ^/test_git_svn/branches/branch_b
echo "verify file c exists"
```

# workflows

## local git branch dcommit - merge commit 
Commiting a merge squashed the all the commits on the merged branch

```
cd $HOME/test_git
git checkout master
git svn branch feature_a -m "feature a"
git checkout -b feature_a origin/feature_a
touch e 
git add e 
git commit e -m "add e"
touch f 
git add f
git commit f -m "add f"
git rebase master
git checkout master
```

```
git merge --no-ff feature_a -m "feature a work"
```

```
git svn dcommit

cd $HOME/test_svn
svn switch ^/test_git_svn/trunk
svn update
svn log
echo "all commits squashed into one svn commit
```

## revert merge
```
cd $HOME/test_git
git revert HEAD -m 1
git svn decommit

cd $HOME/test_svn
svn update
svn log
```

## local git branch dcommit - rebased first
Moving all commits to the target branch to create a linear history

```
cd $HOME/test_git
git checkout master
git svn branch feature_b -m "feature b"
git checkout -b feature_b origin/feature_b
touch g h
git commit g -m "add g"
git commit h -m "add h"
git rebase master
git checkout master
```

```
git merge --ff-only feature_a -m "feature a work"
```

```
git svn dcommit
cd $HOME/test_svn
svn switch ^/test_git_svn/trunk
svn update
svn log
echo "all commits squashed into one svn commit
```
