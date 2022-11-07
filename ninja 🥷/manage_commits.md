# Managing Commits
There's a large miss-understanding at the early stages of career's about how to properly manage/use commits. Most people think of commits as one-liner sentencces that sorta, kinda, maybe are something useful i'll look at in the future, but likely not. Instead, commit's should be thought of as significant as a person's **birthday** 🎂 it's totem, moment in time, where you reflect and inventory the growth that's in the recent past. You can describe what challenges were faced, what challenges remain, what doubts are present, what future work should be done to increase the quality of the code. Etc. There's literally no limit to how useful a commit can be for you/your team. It's simply up to you to make the commit truly valuble.

## Make a Commit
1. `gc`
```
{
  "commit message": {
    "scope": "javascript, typescript, text, python",
    "prefix": "gc",
    "body": [
      "${1:<Ticket #> | <Desc> }",
      "${2:}",
      "${3:-------------- }",
      "${4:  WARNING: This commit is dirty }",
      "${5:  }",
      "${6:-------------- }",
      "${7:  MAIN }",
      "${8:  * }",
      "${9:-------------- }",
      "${10:  NOTES }",
      "${11:  * }",
      "${12:-------------- }",
    ]
    }
}
```
```text
    <Ticket #> | <Desc>

    --------------
      WARNING: This commit is dirty

    --------------
      MAIN
      *
    --------------
      NOTES
      *
    --------------
```
<img src="https://imgur.com/EmUeNJa.png" style="max-width:500px" />
<br>
<img src="https://imgur.com/e3ro0VJ.png" style="max-width:500px" />
<br>
<img src="https://imgur.com/1nTNVpr.png" style="max-width:800px" />

## Ammend a commit msg.
* `gc!`
  - Make changes, fix-typos, add changes to your commit messages.

## Reset a commit
* `git reset HEAD^`
  - ```
      o---o---o---HEAD

      o---o---HEAD .x.
    ```
  - The code isn't gone/deleted, rather everything that was at the "HEAD" has no moved to the "unstaged" area. You will need to add a new commit to append the changes to the current branch.

## Update a commit
1. **Option 1**
  1. `git reset HEAD^` | `Easy`
    - Reset the commit moving all file changes in the latest commit back to the staging area.
    - You will LOSE the commit message, so perhaps save it/copy it before choosing this option.
  2. `git add <file>`
  3. `gc`
    - Write your new/updated commit msg.
  4. Push changes
    1. `ggpush` will push a branch to origin if it doesn't already exist.
    2. `ggfl` force push the changes to remote if you already pushed the branch to the origin in the past.
2. **Option 2** | `Hard`
  1. Add some new file change.
  2. Create a one-liner commit.
    - `git add .`
    - `git commit -m updating...`
  3. Rebase the last two commits & squash them together 🤯
    - `git rebase -i HEAD~2`
      * Note `-i` flag denotes "show interactive menu"
    - Select the `squash` (`s`) option for the latest commit. This will squash that commit into the previous commit. You'll also have to choose the commit message you want. This is where you should write a meaningful commit message and explain your changes.
  4. `ggpush` if not already at origin, else `ggfl` to force push your changes to overwrite the origin commit history.

## Remove old commits but keep new commits
5. `git cherry-pick <hash code>`
  - Cherry-picking is super useful when we want to take a few commits from a working branch, and put them into a polished branch.
  - ```
    working branch

    o----x----y----z----HEAD
    ^    ^               ^   = keep


    o----x----HEAD

    new branch
    ```
6. Control flow
  1. `gco o -b my_new_branch`
  2. `git cherry-pick x`
  3. `ggpush`gg
