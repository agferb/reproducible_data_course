# 1. Git Basic Concepts

## 1.1. Differenciating concepts

- Git:
  
  - A tool to track changes in repositories
  
  - Exists locally

- GitHub
  
  - A management environment /navigator for repositories tracking timeline
  
  - Uses git in the backgroud
  
  - Exists

## 1.2. How to start  `git` in repository

#### Init git in project (repository)

Only once in the repository, no need to reactivate in sub-folders

```bash
git init
```

#### Set git configurations

Check if `git` has your information in the `config` file

```bash
git config --global --list
```

Add or change your information

```bash
git config --global user.name "Your Name"
git config --global user.email "you_email@domain.com"
```

## 1.3. How to add commits

Different from 'save'! Whenever you want to keep the current version, send it to `.git` folder.

**Files need to be saved before commit!**

```bash
git add file1.md file2.md
git commit -m "[meaningful message]"
```

#### Meaningful message

For you and your collaborators to remember tracks

- **Why** was changed

- **How** this addresses previous issues

- **Effects** due to changes

- **Limitations** of the change

#### More detailed message

```bash
git commit
```

Will open text editor to write a commit message.

#### Change to other message editors

- Nano (default, most realiable option):

- ```bash
  git config --global core.editor "nano"
  ```

- Use a specific editor:

- ```bash
  git config --global core.editor "<editor_name> --wait"
  ```
  
  - In which `<editor_name>` can be `code` for VSCode or `cursor` for cursor.

**In Nano:** add comments, press `Ctrl+X`, `Y` and `Enter`

#### Staging area

Commit files separetaly or in blocks? Depends!

- Pros: reduce # of commits

- Cons: might be confusing to access individual changes afterwards

Staging area is the folder to keep files to be tracked before adding a global commit message.

#### Check file status:

```bash
git status
```

Three different types:

- Tracked files

- Changes to be staged

- Changes to be commited (new or modified files)

- Untracked files

## 1.4. Timeline

#### Check commit history

```bash
git log
```

From earliest to oldest commit. States author, date and message. Does **not** show the names of the files.

To filter # of commits / date:

```bash
git log -n --abbrev-commit --since=<> --until=<>
```

To get abbreviated commit identifier:

```bash
git log --abbrev-commit
```

To get short log:

```bash
git log --oneline
```

To get files with changes commited:

```bash
git log --name-only
```

#### Compare 2 commits

```bash
git diff <old> <new>
```

## 1.5. Special files

#### README.md

- Description of project

- Initial page of GitHub repository

#### .gitignore

- List of files to be ignored
  
  - Complete listing of files or folders
  
  - `*.csv` to ignore .csv files
  
  - `!data_to_keep.csv` tracks file anyway

## 1.6. Local x Remote

Add SSH key and link it to GitHub.

- Key should be accesible through home (`~`) folder

- Identification from your computer to be able to commit remotly

#### Pushing a repository for the 1st time

```bash
git remote add origin <github_repository>
```

```bash
git remote set-url origin git@github.com:<username>/<repo>.git
```

```bash
git push --set-upstream origin main
```

Your SSH key password will be asked, remember it!

#### Push x Pull

- `push`: local to remote

- ```bash
  git push
  ```

- `pull`: remote to local

- ```bash
  git pull
  ```

#### Delete connection to remote

```bash
git remote remove <name>
```

Usually `<name> = origin`, but sometimes it can be different. To check run `git remote`.

## 1.7. Undo

#### Repositories

For a deleted repository locally, extract it from GitHub

```bash
git clone <repository_SSH>
```

#### Commits

To undo a uncorrectly made commit **without** undoing the changes, `reset` can be used. For safety reasons, it is prefered to not use the option that deletes changes from commits.

- Files stay modified and staged — ready to re-commit:

- ```bash
  git reset --soft <commit_id>
  ```

- Files are modified but unstaged:

- ```bash
  git reset <commit_id>
  ```

To rewrite a commit (no tracking info lost) **and** its associated changes, `revert` can be used. When called, it opens the commit editor to rewrite it.

- To open editor before rewritting commit:

- ```bash
  git revert <commit_id>
  ```

- To rewrite commit without opening editor:

- ```bash
  git revert --no-edit <commit_id>
  ```

There are multiple ways of referencing the commits in the previous commands

- Type the log ID of the commit

- Use the argument `HEAD`:
  
  - `reset` requires `HEAD~1` or larger, being `~1` the previous commit to be returned to
  
  - `revert` understands `HEAD` as the current commit to be rewritten

## 1.8. Alternative histories - Branches

- Alternative commit history (independent from main branch)

- 1 commit shared with main branch (branching point)

- Good comments on the branch
  
  - Who created it
  
  - Why it was created
  
  - For what it should be used

#### Creating branches

```bash
git branch <branch_name>
```

If no name is provided, a list of all branches will be shown.

How to move between branches:

```bash
git checkout <branch_to_go>
```

Branches need to be push independently. Pushing the first time:

```bash
git push --set-upstream origin <branch_name>
```

Pushing from the 2nd time onwards just need to run `git push` in the branch.
