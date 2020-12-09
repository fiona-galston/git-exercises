# Exploring History

## Don't Lose Your HEAD


The "detached HEAD" is like "look, but don't touch" here, so you shouldn't make any changes in this state.
After investigating your repo's past state, reattach your `HEAD` with `git checkout master`.

It's important to remember that
we must use the commit number that identifies the state of the repository
*before* the change we're trying to undo.
A common mistake is to use the number of
the commit in which we made the change we're trying to discard.
In the example below, we want to retrieve the state from before the most
recent commit (`HEAD~1`), which is commit `f22b25e`:

![Git Checkout](/fig/git-checkout.svg)

So, to put it all together,
here's how Git works in cartoon form:

![https://figshare.com/articles/How_Git_works_a_cartoon/1328266](/fig/git_staging.svg)


## 5.1 Recovering Older Versions of a File

Jennifer has made changes to the Python script that she has been working on for weeks, and the modifications she made this morning "broke" the script and it no longer runs. She has spent ~ 1hr trying to fix it, with no luck...

Luckily, she has been keeping track of her project's versions using Git! Which commands below will let her recover the last committed version of her Python script called `data_cruncher.py`?
~~~
1. `$ git checkout HEAD`

2. `$ git checkout HEAD data_cruncher.py`

3. `$ git checkout HEAD~1 data_cruncher.py`

4. `$ git checkout <unique ID of last commit> data_cruncher.py`

5. Both 2 and 4
~~~

<details>
<summary>Solution</summary>

The answer is (5)-Both 2 and 4. 

The `checkout` command restores files from the repository, overwriting the files in your working directory. Answers 2 and 4 both restore the *latest* version *in the repository* of the file `data_cruncher.py`. Answer 2 uses `HEAD` to indicate the *latest*, whereas answer 4 uses the unique ID of the last commit, which is what `HEAD` means. 

Answer 3 gets the version of `data_cruncher.py` from the commit *before* `HEAD`, which is NOT what we wanted.

Answer 1 can be dangerous! Without a filename, `git checkout` will restore **all files** in the current directory (and all directories below it) to their state at the commit specified. This command will restore `data_cruncher.py` to the latest commit version, but it will also 
 restore *any other files that are changed* to that version, erasing any changes you may have made to those files! As discussed above, you are left in a *detached* `HEAD` state, and you don't want to be there.
</details>

 ## 5.2 Reverting a Commit

Jennifer is collaborating on her Python script with her colleagues and
realizes her last commit to the project's repository contained an error and
she wants to undo it.  `git revert [erroneous commit ID]` will create a new commit that reverses Jennifer's erroneous commit. Therefore `git revert` is different to `git checkout [commit ID]` because `git checkout` returns the files within the local repository to a previous state, whereas `git revert` reverses changes committed to the local and project repositories. 

Below are the right steps and explanations for Jennifer to use `git revert`, what is the missing command?
~~~
1. `________ # Look at the git history of the project to find the commit ID`

2. Copy the ID (the first few characters of the ID, e.g. 0b1d055).

3. `git revert [commit ID]`

4. Type in the new commit message.

5. Save and close
~~~

 ## 5.3 Understanding Workflow and History

 What is the output of the last command in

 ~~~
 $ cd planets
 $ echo "Venus is beautiful and full of love" > venus.txt
 $ git add venus.txt
 $ echo "Venus is too hot to be suitable as a base" >> venus.txt
 $ git commit -m "Comment on Venus as an unsuitable base"
 $ git checkout HEAD venus.txt
 $ cat venus.txt #this will print the contents of venus.txt to the screen
 ~~~

 1. ~~~
    Venus is too hot to be suitable as a base
    ~~~
 2. ~~~
    Venus is beautiful and full of love
    ~~~
 3. ~~~
    Venus is beautiful and full of love
    Venus is too hot to be suitable as a base
    ~~~
 4. ~~~
    Error because you have changed venus.txt without committing the changes
    ~~~

<details>
<summary>Solution</summary>

 The answer is 2. 
 
 The command `git add venus.txt` places the current version of `venus.txt` into the staging area. 
 The changes to the file from the second `echo` command are only applied to the working copy, not the version in the staging area.
 So, when `git commit -m "Comment on Venus as an unsuitable base"` is executed, the version of `venus.txt` committed to the repository is the one from the staging area and has only one line.
  
  At this time, the working copy still has the second line (and 
  `git status` will show that the file is modified). However, `git checkout HEAD venus.txt` 
  replaces the working copy with the most recently committed version of `venus.txt`.
 
 So, `cat venus.txt` will output 
  ~~~
  Venus is beautiful and full of love.
 ~~~
</details>

 ## 5.4 Checking Understanding of `git diff`

 Consider this command:
 ~~~
  git diff HEAD~9 mars.txt. 
  ~~~
What do you predict this command will do if you execute it? What happens when you do execute it? Why?

 Try another command, 
 ~~~
 git diff [ID] mars.txt
 ~~~
 where [ID] is replaced with
 the unique identifier for your most recent commit. What do you think will happen,
 and what does happen?

 ## 5.5 Getting Rid of Staged Changes

 `git checkout` can be used to restore a previous commit when unstaged changes have
 been made, but will it also work for changes that have been staged but not committed?
 Make a change to `mars.txt`, add that change, and use `git checkout` to see if you can remove your change.

 ## 5.6 Explore and Summarize Histories

 Exploring history is an important part of Git, and often it is a challenge to find the right commit ID, especially if the commit is from several months ago.

 Imagine the `planets` project has more than 50 files.
 You would like to find a commit that modifies some specific text in `mars.txt`.
 When you type `git log`, a very long list appeared.
 How can you narrow down the search?

 Recall that the `git diff` command allows us to explore one specific file,
 e.g., `git diff mars.txt`. We can apply a similar idea here.

 ~~~
 $ git log mars.txt
 ~~~

 Unfortunately some of these commit messages are very ambiguous, e.g., `update files`.
 How can you search through these files?

 Both `git diff` and `git log` are very useful and they summarize a different part of the history for you.
 Is it possible to combine both? Let's try the following:

 ~~~
 $ git log --patch mars.txt
 ~~~

 You should get a long list of output, and you should be able to see both commit messages and 
 the difference between each commit.

 Question: What does the following command do?

 ~~~
 $ git log --patch HEAD~9 *.txt 
 ~~~

