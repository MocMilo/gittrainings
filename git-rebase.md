#### (STEP 1) NEW FEATURE DEVELOPMENT FLOW

Let's create new ***feature branch*** from branch **master**

~~~
$ git checkout master

[branch:master]$ git checkout -b JAV-3-next-feature
~~~
ok, I want to have copy of my local branch changes on remote repository, 

~~~
[branch:JAV-3-next-feature]$ git push 
~~~

**NOTE!:** ***The rest of commands in this guide are done in branch:*** **JAV-3-next-feature**

There is no such branch on remote. So I create my new branch JAV-3-next-feature on remote:

~~~
$ git push --set-upstream origin JAV-3-next-feature
~~~
I make changes on branch JAV-3-next-feature: I add file3.txt

~~~
$ git add file3.txt

$ git commit -m "[JAV-3] Added file3.txt"
~~~
I make changes on branch JAV-3-next-feature: I added 3 lines of code in file3.txt
~~~
$ git add file3.txt

$ git commit -m "[JAV-3] Added 3 lines of code to file3.txt."
~~~
I make changes on branch JAV-3-next-feature: I removed one last line of code from file3.txt
~~~
$ git add file3.txt

$ git commit -m "[JAV-3] Removed last line of code from file3.txt."
~~~

~~~
$ git status
~~~

~~~
desplays 3 commits:
        sha: zzzzzzzzzz "[JAV-3] Removed last line of code from file3.txt." 
        sha: yyyyyyyyyy "[JAV-3] Added 3 lines of code to file3.txt." 
        sha: xxxxxxxxxx "[JAV-3] Added file3.txt" 
~~~

Ok my code works locally, but realize: *"3 separate commits are not necessary"*. \
I want to squash these commits to one. 

**IMPORTANT:** I don't want to lose work. Just in case I want the next squash operation to be **reversible**. 

I push all changes to remote repository: 
~~~
$ git push
~~~

#### (STEP 2) SQUASHING COMMITS TO ONE COMMIT USING REBASE 

Now I have all my 3 commits from branch JAV-3-next-feature on remote repository \
Now I can start to squash my 3 commits to one locally \
My intention is also to merge with eventual commits new commits on branch **master** 

**Eventually:** this situation may result with conflicts to resolve.\
If I edited files that existed on master and these files were also changed simultaneously by another person. 
\
(in this case we added file3.txt so there was no possibility to change by anybody because file3.txt not exists in branch **master**

-i means *interactive* 

~~~
$ git rebase -i origin/master
~~~


GIT opens "editor window" or opens "Vim" or "Nano" with list of files: \
and displays 3 commits: 
 
~~~
(...)
        pick xxxxxxxxxx "[JAV-3] Added file3.txt" 
        pick yyyyyyyyyy "[JAV-3] Added 3 lines of code to file3.txt." 
        pick zzzzzzzzzz "[JAV-3] Removed last line of code from file3.txt."
(...)
        list of other options:
(...)
~~~

If I just close editor with no changes, nothing will happen to commits. 3 commits will remain the same. \
If I want to squash 2 last commits (yyyyyyyyyy and zzzzzzzzzz) to first one: xxxxxxxxxx I have to edit and save file. 

I change *pick* -> *squash* in front of commits sha: 

~~~
(...)
        pick xxxxxxxxxx "[JAV-3] Added file3.txt" 
        squash yyyyyyyyyy "[JAV-3] Added 3 lines of code to file3.txt." 
        squash zzzzzzzzzz "[JAV-3] Removed last line of code from file3.txt."
(...)
~~~

I save the file and exit the editor.

Then another editor window pops up. This window allow you to make "result commit message" \
I can reuse the messages from squashed commits. \
I Edit and save the commit.

Now I check the results of rebase: 

~~~
$ git log
~~~

I should see one commit with 3 messages (if you reused squashed commits messages) 

##### OPTIONAL: (STEP 2a) REVERING "COMMITS SQUASHED WITH REBASE" 

Ok, but if I would like revert local repo state with "squashed commits"?

Maybe some conflicts emerged when I rebased my JAV-3-next-feature branch... \
Maybe I generally failed to resolve the conflicts, I messed up something and I \
want to resolve conflicts from the beginning?
  
If I haven't pushed my ***"squashed commit"*** to remote repository. \
I can still easily revert the situation, because on remote repository I still have 3 separate commits on **JAV-3-next-feature** branch \
So, what I do:
~~~
$ git reset --hard HEAD^
~~~
Now on my local repository, I'm in last commit with state "before squashed changes were committed" \
Let's say I'm on on sha: **oooooooooooooooo** commit. \
I was moved to "first commit" of branch **JAV-3-next-feature** which is just the same as commit from branch **master** \
from where I "created **JAV-3-next-feature**". It is just a "clean" **JAV-3-next-feature** branch, with no changes.

Now I can just pull the 3 commits from remote repository.
~~~
$ git pull 
~~~
##### OPTIONAL: (STEP 2b) GOING BACK TO STATE BEFORE REVERTING REBASE COMMITS

Now situation on My remote and local repository is just the same (just as before rebase on my local repository). \
Ok, but IF I WANT to have "squashed commits" back again?! \
\
I reset to initial commit of **JAV-3-next-feature** 
~~~
$ git reset --hard oooooooooooooooo
~~~
Now I have to display **"GIT reflog"** to see ***history of last operations on git***.
~~~
$ git reflog
~~~

*GIT reflog* may look like this:
 
~~~
     4a7551f (HEAD -> JAV-3-another-feature, origin/master, master) HEAD@{0}: reset: moving to 4a755
     da07c34 (origin/JAV-3-another-feature) HEAD@{1}: reset: moving to HEAD
     da07c34 (origin/JAV-3-another-feature) HEAD@{2}: reset: moving to HEAD
     da07c34 (origin/JAV-3-another-feature) HEAD@{3}: reset: moving to HEAD
     da07c34 (origin/JAV-3-another-feature) HEAD@{4}: pull: Fast-forward
     4a7551f (HEAD -> JAV-3-another-feature, origin/master, master) HEAD@{5}: reset: moving to HEAD^
     fb74881 HEAD@{6}: rebase -i (finish): returning to refs/heads/JAV-3-another-feature
     fb74881 HEAD@{7}: rebase -i (squash): [JAV-3] Added 3 lines of code.
     5436c05 HEAD@{8}: rebase -i (start): checkout origin/master
     da07c34 (origin/JAV-3-another-feature) HEAD@{9}: checkout: moving from master to JAV-3-another-feature
     4a7551f (HEAD -> JAV-3-another-feature, origin/master, master) HEAD@{10}: checkout: moving from JAV-3-another-feature to master
     da07c34 (origin/JAV-3-another-feature) HEAD@{11}: commit: [JAV-3] Added next 2 lines of code.
     5436c05 HEAD@{12}: commit: [JAV-3] Added 3 lines of code.
     4a7551f (HEAD -> JAV-3-another-feature, origin/master, master) HEAD@{13}: checkout: moving from master to JAV-3-another-feature
     4a7551f (HEAD -> JAV-3-another-feature, origin/master, master) HEAD@{14}: pull: Fast-forward
     36149f8 HEAD@{15}: checkout: moving from JAV-2-another-feature to master
     cc02ce8 (origin/JAV-2-another-feature, JAV-2-another-feature) HEAD@{16}: rebase -i (finish): returning to refs/heads/JAV-2-another-feature
     cc02ce8 (origin/JAV-2-another-feature, JAV-2-another-feature) HEAD@{17}: rebase -i (squash): [JAV-2] Added file2.txt with 10 lines.
     701dd0d HEAD@{18}: rebase -i (squash): # This is a combination of 2 commits.
     83b2bf9 HEAD@{19}: rebase -i (start): checkout origin/master
     60b8fa8 HEAD@{20}: commit: [JAV-2] Removed last 3 lines.
     ea0785e HEAD@{21}: commit: [JAV-2] Removed first 3 lines.
     83b2bf9 HEAD@{22}: checkout: moving from JAV-1-feature to JAV-2-another-feature 
~~~

I look for commit sha that represents state with 3 squashed commits. It is in line:

**fb74881** HEAD@{6}: rebase -i **(finish)**: returning to refs/heads/JAV-3-another-feature

So I reset to this point of repo state:

~~~
$ git reset --hard fb74881
~~~
#### (STEP 3) FINAL STEP - WHEN REBASE WAS SUCCESSFUL 

ok, I'm finally happy with repository state on **JAV-3-another-feature**. \
Now I want to have squashed 3 commits in remote repository. 

By this way, for example: I can update my pull request: 

**JAV-3-another-feature -> master** 
 
after code review and "adding extra changes according to reviewers comments". 

~~~
$ git push -f
~~~

LINKS: 
* [How to rebase Pull Request?](https://github.com/edx/edx-platform/wiki/How-to-Rebase-a-Pull-Request)















