# Delete

## Delete Branch | `Easy`
1. Delete local branch
    - `gb -D my_branch`
2. Delete remote branch
    - `git push origin --delete my_branch`
    - <img src="https://imgur.com/P3iHIPl.png" style="max-width: 500px" />
    - `https://github.com/TobiahRex/algorist-toolbox/branches`

## Delete Commit | `Medium/Hard`
1. Latest Commit = Easy
    - `git reset HEAD^`
        - Move all changes in the last commit (auto tagged as `HEAD`) to the _unstaged_ area.
    - `git stash`
        - Move all the changes out of the staging area, but somewhere they can be retrieved later (if i want), otherwise they'll be garbage collected.
2. Historical Commit = `Medium/Hard`
    - `git rebase -i HEAD~4`
        - Move the last 4 commits into a rebase area. I want to modify, remove, delete, squash, manipulate the commit history. See the [DOCS here](https://git-scm.com/docs/git-rebase) for more details. It's *super powerful* tool and very useful.