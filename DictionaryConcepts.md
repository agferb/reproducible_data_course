# 1. Git Basic Concepts

## 1.1. Dictionary of Concepts

### Differenciating

- Git:
  
  - A tool to track changes in repositories
  
  - Exists locally

- GitHub
  
  - A management environment /navigator for repositories tracking timeline
  
  - Uses git in the backgroud
  
  - Exists

#### Start git in project (repository)

Only once in the repository, no need to reactivate in sub-folders

```bash
user$ git init
```

#### Git configurations

Check if `git` has your information in the `config` file

```bash
user$ git config --global --list
```

Add or change your information

```bash
user$ git config --global user.name "Your Name"
user$ git config --global user.email "you_email@domain.com"
```

#### Commit

Different from 'save'! Whenever you want to keep the current version, send it to `.git` folder

```bash
user$ git add file.md
user$ git commit -m "[meaningful message]"
```

#### Meaningful message:

- **Why** was changed

- **How** this addresses previous issues

- **Effects** due to changes

- **Limitations** of the change

#### 
