# Ignoring Things

 ## 6.1 Ignoring Nested Files

 Given a directory structure that looks like:

 ~~~
 results/data
 results/plots
 ~~~

 How would you ignore only `results/plots` and not `results/data`?
<details>
<summary>Solution</summary>

If you only want to ignore the contents of `results/plots`, you can change your `.gitignore` to ignore only the `/plots/` subfolder by adding the following line to your .gitignore:
~~~
results/plots/
~~~
This line will ensure only the contents of `results/plots` is ignored, and
not the contents of `results/data`.

As with most programming issues, there are a few alternative ways that one may ensure this ignore rule is followed.
</details>


## 6.2 Including Specific Files

 How would you ignore all `.dat` files in your root directory except for
 `final.dat`?
<details>
<summary>Solution</summary>

 You would add the following two lines to your .gitignore:

 ~~~
 *.dat           # ignore all data files
 !final.dat      # except final.data
 ~~~

 The exclamation point operator will include a previously excluded entry.

 Note also that because you've previously committed `.dat` files in this
 lesson they will not be ignored with this new rule. Only future additions
 of `.dat` files added to the root directory will be ignored.
</details>

 ## 6.3 Ignoring Nested Files: Variation

 Given a directory structure that looks similar to the earlier Nested Files
 exercise, but with a slightly different directory structure:

 ~~~
 results/data
 results/images
 results/plots
 results/analysis
 ~~~

 How would you ignore all of the contents in the results folder, but not `results/data`?

 Hint: think a bit about how you created an exception with the `!` operator
before.

 ## Solution

 If you want to ignore the contents of `results/` but not those of `results/data/`, you can change your `.gitignore` to ignore
 the contents of results folder, but create an exception for the contents of the `results/data` subfolder. Your .gitignore would look like this:

 ~~~
 results/*               # ignore everything in results folder
 !results/data/          # do not ignore results/data/ contents
> > ~~~
> > {: .output}
> >
> {: .solution}
{: .challenge}

> ## Ignoring all data Files in a Directory
>
> Assuming you have an empty .gitignore file, and given a directory structure that looks like:
>
> ~~~
> results/data/position/gps/a.dat
> results/data/position/gps/b.dat
> results/data/position/gps/c.dat
> results/data/position/gps/info.txt
> results/plots
> ~~~
> {: .language-bash}
>
> What's the shortest `.gitignore` rule you could write to ignore all `.dat`
> files in `result/data/position/gps`? Do not ignore the `info.txt`.
>
> > ## Solution
> >
> > Appending `results/data/position/gps/*.dat` will match every file in `results/data/position/gps`
> > that ends with `.dat`.
> > The file `results/data/position/gps/info.txt` will not be ignored.
> {: .solution}
{: .challenge}

> ## The Order of Rules
>
> Given a `.gitignore` file with the following contents:
>
> ~~~
> *.dat
> !*.dat
> ~~~
> {: .language-bash}
>
> What will be the result?
>
> > ## Solution
> >
> > The `!` modifier will negate an entry from a previously defined ignore pattern.
> > Because the `!*.dat` entry negates all of the previous `.dat` files in the `.gitignore`,
> > none of them will be ignored, and all `.dat` files will be tracked.
> >
> {: .solution}
{: .challenge}

> ## Log Files
>
> You wrote a script that creates many intermediate log-files of the form `log_01`, `log_02`, `log_03`, etc.
> You want to keep them but you do not want to track them through `git`.
>
> 1. Write **one** `.gitignore` entry that excludes files of the form `log_01`, `log_02`, etc.
>
> 2. Test your "ignore pattern" by creating some dummy files of the form `log_01`, etc.
>
> 3. You find that the file `log_01` is very important after all, add it to the tracked files without changing the `.gitignore` again.
>
> 4. Discuss with your neighbor what other types of files could reside in your directory that you do not want to track and thus would exclude via `.gitignore`.
>
> > ## Solution
> >
> > 1. append either `log_*`  or  `log*`  as a new entry in your .gitignore
> > 3. track `log_01` using   `git add -f log_01`
> {: .solution}
{: .challenge}
