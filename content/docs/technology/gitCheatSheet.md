+++
title = 'GIT'
date = 2024-04-04T15:29:34+01:00
draft = false
tags = ["GIT"]
categories = ["tutorials", "git"]
+++

## Git Cheat Sheet

---

## 🔧 Configuration (One-time setup)

> git config user.name "Your Name"  
> git config user.email "you@example.com"  

Set username and email for commits.

---

## 📁 Repository Setup

> git init  
> git init -b main  
> git remote add origin <url>  

Initialise a repository and connect it to a remote.

---

## 🔍 Status & Inspection

> git status  
> git log  
> git show <commit>  
> git diff  

Check repository state, history, commit details, and differences.

---

## 🌿 Branching & Navigation

> git branch  
> git checkout branch-name  
> git checkout -b new-branch  
> git switch branch-name  
> git switch -c new-branch  

Create, list, and switch branches.  
(`git switch` is the modern alternative to `checkout`.)

---

## 📦 Staging & Committing

> git add .  
> git commit -m "message"  
> git rm file.txt  

Stage changes, commit them, or remove tracked files.

---

## 🔄 Sync with Remote

> git fetch  
> git pull origin branch-name  
> git push  
> git push --set-upstream origin branch-name  
> git push origin main -f  

Fetch, pull, and push changes between local and remote.  
⚠️ Force push (`-f`) overwrites remote history.

---

## History Manipulation

> git reset <commit>  
> git reset --hard <commit>  
> git merge branch-name  
> git rebase branch-name  

Move HEAD, discard changes, merge branches, or rebase commits.

---

## 🔁 Restore Files (Modern Way)

> git restore file.txt  

Restore file contents without switching branches.
---

## Switch and Restore
-  `git switch` → for changing or creating branches  
- `git restore` → for restoring files  

| **Action** | **Old way (`checkout`)** | **New way (`switch` / `restore`)** |
|-------------|---------------------------|-------------------------------------|
| Switch to an existing branch | `git checkout branch-name` | `git switch branch-name` |
| Create and switch to a new branch | `git checkout -b new-branch` | `git switch -c new-branch` |
| Switch to a branch from another base | `git checkout -b new-branch develop` | `git switch -c new-branch develop` |
| Restore a file to last commit | `git checkout -- file.txt` | `git restore file.txt` |

### 🔹 Command Summary

| **Command** | **Purpose** |
|--------------|-------------|
| `git switch` | Only for switching or creating branches |
| `git restore` | Only for restoring file contents |
| `git checkout` | Old command that can do both, but can be confusing |

---


## 🛠 Hugo

> hugo server  

Run Hugo local development server.

Test the run from workflow on [GitHub Actions](https://github.com/arshadhs/arshadhs.github.io/actions)
---

{{< home-link "Home" >}} | {{< section-index >}}
