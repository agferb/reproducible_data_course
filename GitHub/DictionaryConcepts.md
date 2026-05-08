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

## 1.3. How to make commits

Different from 'save'! Whenever you want to keep the current version, send it to `.git` folder.

**Files need to be saved before commit!**

To commit untracked files (new files), add them first and then commit. You can set a single commit message to multiple files or do the following steps for each file.

```bash
git add <new_file1> <new_file2>
```

```bash
git commit -m "[meaningful message]"
```

For files already tracked, the `add` command can be merged with `commit`. This way **all files with changes**  will be commited.

```bash
git commit -am "[meaningful message]"
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

#### Adding multiple files

- All tracked files:
  
  ```bash
  git add -u
  ```

- All tracked and untracked files:
  
  ```bash
  git add -A
  ```

#### Change to other message editors

- Nano (default, most realiable option):
  
  ```bash
  git config --global core.editor "nano"
  ```

- Use a specific editor:
  
  ```bash
  git config --global core.editor "<editor_name> --wait"
  ```

- - In which `<editor_name>` can be `code` for VSCode or `cursor` for cursor.

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
  
  ```bash
  git push
  ```

- `pull`: remote to local
  
  ```bash
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
  
  ```bash
  git reset --soft <commit_id>
  ```

- Files are modified but unstaged:
  
  ```bash
  git reset <commit_id>
  ```

To rewrite a commit (no tracking info lost) **and** its associated changes, `revert` can be used. When called, it opens the commit editor to rewrite it.

- To open editor before rewritting commit:
  
  ```bash
  git revert <commit_id>
  ```

- To rewrite commit without opening editor:
  
  ```bash
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

#### Pushing branches

To check which branches are available locally and which ones are available remotly, with last commits in each:

```bash
git branch -va
```

Branches that exist remotly but not locally, just access it through `checkout` to have it locally.

Branches that exist locally but not remotly need to be pushed. For the first time:

```bash
git push --set-upstream origin <branch_name>
```

To push branches that already exist remotly just `git push`.

#### Collaborative work

To add collaborators to your GitHub repository (or to be added to others repositories) do:

- Repository &rarr; Settings &rarr; Collaborators &rarr; Add people

When added as a collaborator, you can bring the repository to local storage by:

```bash
git clone <repository_SSH>
```

This will create a link in your computer the the whole depository but **it will only pull the main branch**. To save locally another branch just access it through `checkout`.

#### Pulling branches

To get the most recent update of the repository from GitHub, you should pull it.

```bash
git pull
```

If there are new branches remotly, they will not be saved locally, just linked. Again, to save them locally just access through `checkout`.

## 1.9. Merge & Conflicts

We might want to incorporate the changes in a branch into main, this can be done 2 ways: with merge and with pull request (discussed below).

Merging another branch into the current one will reflect the merge operations into the current branch and not the other. The way to do it is:

```bash
git merge <branch_to_be_merged>
```

After merging it is possible to remove locally on of the branches, though **all branches will exist remotly** if they were pushed at any moment beforehand.

```bash
git branch -d <old_branch>
```

#### Dealing with conflicts from `merge`

Merging branches can lead to conflicts. Conflicts occur when there are incompatible changes between 2 branches. When this happens, temporary views of the conflictuous files will open like below:

```md
<<<<<< HEAD

I'm scamander from "the fantastic beast", Harry Potter collection. Pickett is my friend.

======== I'm Pickett, I'm a male Bowtruckle

>>>>>> 1763765483393gcs92bvc9823csd2344c
```

Below `<<<<<< HEAD` are the changes in main, and after `======` are the changes in the branch to be merged. To resolve it, edit the temporary files however you prefer and commit them:

```bash
git commit -am "[meaningful message over the conflict resolution]"
```

#### Dealing with conflicts from `push`

Conflicts might also occur when you push a branch remotly if your changes are incompatible with the version available. If this happens, git will tell you what to do, but usually these 2 options:

- Pull the remote branch (defualt), then compare conflictuous files (like in with `merge`), make a commit over the conflict and push 
  
  ```bash
  git pull
  ```
  
  ```bash
  git commit -am "[meaningful message over the conflict resolution]"
  ```
  
  ```bash
  git push
  ```

- Git might ask to make a rebase during pull to avoid issus, so you need to add before the last commands: 
  
  ```bash
  git config pull.rebase false
  ```

#### Protecting a branch

To avoid undesirable pushes to a branch (or even its deletion) the owner of the repository in GitHub can set rules to protect a branch. This is usually done to the main branch.

To create rules for the repository the path in GitHub is:

- Repository &rarr; Settings &rarr; Rules → Rulesets → New branch ruleset

Bypass list block defines certain operations that will not be subject to the rules. All branches, the main branch (default) or specific branches can be subject to the created rules in the Target branches block.

Many different rules can be created, the most relevant ones are:

- Restrict deletion

- Require pull request before merging

- Block force pushes

## 1.10. Go back in time - Detached Heads

It is possible to access a previous commit and work starting from it, without reverting it (as in `reset` or `revert`):

```bash
git checkout <commit_id>
```

In this case, git creates a detached head, that is not associated with the branch from which the commit was accessed. You can work on this detached head, make changes and commit them, but to push them you need to connect to them to the repository:

```bash
git switch -c <new_branch_from_detached_head>
```

It is safer to already create this branch, otherwise all your saves will be lost when the terminal is closed.

## 1.11. Tagging & Releasing

#### Tags

Tagging is a git resource to keep note of important commits in a branch, but it is also the first step in creating releases in GitHub.

The syntax to tag the current commit is:

```bash
git tag <tag_name> 
```

To add tags to older commits and/or with messages:

```bash
git tag <tag_name> <commit_id> -m "[message]"
```

#### Releases

Releases are extents of tags in GitHub which usually stand for production versions of your repository that are made available to the general public. Downloads from releases  as .zip files are possible - without cloning repoistory or .git folder.

Releases can be created from the Tags area, accesible from the Code tab. Inside a tag there is a option to create a release. A release note is very recomendable.

## 1.12. Forking & Pull request

#### Fork

When collaborators are added to the repository, all of them can make edits to it - create branches, make commits... A less intrusive option is to fork the repository. When someone works in a forked repository, it can see (and incorporate) changes made in the upstream repository, and push changes making them available only in the fork.

To create a fork of another repository, in Code tab click the Fork option. You can keep the original name of the upstream or change it.

When there are changes in the upstream, you can sync them through the Sync fork option in Code tab.

#### Pull request

If you want to merge your changes to the upstream, you can create a **Pull request**. The owner of the upstream will evaluate the request to accept or deny it. You can create it through the Contribute option in Code tab. 

Pull requests can also be configured for merges inside tha same repo through rulesets (as explained in Protect a branch). This is a safer option for large projects to avoid unecessary merges.

## 1.13. Issues

Issues are a GitHub tool to address enhancements, bugs, documentation and so on to pieces of code in the repository. They are specially useful when working in projects.

There are 2 ways of creating issues:

- Go to Issues tab and then create new issue from there

- Go to the file related to the issue to be created, go to Code view, select the lines of code to create the issue (Tab+Click), click the [...] option that appears and then in Reference in new Issue. This way the selected lines will appear in the issue. 
