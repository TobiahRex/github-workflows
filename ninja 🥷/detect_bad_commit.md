# Find Bad Commit

#### _Problem:_
You've been building up a project over time. You've added a bunch of commits, merged a bunch of code, lots of PR's, lots of commits, but you don't have 100% test coverage, or even if you do, somehow you miss something, and notice there's a BUG in deployed code.  Uh-oh. What's the fastest way to fix the BUG? The answer is not always this simple, but the quick emergency fix, is to revert the changes that included the BUG. But how do we know which PR's/Commits to revert? Answer: `git-bisect`
#### _Solution:_
[**Git-Bisect**](https://git-scm.com/docs/git-bisect/):
Uses binary-search under-the-hood to build a tree of your commit history going back to the seed-commit. You are implicitly navigating the tree by using the `git-bisect` API to isolate the commit that has the BUG. Let's demonstrate...
1. `gco my_BUG_branch`
    - Checkout target branch
2. `gbss`
    - initialize the git-bisect workflow start
3. ... git will automatically select the commit for you using it's internal binary-search algorithm, we just tell git whether we do/don't see the bug anymore for that commit ...
4. `gbsg`
    - Mark this commit as a good commit: No Bugs.
5. `gbsb`
    - Mark this commit as a bad commit: The BUG is HERE! NOTE: marking a commit as "Bad" doesn't mean you're saying this is the commit that created the BUG, rather, you're saying this commit is infected with the BUG.  We still may have other commits to inspect before we can know for sure if this commit is the source of the BUG.
6. We continue this process for some time, then eventuall git tells us the commit is `xyz`.
7. `gbsr`
    - **Optional**: reset/start over

**NOTE**
Step **3** can be evaluated as "Good" by checking out that commit, spinning up the app, and testing the feature you know to be broken. It may also sometimes be that this commit doesn't even have that feature built out, in which case you can also say it's "good" by saying "it doesn't exist as the feature is now gone!" A nuclear option for sure, but it at least removes the bug. It's up to you and your team to decide if this is an acceptible option.
