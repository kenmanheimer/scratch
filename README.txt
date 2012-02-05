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

4. Some questions.
   - Can I test push/pull with my own repository?
   - Is manual setup of 2. equivalent to what would have happened had I cloned?
