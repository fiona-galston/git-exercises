# Remotes in GitHub
---

Version control really comes into its own when we begin to collaborate with
other people.  We already have most of the machinery we need to do this; the
only thing missing is to copy changes from one repository to another.

Systems like Git allow us to move work between any two repositories.  In
practice, though, it's easiest to use one copy as a central hub, and to keep it
on the web rather than on someone's laptop.  Most programmers use hosting
services like [GitHub](https://github.com), [Bitbucket](https://bitbucket.org) or
[GitLab](https://gitlab.com/) to hold those master copies; we'll explore the pros
and cons of this in the final section of this lesson.

Let's start by sharing the changes we've made to our current project with the
world.  Log in to GitHub, then click on the icon in the top right corner to
create a new repository called `planets`:

![Creating a Repository on GitHub (Step 1)](/fig/github-create-repo-01.png)

Name your repository "planets" and then click "Create Repository".

Note: Since this repository will be connected to a local repository, it needs to be empty. Leave 
"Initialize this repository with a README" unchecked, and keep "None" as options for both "Add 
.gitignore" and "Add a license." See the "GitHub License and README files" exercise below for a full 
explanation of why the repository needs to be empty.

![Creating a Repository on GitHub (Step 2)](/fig/github-create-repo-02.png)

As soon as the repository is created, GitHub displays a page with a URL and some
information on how to configure your local repository:

![Creating a Repository on GitHub (Step 3)](/fig/github-create-repo-03.png)

This effectively does the following on GitHub's servers:

~~~
$ mkdir planets
$ cd planets
$ git init
~~~

### The Local Repository with Git Staging Area
![The Local Repository with Git Staging Area](/fig/git-staging-area.svg)

### Local & Fresh Remote

![Freshly-Made GitHub Repository](/fig/git-freshly-made-github-repo.svg)

The next step is to connect the two repositories. We do this by making the GitHub repository a remote for the local repository. The home page of the repository on GitHub includes the string we need to identify it:

![Where to Find Repository URL on GitHub](/fig/github-find-repo-string.png)

### After Remote Connection

Our local and remote repositories are now in this state:

![GitHub Repository After First Push](/fig/github-repo-after-first-push.svg)

> ## 7.1 GitHub GUI
>
> Browse to your `planets` repository on GitHub.
> Under the Code tab, find and click on the text that says "XX commits" (where "XX" is some number).
> Hover over, and click on, the three buttons to the right of each commit.
> What information can you gather/explore from these buttons?
> How would you get that same information in the shell?
>
> <details>
> <summary>Solution</summary>
> The left-most button (with the picture of a clipboard) copies the full identifier of the commit 
> to the clipboard. In the shell, ```git log``` will show you the full commit identifier for each 
> commit.
>
> When you click on the middle button, you'll see all of the changes that were made in that 
> particular commit. Green shaded lines indicate additions and red ones removals. In the shell we 
> can do the same thing with ```git diff```. In particular, ```git diff ID1..ID2``` where ID1 and 
> ID2 are commit identifiers (e.g. ```git diff a3bf1e5..041e637```) will show the differences 
> between those two commits.
>
> The right-most button lets you view all of the files in the repository at the time of that 
> commit. To do this in the shell, we'd need to checkout the repository at that particular time. 
> We can do this with ```git checkout ID``` where ID is the identifier of the commit we want to 
> look at. If we do this, we need to remember to put the repository back to the right state 
> afterwards!
</details>

> ## GitHub Timestamp
>
> Create a remote repository on GitHub. Push the contents of your local
> repository to the remote. Make changes to your local repository and push these
> changes. Go to the repo you just created on GitHub and check the
> [timestamps]({{ page.root }}{% link reference.md %}#timestamp) of the files. How does GitHub
> record times, and why?
>
><details>
> <summary>Solution</summary>
>  GitHub displays timestamps in a human readable relative format (i.e. "22 hours ago" or "three 
>  weeks ago"). However, if you hover over the timestamp, you can see the exact time at which the 
> last change to the file occurred.
</details>

> ## Push vs. Commit
>
> In this lesson, we introduced the "git push" command.
> How is "git push" different from "git commit"?
><details>
> <summary>Solution</summary>
> When we push changes, we're interacting with a remote repository to update it with the changes 
> we've made locally (often this corresponds to sharing the changes we've made with others). 
> Commit only updates your local repository.
</details>

> ## GitHub License and README files
>
> In this section we learned about creating a remote repository on GitHub, but when you initialized 
> your GitHub repo, you didn't add a README.md or a license file. If you had, what do you think 
> would have happened when you tried to link your local and remote repositories?
><details>
> <summary>Solution</summary>
> In this case, we'd see a merge conflict due to unrelated histories. When GitHub creates a README.md file, it performs a commit in the remote repository. When you try to pull the remote repository to your local repository, Git detects that they have histories that do not share a common origin and refuses to merge.
>
> ~~~
> $ git pull origin master
> ~~~
>
> ~~~
> warning: no common commits
> remote: Enumerating objects: 3, done.
> remote: Counting objects: 100% (3/3), done.
> remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
> Unpacking objects: 100% (3/3), done.
> From https://github.com/vlad/planets
>  * branch            master     -> FETCH_HEAD
>  * [new branch]      master     -> origin/master
> fatal: refusing to merge unrelated histories
> ~~~
>
> You can force git to merge the two repositories with the option `--allow-unrelated-histories`. 
> Be careful when you use this option and carefully examine the contents of local and remote 
> repositories before merging.
> ~~~
> $ git pull --allow-unrelated-histories origin master
> ~~~
>
> ~~~
> From https://github.com/vlad/planets
>  * branch            master     -> FETCH_HEAD
> Merge made by the 'recursive' strategy.
> README.md | 1 +
> 1 file changed, 1 insertion(+)
> create mode 100644 README.md
> ~~~
</details>
