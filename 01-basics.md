# Episode 1 Automated Version Control

[![Piled Higher and Deeper by Jorge Cham, http://www.phdcomics.com/comics/archive_print.php?comicid=1531](/fig/phd101212s.png)](http://www.phdcomics.com)

"Piled Higher and Deeper" by Jorge Cham, http://www.phdcomics.com

>We've all been in this situation before: it seems ridiculous to have
multiple nearly-identical versions of the same document. Some word
processors let us deal with this a little better, such as Microsoft
Word's [Track Changes](https://support.office.com/en-us/article/Track-changes-in-Word-197ba630-0f5f-4a8e-9a77-3712475e806a), 
Google Docs' [version history](https://support.google.com/docs/answer/190843?hl=en), or 
LibreOffice's [Recording and Displaying Changes](https://help.libreoffice.org/Common/Recording_and_Displaying_Changes).

>Version control systems start with a base version of the document and
then record changes you make each step of the way. You can
think of it as a recording of your progress: you can rewind to start at the base
document and play back each change you made, eventually arriving at your
more recent version.

### Changes are saved sequentially

![Changes Are Saved Sequentially](/fig/play-changes.svg)

>Once you think of changes as separate from the document itself, you
can then think about "playing back" different sets of changes on the base document, ultimately
resulting in different versions of that document. For example, two users can make independent
sets of changes on the same document. 

### Different people can save different changes

![Different Versions Can be Saved](/fig/versions.svg)

>Unless multiple users make changes to the same section of the document - a conflict - you can 
incorporate two sets of changes into the same base document.

### Multiple Changes to the same section need to be merged

![Multiple Versions Can be Merged](/fig/merge.svg)

>A version control system is a tool that keeps track of these changes for us,
effectively creating different versions of our files. It allows us to decide
which changes will be made to the next version (each record of these changes is called a commit, and keeps useful metadata about them. The complete history of commits for a particular project and their
metadata make up a repository.
Repositories can be kept in sync across different computers, facilitating
collaboration among different people.


 ## Paper Writing

 * Imagine you drafted an excellent paragraph for a paper you are writing, but later ruin it. How would you retrieve the *excellent* version of your conclusion? Is it even possible?

 * Imagine you have 5 co-authors. How would you manage the changes and comments they make to your paper? If you use LibreOffice Writer or Microsoft Word, what happens if you accept changes made using the `Track Changes` option? Do you have a history of those changes?

<details>
<summary> Solution
</summary>

* Recovering the excellent version is only possible if you created a copy
     of the old version of the paper. The danger of losing good versions
    often leads to the problematic workflow illustrated in the PhD Comics
     cartoon at the top of this page.
     
 *   Collaborative writing with traditional word processors is cumbersome.
    Either every collaborator has to work on a document sequentially
    (slowing down the process of writing), or you have to send out a
     version to all collaborators and manually merge their comments into
    your document. The 'track changes' or 'record changes' option can     highlight changes for you and simplifies merging, but as soon as you
    accept changes you will lose their history. You will then no longer     know who suggested that change, why it was suggested, or when it was
    merged into the rest of the document. Even online word processors like
    Google Docs or Microsoft Office Online do not fully resolve these
   problems.

</details>