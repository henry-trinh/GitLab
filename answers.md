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

6. What do you think would happen if you ran the following commands?
What branches would change, and how?
```
git checkout top_ten
git merge test
```

7. What do you think would happen if you ran the following commands?
What branches would change, and how?
```
git checkout test
git rebase top_ten
git rebase top_N
```
