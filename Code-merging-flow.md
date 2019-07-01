When you want to develop an awesome feature. Here are steps to merge your code to master (production)

## 0. You need at least master branch, and it's updated
```
cd /path/to/your/master
git pull origin master
```

## 1. Create new branch and push it to git repo
```
git checkout -b your_branch
git push origin your_branch

# check if you created correct branch
git branch
```

Tips: if you want another directory to store your new branch, use this
```
# update master, then copy master to another directory
cd /path/to/your/master
git pull origin master

# copy to another directory
cd ..
cp -r master your_branch
cd your_branch
git checkout -b your_branch
git push origin your_branch

# check to make sure you created right branch
git branch
```

## 2. Develop feature on your branch, commit and push all changes
```
cd /path/to/your_branch
# do stuffs
# commit & push
git add all_your_changed_files
git commit -m "your commit message"
git push origin your_branch
```

## 3. Test code (performance and quality)
```
cd /path/to/your_branch
# do testing, make sure test result is okay
```

## 4. Merge lastest changes from master to your branch
```
# merge from master to your branch
cd /path/to/your_branch
git merge master

## resolve conflict, commit and push it
git add changes_from_master
git commit -m "merge lastest changes from master"
git push origin your_branch
```

## 5. Test code again
```
cd /path/to/your_branch
# do testing, make sure test result is okay
```

## 6. Merge your code to master
```
# be sure to update master
cd /path/to/master
git checkout master
git pull origin master

# merge code from your branch
# we highly recommend use -no-ff, check detail https://git-scm.com/docs/git-merge
git merge -no-ff your_branch

# resolve conflict
```

## 7. Test code again
```
cd /path/to/master
# do testing, make sure test result is okay
```

## 8. Commit, push code and you're done
```
cd /path/to/master
git add changes_from_your_branch
git commit -m "your commit message"
git push origin master
```

## NOTE: keep your branch alive after merging