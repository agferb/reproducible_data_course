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

```bash
git config --global core.editor "nano"        # Nano
git config --global core.editor "code --wait" # VSCode
```

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
git remote set-url origin git@github.com:<username>/<repo>.git
git push --set-upstream origin main
```

Your SSH key password will be asked, remember it!

#### Push x Pull

- `push`: local to remote

- `pull`: remote to local

```bash
git push
git pull
```
