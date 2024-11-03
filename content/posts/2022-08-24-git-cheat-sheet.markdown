---
layout:      "post"
url:         "/git-cheat-sheet/"
title:       "Git Cheat Sheet"
subtitle:    "Useful git commands"
description: "Useful git commands"
date:        "2022-08-24 19:37:00"
author:      "Wander Costa"
header_img:  "/img/post-bg-branches.jpg"
thumb_img:   "/img/post-thumb-branches.jpg"
img_credits: "brgfx"
img_credits_url: https://www.freepik.com/author/brgfx
img_credits_real_url: https://www.freepik.com/free-vector/set-tree-branch_3388205.htm
tags:

- git

---

Below is a table with commands I don’t run frequently enough to remember them, and have to rely on searching on internet
again and again. Now it’s saved somewhere. 😀

Last update: October 3rd, 2023.

| **Operation**                    | **Command**                                  |
|:---------------------------------|:---------------------------------------------|
| Commit without files             | `git commit -m "some message" --allow-empty` |
| List commits in one line each    | `git log --oneline`                          |
|                                  |                                              |
| Remove local tag                 | `git tag -d <tag_name>`                      |
| Remove remote tag                | `git push --delete origin <tag_name>`        |
|                                  |                                              |
| Stash changes                    | `git stash`                                  |
| Retrieve changes from stash      | `git stash pop`                              |
|                                  |                                              |
| Delete local uncommitted changes | `git reset --hard`                           |
| Delete commits until ID          | `git reset --hard <commit_ID>`               |
| Delete local changes to file     | `git restore <file>`                         |
| Revert a commit                  | `git revert <commit_ID>`                     |
|                                  |                                              |
| Rebase                           | `git rebase <branch>`                        |
| Rebase interactive               | `git rebase -i <branch>`                     |
|                                  |                                              |
| Add a remote                     | `git remote add <alias> <uri>`               |

What about you? Do you want to share git commands you always forget?
