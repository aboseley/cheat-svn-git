# test-git-svn

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

cd $HOME/test_git
svn switch ^/test_git_svn/branches/branch_b
echo "verify file c exists"
```
