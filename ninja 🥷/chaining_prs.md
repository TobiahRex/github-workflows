# Chaining PR's
When PR's haven't been merged yet, perhaps due to peer-review taking some time, but we want to add more features or branches, we typically "chain" the PR's by building on top of the features.

## Managing Chained-PR's
```
    master => feature_1 => feature_2 => feature_3
```
#### _Problem:_
Coordinating the chaining of PR's can be tricky sometimes. If you have many team members all working on a large feature but doing individual work, this can be quite common. It's also common if you don't want to be bottle-necked by PR reviews, so you decide to waterfall your branches in a way that everything builds on top of itself. How do you keep track in an organized systematic way?
#### _Solutions:_
You have to ensure you stay organized as a team and have a systematic way to determine which PR's go in which order.
1. Use the Git Dashboard! Simple and easy. From the PR dashboard merge in the following order
    ```
      4          3             2            1
    master <= feature_1 <= feature_2 <= feature_3
    ```
    1. `feature_3` merges into `feature_2`. `feature_2` now includes history from both branches.
    2. `feature_2` merges into `feature_1`. `feature_1` now includes history from all three branches.
    3. `feature_1` merges into `master`. Now master has sequential history of all branches.
    4. If **merge conflicts** arise, then simply resolve them as you go.
2. Use the same workflow as #1 but do so in the CLI rather than the Ui Dashboard. Why might this be better? In case of **merge conflicts** you'll have to open the IDE and manually fix conflicts anyways. So choosing this option is perhaps more efficient, albeit not as visually appealing.
1. Bail-out example
    - ```
        $ gfa
        $ gco feature_2
        $ gmom feature_3
        ...resolve conflicts...

        $ gma
        ...bail out, reset, start over....
        ```
2. Happy example
    - ```
        $ gfa
        $ gco feature_2
        $ gmom feature_3
        ...resolve conflicts...

        $ gco feature_1
        $ gmom feature_2
        ...resolve conflicts...

        $ gco -b features_1_2_3
        ...create new composed branch...

        $ ggpush
        ...push all features to origin as 1 branch...
      ```

## Fix Out-of-Date Chained PR's
#### _Problem_:
1. Branch `my_branch_A` is under Peer Review, and probably won't be finished in the immediate future. While we wait for our peers to review, we want to keep adding/building/working on the next set of features.
2. We build another branch starting at the last commit of `my_branch_A`, and we call this new branch `my_branch_B`. The mental model would be diagram would be something like...
    - ```
        # my_branch_A Branch status in PR being reviewed

                        hash: xyz     hash: uvw     hash: tuv
        master ... ---- commit-A ---- commit-B ---- HEAD




        # my_branch_B is building "on top" of my_branch_A

        master ... ---- commit-A ---- commit-B ---- HEAD
                                                        \---- commit-C ---- commit-D

                                                                    my_branch_B
      ```
3. We finish building `my_branch_B` stuff. We want to now "extend" `my_branch_A` with the stuff we just built in `...B`. But during PR review, our peers asked us to make some small adjustments to our code on `my_branch_A` and so the branch history has changed. `my_branch_B` doesn't have the latest history.
```
# my_branch_A: NEW

                hash: xyz     hash: ghi     hash: tuv
master ... ---- commit-A ---- commit-B ---- HEAD



# my_branch_B is out of date
                hash: xyz     hash: uvw     hash: tuv
master ... ---- commit-A ---- commit-B ---- HEAD
                                                \---- commit-C ---- commit-D

                                                          my_branch_B
```

4. `commit-B`'s hash value changed from `ghi` to `uvw` so `...B` is out of date.
#### _Solution_:
1. `gfa`
    - pull down all remote branches into local addresses
2. `gco my_branch_A`
    - checkout A
3. `grb origin/my_branch_A`
    - sync local A with origin A
4. `gco my_branch_B`
    - checkout B
5. `grb origin/my_branch-A`
    - sync local B with local/updated A
    - This step sync's the history. So the hash of `commit-B` will be updated with the value in `origin`
