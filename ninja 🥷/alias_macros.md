# Alias Macros
Abstract git command combinations into bash functions to streamline multiple git steps.

## Clone, Install, Start
```bash
function gtclone { ## git clone $1 and rename to $1 (rename optional)
  if [ "$2" ]
  then
    git clone "$1" "$2"
    cd "$2"
    lsa
    ahere
    yarn
  else
    git clone "$1"
    lsa
    yarn
  fi
}
```
The above function clones a repo with the github origin address as the first argument `"$1"` and puts it in a directory called `"$2"`. If no directory exists, then a directory matching the origin's name is created. After the clone is finished downloading, we navigate into the directory and install any dependencies using `yarn`. The function assumes JS/npm based application, but can be easily modified for something like `pip` etc. Finally one could easily extend the function to start the application.

## Create Repo, Push Code, Open in Browser
```bash
function gtmake {
  git init
  repo_name=$1
  dir_name=`basename $(pwd)`

  if [ "$repo_name" = "" ]; then
    echo "Repo will be '$dir_name'"
  fi

  if [ "$repo_name" = "" ]; then
    repo_name=$dir_name
  fi

  echo -n "Creating Github repository '$repo_name' ..."
  curl -u "<username>:<access token>" https://api.github.com/user/repos -d '{"name":"'$repo_name'"}' > /dev/null 2>&1
  echo " done."

  echo -n "Pushing local code to remote ..."
  git remote add origin https://github.com/<username>/$repo_name.git
  git add .
  sleep 2
  git commit -m 'initial commit'
  sleep 2
  git push -u origin master
  sleep 2
  open https://github.com/<username>/$repo_name.git
  echo " done."
}
```
1. create repo `$1`
2. add every file the a single commit `.`
3. Create a seed commit as `'initial commit'`
4. Create a repo from git cli
5. Assign the origin address
6. Push the commit to the remote origin address.
7. There maybe issues regarding access tokens - if so i recommend trying something a bit more complicated, but functional [here](https://github.com/Joe-a-d/CLI/blob/master/git-new/git-new)

## Add & Remove Tags
1. Add Tag:
    - `git tag -a my-label`
    - [Docs Here](https://git-scm.com/docs/git-tag)
2. Remove Tag locally and from remote branch.
    - ```bash
        function killtag {
            git tag -d "$1"
            git push origin --delete "$1"
            "$1" is ??
        }
        ```

## Change Remote Origin Address
```bash
function gtrchange { ## replace current origin with $1
  git remote -v
  git remote remove origin
  git remote add origin "$1"
  git remote -v
}
```