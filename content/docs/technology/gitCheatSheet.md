+++
title = 'GIT'
date = 2024-04-04T15:29:34+01:00
draft = false
tags = ["GIT"]
categories = ["tutorials", "git"]
+++

### Cheat Sheet

> git user   
> git email   

> git init   
> git status   

> git log   

> git checkout   
> git branch   
> git checkout master   

---

> git init -b main   
*git add all*   
> git add .   
> git commit -m "..."   
> git remote add origin <url>  
> git push origin main -f   

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

## Hugo Command
> hugo server

Test the run from workflow on [GitHub Actions](https://github.com/arshadhs/arshadhs.github.io/actions)

---

{{< home-link "Home" >}} | {{< section-index >}}
