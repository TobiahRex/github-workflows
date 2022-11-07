# Sync Branches

#### _Problem_:
Sometimes we want to push changes to a branch, but someone else is working on the same branch and they added some new history that we didn't know about. We must now sync before we can also add to the history.
1. `ggpush` doesn't work. Errors says we're **"...behind"**

#### _Solution_:
1. `gfa`
    - sync your repo with the origin because the origin changed since we last talked to the origin.
    - Mental Model:
        - ```
            You want to talk to Bob at company-Z, but bob no longer work there.
            So you need to re-sync your phonebook and talk to Bob's replacement.
          ```
3. `grb origin/my_branch`
    - append my changes to the end of history
4. `ggpush`
    - Viola! Changes pushed
5. CLI workflow all-together...
    - ```
        $ ggpush
        ...error... 😡

        $ gfa
        ...syncing...

        $ grb origin/my_branch
        ...re-writing history...

        $ ggpush
        ...pushing changes to remote...
        ```