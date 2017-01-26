# test-git-svn

Create the svn repo
```
svnadmin create $HOME/svn_repos
svn mkdir file:///$HOME/svn_repos/test_git_svn/{trunk,branches,tags} -m "create project test_svn_git"
```

Checkout a git and svn working folder
```
svn co file:///$HOME/svn_repos/test_git_svn/trunk test_svn
git clone -s file:///$HOME/svn_repos/test_git_svn test_git
```

# file ops
Add a file in svn -> git
```
cd test_svn
touch a
svn add a
svn commit a -m "add a"

cd test_git
git svn rebase
```

Add a file in git- > svn
```
cd test_gi
touch b
git commit b -m "add b"
git dcommit 

cd test_svn
svn update
```

# branches
```
cd test_svn
svn copy  ^/test_git_svn/trunk ^/test_git_svn/branches/branch_a -m "create branch a"
svn switch ^/test_git_svn/branches/branch_a

cd test_git
git svn fetch
git checkout -b branch_a origin/branch_a
#

