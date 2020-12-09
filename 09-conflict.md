# Conflicts
 ## 9.1 Conflicts on Non-textual files

 What does Git do
 when there is a conflict in an image or some other non-textual file
 that is stored in version control?

<details>
<summary>Solution</summary>

 Let's try it. Suppose Dracula takes a picture of Martian surface and
 calls it `mars.jpg`.

 If you do not have an image file of Mars available, you can create
 a dummy binary file like this:

input:
 ~~~
 $ head -c 1024 /dev/urandom > mars.jpg
 $ ls -lh mars.jpg
 ~~~

output:
 ~~~
 -rw-r--r-- 1 vlad 57095 1.0K Mar  8 20:24 mars.jpg
 ~~~


 `ls` shows us that this created a 1-kilobyte file. It is full of random bytes read from the special file, `/dev/urandom`.

 Now, suppose Dracula adds `mars.jpg` to his repository:
  input:
 ~~~
 $ git add mars.jpg
 $ git commit -m "Add picture of Martian surface"
 ~~~

output:
 ~~~
 [master 8e4115c] Add picture of Martian surface
  1 file changed, 0 insertions(+), 0 deletions(-)
  create mode 100644 mars.jpg
 ~~~


 Suppose that Wolfman has added a similar picture in the meantime.
 His is a picture of the Martian sky, but it is *also* called `mars.jpg`.
 When Dracula tries to push, he gets a familiar message:

input:
 ~~~
 $ git push origin master
 ~~~

output:
 ~~~
 To https://github.com/vlad/planets.git
  ! [rejected]        master -> master (fetch first)
 error: failed to push some refs to 'https://github.com/vlad/planets.git'
 hint: Updates were rejected because the remote contains work that you do
 hint: not have locally. This is usually caused by another repository pushing
 hint: to the same ref. You may want to first integrate the remote changes
 hint: (e.g., 'git pull ...') before pushing again.
 hint: See the 'Note about fast-forwards' in 'git push --help' for details.
 ~~~

 We've learned that we must pull first and resolve any conflicts:

input:
 ~~~
 $ git pull origin master
 ~~~

 When there is a conflict on an image or other binary file, git prints
 a message like this:

output:
 ~~~
 $ git pull origin master
 remote: Counting objects: 3, done.
 remote: Compressing objects: 100% (3/3), done.
 remote: Total 3 (delta 0), reused 0 (delta 0)
 Unpacking objects: 100% (3/3), done.
 From https://github.com/vlad/planets.git
  * branch            master     -> FETCH_HEAD
    6a67967..439dc8c  master     -> origin/master
 warning: Cannot merge binary files: mars.jpg (HEAD vs. 439dc8c08869c342438f6dc4a2b615b05b93c76e)
 Auto-merging mars.jpg
 CONFLICT (add/add): Merge conflict in mars.jpg
 Automatic merge failed; fix conflicts and then commit the result.
 ~~~


 The conflict message here is mostly the same as it was for `mars.txt`, but
 there is one key additional line:

 ~~~
 warning: Cannot merge binary files: mars.jpg (HEAD vs. 439dc8c08869c342438f6dc4a2b615b05b93c76e)
 ~~~

 Git cannot automatically insert conflict markers into an image as it does
 for text files. So, instead of editing the image file, we must check out
 the version we want to keep. Then we can add and commit this version.

 On the key line above, Git has conveniently given us commit identifiers
 for the two versions of `mars.jpg`. Our version is `HEAD`, and Wolfman's
 version is `439dc8c0...`. If we want to use our version, we can use
 `git checkout`:

input:
~~~
 $ git checkout HEAD mars.jpg
 $ git add mars.jpg
 $ git commit -m "Use image of surface instead of sky"
 ~~~

output:
 ~~~
 [master 21032c3] Use image of surface instead of sky
 ~~~


 If instead we want to use Wolfman's version, we can use `git checkout` with
 Wolfman's commit identifier, `439dc8c0`:

input:
 ~~~
 $ git checkout 439dc8c0 mars.jpg
 $ git add mars.jpg
 $ git commit -m "Use image of sky instead of surface"
 ~~~

output:
 ~~~
[master da21b34] Use image of sky instead of surface
 ~~~


 We can also keep *both* images. The catch is that we cannot keep them
 under the same name. But, we can check out each version in succession
 and *rename* it, then add the renamed versions. First, check out each
 image and rename it:

 ~~~
 $ git checkout HEAD mars.jpg
 $ git mv mars.jpg mars-surface.jpg
 $ git checkout 439dc8c0 mars.jpg
 $ mv mars.jpg mars-sky.jpg
 ~~~
 {: .language-bash}

 Then, remove the old `mars.jpg` and add the two new files:

input:
 ~~~
 $ git rm mars.jpg
 $ git add mars-surface.jpg
 $ git add mars-sky.jpg
 $ git commit -m "Use two images: surface and sky"
 ~~~

output:
 ~~~
 [master 94ae08c] Use two images: surface and sky
  2 files changed, 0 insertions(+), 0 deletions(-)
  create mode 100644 mars-sky.jpg
  rename mars.jpg => mars-surface.jpg (100%)
 ~~~


 Now both images of Mars are checked into the repository, and `mars.jpg`
 no longer exists.

</details>

> ## 9.2 A Typical Work Session
>
> You sit down at your computer to work on a shared project that is tracked in a
> remote Git repository. During your work session, you take the following
> actions, but not in this order:
>
> - *Make changes* by appending the number `100` to a text file `numbers.txt`
> - *Update remote* repository to match the local repository
> - *Celebrate* your success with some fancy beverage(s)
> - *Update local* repository to match the remote repository
> - *Stage changes* to be committed
> - *Commit changes* to the local repository
>
> In what order should you perform these actions to minimize the chances of
> conflicts? Put the commands above in order in the *action* column of the table
> below. When you have the order right, see if you can write the corresponding
> commands in the *command* column. A few steps are populated to get you
> started.
>
> |order|action . . . . . . . . . . |command . . . . . . . . . . |
> |-----|---------------------------|----------------------------|
> |1    |                           |                            |
> |2    |                           | `echo 100 >> numbers.txt`  |
> |3    |                           |                            |
> |4    |                           |                            |
> |5    |                           |                            |
> |6    | Celebrate!                | `AFK`                      |
>
<details>
<summary>Solution</summary>
> >
> > |order|action . . . . . . |command . . . . . . . . . . . . . . . . . . . |
> > |-----|-------------------|----------------------------------------------|
> > |1    | Update local      | `git pull origin master`                     |
> > |2    | Make changes      | `echo 100 >> numbers.txt`                    |
> > |3    | Stage changes     | `git add numbers.txt`                        |
> > |4    | Commit changes    | `git commit -m "Add 100 to numbers.txt"`     |
> > |5    | Update remote     | `git push origin master`                     |
> > |6    | Celebrate!        | `AFK`                                        |
> >
</details>
