# Git Command Reference (CS 46B Lab 12)

Every git and GitHub command used in this lab, what it does, and when to use it.

---

## 1. Installing git and the GitHub CLI

| OS | Install git | Install gh |
|---|---|---|
| Windows | `winget install Git.Git` | `winget install GitHub.cli` |
| Linux (Debian/Ubuntu) | `sudo apt install git` | `sudo apt install gh` |
| macOS | `brew install git` | `brew install gh` |

On macOS, install Homebrew first with the command on https://brew.sh. Run `brew` without `sudo`; Homebrew refuses to run as root.

```bash
git --version        # check that git is installed
brew help            # / winget help / apt help: how to use the package manager
```

---

## 2. Telling git who you are (one-time setup)

```bash
git config --global user.name  "<your-username>"
git config --global user.email "<your.email@example.com>"
git config --global --list     # show your current settings
```

Every commit you make is labeled with this name and email.

---

## 3. Authentication

### HTTPS (with the GitHub CLI)
```bash
gh --help            # list gh commands
gh auth login        # log in; pick "GitHub.com" and then "HTTPS"
gh auth status       # check that you're logged in
```

### SSH
```bash
ls ~/.ssh                                   # check whether you already have a key
ssh-keygen -t rsa -C "<username> <email>"   # make a key; press Enter at every prompt
cat ~/.ssh/id_rsa.pub                       # print the PUBLIC key
ssh -T git@github.com                       # test the connection
```
Paste the output of `cat ~/.ssh/id_rsa.pub` into **GitHub → Settings → SSH and GPG keys → New SSH key**.

> Only share the `.pub` file. `id_rsa` without `.pub` is your **private** key. Never paste it anywhere.

---

## 4. Starting a repository

```bash
cd <project-folder>      # the folder that holds src/
touch .gitignore         # macOS/Linux
New-Item -Path . -Name ".gitignore" -ItemType "File"   # Windows PowerShell
git init                 # turn this folder into a git repository (creates .git/)
```

`.gitignore` lists the files git should never track, such as `bin/` and `*.class`.

---

## 5. Everyday workflow: status, add, commit

```bash
git status                     # what changed? what's staged? what's untracked?
git add .gitignore src         # stage specific files or folders
git add .                      # stage everything in the current folder
git commit -m "message"        # save the staged changes as a commit
git commit -am "message"       # stage every TRACKED file that changed, then commit
```

- **Untracked**: git sees the file but isn't tracking it yet.
- **Staged**: the file will go into the next commit (after `git add`).
- **Committed**: the change is saved in your local history.

`-a` does **not** pick up brand-new files. You still have to `git add` those first.

---

## 6. Connecting to GitHub and pushing

```bash
git remote add origin git@github.com:<username>/<repo>.git   # SSH
git remote add origin https://github.com/<username>/<repo>   # HTTPS
git remote -v                                                # show the remotes
git remote set-url origin <new-url>                          # change a remote's URL

git push -u origin main   # first push: send main and set it as the upstream
git push                  # every push after that
```

The first time you connect over SSH, git asks
`Are you sure you want to continue connecting (yes/no/[fingerprint])?`. Type `yes`.

---

## 7. Getting changes from others

```bash
git pull                 # download and merge new commits from GitHub
git fetch                # download new commits without merging them
git clone <repo-link>    # copy a whole repo from GitHub onto your computer
```

Run `git pull` **before** you start work and before you push. This prevents most conflicts.

---

## 8. Collaborating with a partner

1. On GitHub, go to your repo → **Settings → Collaborators → Add people** and enter your partner's username.
2. Your partner accepts the invitation sent to their email.
3. Your partner clones your repo:
   ```bash
   mkdir ~/git && cd ~/git
   git clone <your-repo-link>
   ls
   code <directory_name> -r     # open it in VS Code
   ```
4. They edit, then run `git add` → `git commit` → `git push`.
5. You get their changes with `git pull`.

---

## 9. Looking at history

```bash
git log                  # full commit history
git log --oneline        # one line per commit
git diff                 # unstaged changes
git diff --staged        # staged changes
git show <commit>        # what a particular commit changed
```

---

## 10. Undoing things

```bash
git restore <file>             # throw away unstaged changes to a file
git restore --staged <file>    # unstage a file but keep your edits
git commit --amend             # fix the last commit (only if you haven't pushed it yet)
git revert <commit>            # make a new commit that undoes an old one (safe after pushing)
```

---

## 11. Branches

```bash
git branch                     # list branches
git branch <name>              # create a branch
git switch <name>              # switch to a branch (older command: git checkout <name>)
git switch -c <name>           # create a branch and switch to it
git merge <name>               # merge <name> into the branch you're on
git push -u origin <name>      # push a new branch to GitHub
```

---

## Quick cheat sheet

```bash
git pull
# ...edit code...
git status
git add .
git commit -m "Describe what you changed"
git push
```
