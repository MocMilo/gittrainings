GIT USEFULL COMMANDS


HOW TO:

1) squash last 3 commits on YOUR LOCAL BRANCH

c:\repository> git reset --soft HEAD~3 &&
> git commit 

// edit comment to commit containing


2) Now you want to re-open this FEATURE_BRANCH to fix bug and not to have conflicts whit master that allready changed since last merge  
   Before bug fix You want to rebase your FEATURE_BRANCH to origin/master, and accept all the changes from master.

c:\repository> git rebase -s recursive -X theirs origin/master

