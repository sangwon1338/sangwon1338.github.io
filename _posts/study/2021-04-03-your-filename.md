---
layout: post
title: '[Git] Fork 한 repository 최신으로 동기화하기'
subtitle: 개요
date: '2021-04-03 24:51:51 +0900'
categories: study
tags: git
comments: true
published: true
---
Fork 한 repository 를 최신으로 동기화시켜야 할 때가 있다.

- 현재 수업에서 과제를 git에 매주 업데이트 되는 것을 동기화해야할때
- Open Source 에 단발성이 아닌 지속적으로 contribution 하려 할 때
- 기타 등등



업데이트 되는 원본 repositoy를 remote repository 로 추가해야 한다.

Fork 해온 repository 에서  remote repository 를 확인 할 수 있다.

```shell
$ git remote -v
```

Output

```shell
origin  https://github.com/YOUR_USERNAME/YOUR_FORK.git (fetch)
origin  https://github.com/YOUR_USERNAME/YOUR_FORK.git (push)
```

동기화해오고 싶은 원본 repository 를 upstream 이라는 이름으로 추가한다.

```sh
 $ git remote add upstream https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git
```

upstream repository 가 제대로 추가 되었는지 확인한다.

```sh
$ git remote -v
```

Output

```shell
origin    https://github.com/YOUR_USERNAME/YOUR_FORK.git (fetch)
origin    https://github.com/YOUR_USERNAME/YOUR_FORK.git (push)
upstream  https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git (fetch)
upstream  https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git (push)
```

upstream repository 로부터 최신 업데이트를 가져와야 한다.

Git 의 fetch 명령어를 통해 upstream repository 의 내용을 가져온다.

```sh
$ git fetch upstream
```

Output

```sh
remote: Counting objects: 75, done.
remote: Compressing objects: 100% (53/53), done.
remote: Total 62 (delta 27), reused 44 (delta 9)
Unpacking objects: 100% (62/62), done.
From https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY
 * [new branch]      master     -> upstream/master
```

upstream repository 의 master branch (혹은 원하는 branch) 로부터 나의 local master branch 로 merge 한다.

```shell
$ git checkout master
```

Output

```sh
Switched to branch 'master'
```

```sh
$ git merge upstream/master
```

Output

```sh
Updating a422352..5fdff0f
Fast-forward
 README                    |    9 -------
 README.md                 |    7 ++++++
 2 files changed, 7 insertions(+), 9 deletions(-)
 delete mode 100644 README
 create mode 100644 README.md
```

최종적으로 push 하여 remote repository 에도 적용시켜주면 완료!

```sh
$ git push origitn master
```

Reference

[1]: https://help.github.com/articles/syncing-a-fork/
[2]: https://help.github.com/articles/configuring-a-remote-for-a-fork/

