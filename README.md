# bin
A collection of scripts I use to make my command line existence easier.


**⚠️ Warning: ️⚠️**
These are just some dirty, hacky scripts I wrote for myself with no intention
for them to be ever used by anyone else. There is no quality behind any of these
– neither in code style nor in functionality. I just put them on GitHub as
inspiration for others. I don't guarantee any stability or anything here.  
**Use at your own risk!**



## What do they do?

#### `confirm`
Prefix any risky command with `confirm` and it asks you before it executes it:
```shell
$ confirm echo hello
Really do ⇛ echo hello ⇚ ? y
hello
```

#### `search`
Combines `find` and `grep` into one command.

```shell
$ search git .
Seaching '.' for 'git' (case-insensitive)...

File names
==========
./git-new   
./gitpull

File contents
=============
./README.md
```

#### `highlight`
Highlights a word in stuff you pipe into it. **Warning: slow!**

Essentially does `COMMAND | egrep --color=always "${YOUR_SEARCH}|$"`.

```shell
$ ls -l | highlight git
# imagine some partially colored output here
```
(Can't really show the output, as markdown has no colors.)


#### `httpserve`
Serves the current directory (using pythons `http.server` module)
on `localhost:8080`, and opens your default browser.

```shell
$ httpserve 
Serving /your/current/dir
Serving HTTP on 127.0.0.1 port 8080 (http://127.0.0.1:8080/) ...
```

#### `man`
Moved to this repo: [man_sections](https://github.com/0bmxa/man_sections).


#### `package`
Tries to consolidate package manager functions into one.

Not very elaborate, yet. Only supports so far:
- `package outdated` runs:
    - `brew outdated`
    - `gem outdated`
    - `pip3 list --outdated`
- `package update` runs:
    - `brew update && brew upgrade`
    - `sudo gem update`
    - `pip3 install --upgrade <PACKAGE_NAME>`



### Git stuff

#### `git last`
Shows the (up to) last 5 branches you used! **Super useful!**

```shell
$ git last
refs/heads/master
refs/heads/feature/consistency
refs/heads/readme
```


#### `git new`
A wrapper for `git checkout -b`.

Creates a new branch, but always first updates the base branch.
Base branch can be specified (default `develop`).

```shell
$ git new feature/more_tests my_custom_base
Really do ⇛ git checkout my_custom_base ⇚ ? y
Switched to branch 'my_custom_base'
Your branch is up to date with 'origin/my_custom_base'.

Really do ⇛ git pull origin my_custom_base ⇚ ? y
From github.com:0bmxa/sample_repo
 * branch            my_custom_base     -> FETCH_HEAD
Already up to date.

Really do ⇛ git checkout -b feature/more_tests ⇚ ? y
Switched to a new branch 'feature/more_tests'
```


#### `git refresh`
A wrapper for `git rebase`.

Updates the base branch (default: `develop`) and rebases onto it. Also asks for
`stash` if there are uncommitted changes.

Usage: `git refresh [my_base_branch]`


#### `git finalize`
Finalizes a feature branch, by running:

1. `git refresh`
2. An interactive `rebase` against your base branch
3. A `--force push` to your branch
4. Creating a PR (using [hub](https://github.com/github/hub))


#### `git yolo`

Adds everything (`git add -A`), commits with a message from `whatthecommit.com`,
and pushes (no force) to origin. All without asking.

**Why?** I find it useful for trying out things on CI, where your commit is
essentially your build trigger, and I'm anyways cleaning up my commit history
before makign a PR.

Yes, `curl`ing a commit message can be risky.


#### `gitpull` (yes, no space)
Simply runs `confirm git pull origin BRANCHNAME`,
where `BRANCHNAME` is determinded by `git rev-parse --abbrev-ref HEAD`.
(`confirm` see above.)

```shell
$ gitpull
Really do ⇛ git pull origin master ⇚ ?
```


#### `gitpush` (yes, no space)
A wrapper around `git push`, which (reminds you to) and offers to rebase before
pushing.

```shell
$ gitpush
Did you rebase against develop ? n
Really do ⇛ git refresh ⇚ ?
```
```shell
$ gitpush my_base_branch
Did you rebase against my_base_branch ? y
Really do ⇛ git push -u origin feature/more_tests ⇚ ?
```



### Specific to what I do

#### `diffswitcher`
Xcode project file diff. See details in [this gist](https://gist.github.com/0bmxa/6ea1e545bce373af1bfa4230009940c4).

#### `restart-coreaudiod`
Restarts the macOS audio server and some tools depending on it.  
Will be replaced soon by [this one](https://github.com/0bmxa/Pancake/blob/master/Scripts/restart-coreaudiod.sh)
(not yet public).

