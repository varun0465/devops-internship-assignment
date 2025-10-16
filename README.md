###part 1 - Git & SSH


###Difference b/w git pull and git fetch

-----------------------------------
git fetch  ---> Only downloads changes from the remote repository tp local 
                repository. It updates your remote tracking branches but does                 not modify your working files

we use command "git fetch origin"


git pull  ---> The git pull basically is  "git fetch + git merge".
               It not only downloads the changes but also automatically merge
               them into current branch.

We use command "git pull origin main"



####** How to Reslove a Merge Conflict (Example Scenario) **


1. Suppose branch 'feature-A' added 'conflict.txt' with "Line from feature-A"    and 'feature-B' added the same file with different content.
2. When merging 'feature-B' into 'main' (which already has 'feature-A), Git     will mark 'conflict.txt' with conflict markers '<<<<<', '=======', '>>>>>'.
3. To resolve:
   - Open the file, choose or combine the changes.
   - git add conflict.txt
   - git commit -m "resolve merge conflict"


