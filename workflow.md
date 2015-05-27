Dev Branch Workflow
====================
Why not call them feature branches? Well each branch will usually be tied to a single issue or pull request. Sometimes they are features, but sometimes they are bugs, tasks, etc. They all involve some code changes, so simply call them dev branches for development branches. [also dev is slightly faster to type than feature]

`master` branch is the default upstream branch
----------------------------------------------
`master` is the bleeding-edge of the latest development. It's what almost all dev branches should start/branch from and what all pull requests are against. Some people make master the "production" branch, like with "github flow" and github **does** allow you to name another branch as the default branch, but to do so flys in the face of what most projects do so you are asking for trouble to do otherwise. If you really want to have something like `1.x` or `dev` be your main branch, then PLEASE delete the master branch and set the correct default branch in your tooling (in github it's on the repo settings page.) For this document, we'll assume master is the branch you'l' be merging all your requests to.

There is only 1 true `master`, all others must perish
-----------------------------
When you clone a repo, you usually start with a branch called `master`, which is a copy of the `master` on the upstream repo as well. In this workflow you will not be commiting directly to master, so you will almost never need it. In fact, we recommend that you actually delete it from your local repo. git doesn't let you delete a branch that you are on, so let's first rename it.

`git branch -m master local-master; git branch`

Now there's still a master branch, but it's clear that it's the local version and not the "one true master". The reason we do this is that it forces use to use the remote `origin/master` branch most places, which is almost always the better way

Creating branches based on Issue Number and short description
-----------------------------------
Issues / Tickets almost always have some identifying number, so we use that to identify the branch as such when we create it. We'll use [issue_num] as a placeholder for that number. We also always create a branch from the freshest `master` we can to avoid conflicts. So, we've got our ticket and are ready to get to work, this is the command we use to start working.

`git fetch; git checkout -b dev-[issue_num]-very-short-description origin/master`

This command will:
* Grab the latest metadata of the upstream code with `git fetch`
* Create a new branch based on origin/master by using the `-b` flag
* Track origin/master, making pulls rebase instead of merge and a warning if our branch gets behind origin/master
* Immediately checkout our branch instead of a 2 step branch, then checkout process.

DO:
* Always fetch before creating a new branch, as less conficts.
* Always branch from origin/master and not just master (you deleted master right?)
* Use a 1-4 word description after your issue number to give a hint about what this branch does.
* Use dashes and not underscores in your branch name
* Start your branch with `dev-`. Consistent naming across branches makes them easy to find in a list and find the related issue.

DONT:
* Branch from local-master. It's probably out of date or worse is corrupted. (you deleted/renamed master right?)

As you get dozens or hundreds of these dev branches floating around, you'll be glad you did this.

Commiting your work
===================

Create atomic commits
---------------------

There is a happy middle ground between a commit for every line of a file that changes and a giant commit with hundreds of files that change. When deciding how to chunk up your work, consider that someone will be reviewing your code, so use the "golden rule" and first think about how YOU would want to review the code. Best practices of commits are:
* Is the change 'atomic'? Meaning is it just big enough to capture the essence of a change? Could this one commit be applied or reverted?* Add a separate commit for each external library or new module.
* Are there more than 30 files changing at once? If so, it may be hard to review on github's UI. Look at breaking it down into more sensible pieces.
* Did you add a whole commit just to fix a typo? Use --fixup and --autosquash to merge small fixes before pushing code.
* Did you mention the issue number in the commit message? (see format below)
* Does the commit message provide enough detail for the reviewer to understand what changed and more importantly WHY it changed?
* Does the commit message provide enough context that someone looking at it in 2 years will understand it?
* Use url links to public issues, or blog posts if you leveraged that in your code.
* Beware internal urls that might disappear when changing tools.

Adding work to the stage before you commit
--------------------------------------------

Use git add flags to be more careful of what you commit.

Review changes before adding them to stage.
`git add -p [some path]`

This command allows you to review diffs of chunks of code individually before you add it. For instance you can add certain lines but not others. It also allows you to SEE what you are adding and make sure you intended to actually add it. This is also invaluable for creating "atomic" commits but splitting changes up from one file into different commits.

Most common responses are:
* y - yes add this whole chunk to the stage
* n - skip this chunk, don't add it to the stage yet
* s - split this chunk into smaller chunks (if possible)
* e - actually edit this chunks diff to get very fine grained about what changes will be added.

Anything you don't add to the stage doesn't disapper thankfully. The left over change will still be there so you can add them to the stage separately after you're done with your commit.

`git add -u`

The `-u` flag will add all the files that have been updated, but not any new files. Useful for many files at once. Try to use git add -p most of the time though.

Pull things OFF the stage with `git reset [file]`

You can remove files from the stage if you added them by accident. The changes will still be there, and you can re-add them or a subset of them if you like. Unless you want to unset the whole stage, make sure you specify the file or folder you want to remove from the stage. Everyone gets confused by the name called reset.. you can consider this a "soft" reset, unlike it's hardcore brother `git reset --hard` which wipes out any changes that aren't saved to a commit.
