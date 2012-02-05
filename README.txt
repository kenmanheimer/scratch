2012-02-04
==========
1. Created scratch repo fresh on github:
   https://github.com/kenmanheimer/scratch
2. Following instructions presented on the resulting page
   - mkdir scratch
   - cd scratch
   - git init
   - touch README [doing README.txt]
   - git add README [README.txt] [could use Emacs magit-status 'add command']
     [to remove it before commit, 'git rm -f README']
   - git commit -m 'first commit'
   - git remote add origin git@github.com:kenmanheimer/scratch.git
   - git push -u origin master

3. Work on a branch:
   - git branch first_branch
     [now 'git branch' reveals new branch, but my checkout is on master]
   - git checkout first_branch
     [now 'git branch' reveals my checkout is on master]
   - git commit -m "branched work" README.txt
     *or* 
     git commit -a -m "branched work"
     ['-a' to add all found changes]

4. Switch back to master to lose file changes, then back to branch to see them.
   - git checkout master
   - git checkout branch
   'git checkout' refuses to switch away from branch with uncommitted changes.
   But not always - need to determine why, sometimes and not others.

5. Pull the changes from the branch to the master:
   - git checkout master
     [stuff committed only on first_branch is not present in master branch]
   - git pull . first_branch
     [stuff committed only on first_branch is now present in master branch]

6. Stuff edited on master can be carried to branch without commits.
   - [Edit README.txt while on master branch, and don't commit.]
   - git checkout first_branch
     [README.txt shows as Modified - carried to be committed in first_branch]
   ... and vice versa?!  I can now switch back and forth between master and
   first_branch with changes in README.txt.  Not sure why I can now and not
   before.

Some questions.
   - Can I test push/pull with my own repository?
   - Is manual setup of 2. equivalent to what would have happened had I cloned?
