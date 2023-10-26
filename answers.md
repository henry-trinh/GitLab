# Quiz questions

This is only a "quiz" in the loosest sense that it's asking questions whose
answers will be part of your grade. Please use *any resources you want*, as
long as you list those resources (e.g. peers, websites, etc.)

## Navigating logs

1. What is the SHA for the last commit made by Prof. Xanda on the branch
xanda_0000_movie_processing?
(For this and future questions, the first 5 characters is plenty - neither
Git nor I need the whole SHA.)

We run the command "git log xanda_0000_movie_processing" to get the corresponding SHA: 9b2571f9. Note that the branch was stale, so my git commands did not acknowledge its existence for some time.

2. What is the SHA for the last commit associated with line 9 of this file?

We run the command "git blame -L 9,9 quiz.md" to get the corresponding SHA: b2ed39de.

3. What did line 12 of this file say in commit d1d83?

We run the command "git blame -L 12,12 quiz.md d1d83" to get the corresponding line: "2. I should really finish writing this."

4. What changed between commit e474c and 82045?

Between the two commits, the following changes were found using the command "git diff e474c 82045":

     # Sort data and get top 5
-    gross_sort = lambda x : x["Gross"]
+    gross_sort = lambda x : int(x["Gross"])
     rows.sort(key=gross_sort)
-    top_five = rows[:-5:-1]
+    top_five = rows[:-6:-1]

In the first change, there was probably a subscriptable error initially, so we explicitly convert x["Gross"] to an int-type. In the second change, we change the ending index by one value. This was done to retrieve 5 values instead of 4 (the original code was an error!)

## Predicting merges

Assume at the start of each of these three questions that your
branch for switching to a top-10 list was called `top_ten`
and your branch generalizing to any number of movies was called `top_N`.
Predict the behavior of these three possible operations - you don't
have to provide a full `diff` but you should describe at a high level
what changes would happen.

These questions are supposed to be separate paths, not cumulative;
for example, you should *not* assume that the operations of 5 were run
before 6. When testing outcomes later in the lab, you should make sure to
revert back to the state of the branch before you started these questions.

5. What do you think would happen if you ran the following commands?
What branches would change, and how?
```
git checkout test
git merge top_N
```

We switch to the test branch. Then, the top_N branch will merge the changes from top_N to test. As a result, only the test branch will be changed. Assuming there are no changes to the same lines of code in both branches (which would result in a merge conflict), a new merge commit would be created.

6. What do you think would happen if you ran the following commands?
What branches would change, and how?
```
git checkout top_ten
git merge test
```

We switch to the top_ten branch. Then, the test branch will merge the changes from test to top_ten. As a result, only the top_10 branch will be changed. Assuming there are no changes to the same lines of code in both branches (which would result in a merge conflict), a new merge commit would be created.

7. What do you think would happen if you ran the following commands?
What branches would change, and how?
```
git checkout test
git rebase top_ten
git rebase top_N
```

We switch to the test branch. Then, the commits from top_ten will replace the test branch entirely with its commit history. Then, the commits from top_N will replace the test branch with its commit history. As a result, only the test branch will be changed. After running these commands, a merge conflict will occur, prompting the generation of annotations around the segments of the code that differ between the two versions of the file. 

8. Extra Credit


As per the instructions of this assignment, I first did `git checkout -b extra_credit” on the main branch. 

I first did "git archive --format=zip --output project.zip extra_credit" to make a zip file of my entire repo in Git. I did not know this was doable, but it is nice to know, as it saves me time from clicking buttons on MacOS to click on the corresponding directory and then click on a “Compress File” button. It also allows me to make a zip file of any specified branch, and that might be useful (ie: a person has multiple branches and would like to just send one of those to a colleague who does not know about "git switch" or changing between branches in a repo).

Then, I made a change in process_movie_data.py, where the number of rows displayed based on the argument n will be incorrect. Specifically, I changed Line 26 from "top_n = rows[:-n-1:-1]" to "top_n = rows[:-n-5:-1]". Consider this change to result in a bug that was committed. Pretend that this bug was introduced in a large codebase with many commits. Also pretend that no programmer has used "git rebase" in the commit log to make it cleaner. As a result, the programmer notices the bug, but does not know which commit introduced this bug! To simulate this, I committed two (minor) changes. Now, I ran "git bisect good b2ed39d", which was one of Prof. Xanda’s commits where the programmer knew the code was not broken. Then, I ran "git bisect bad 7fc075d" which reflects the head in extra_credit, as I know that it reflects the buggy output. I keep labelling a few more commits as with either a good/bad label, until the command automatically lets me know which commit has the error, using a binary search method to ease the process of bug-finding. To explicitly look at a log of good and bad commits, we can run "git bisect log". Since the amount of commits in this file is relatively small, I had to look and label through most commits to find the error (n commits, needed to label n-1 commits for this repo). This seems relatively useless, but from my understanding, if the commit history was longer with the inclusion of a bug, the binary search would be more apparent, as we would have to look through and label less commits to determine the buggy commit.