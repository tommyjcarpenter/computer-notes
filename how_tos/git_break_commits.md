# Breaking up git commits

To split apart your most recent commit, first:

    git reset HEAD~

Now commit the pieces individually in the usual way, producing as many commits as you need.

If it was farther back in the tree, then

    git rebase -i HEAD~X

where `X` is how many commits back it is.

When you get the rebase edit screen:
1. find the commit you want to break apart.
2. Replace "pick" with "edit".
3. Save the buffer and exit.
4. Rebase will now stop just after the commit you want to edit.

Then:

    git reset HEAD~

Commit the pieces individually in the usual way, producing as many commits as you need,

    git add foo
    git commit -m "..."
    git add othersnafoo
    git commit -m "..."

Then when all the files have been added into commits,

    git rebase --continue

Helpful link: http://stackoverflow.com/questions/6217156/break-a-previous-commit-into-multiple-commits
