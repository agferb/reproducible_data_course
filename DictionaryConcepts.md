# 1. Git Basic Concepts

## 1.1. Dictionary of Concepts

### Differenciating

- Git:
  
  - a tool to track changes in repositories
  
  - a tool to 

- GitHub
  
  - a navigator for using Git
  
  - A management environment for repositories tracking timeline

#### Start Git in project (repository)



#### Commit

Different from 'save'! Whenever you want to keep the current version

```bash
user$ git commit -m "[meaningful message]"
```

#### Meaningful change:

- **Why** was changed

- **How** this addresses previous issues

- **Effects** due to changes

- **Limitations** of the change

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
