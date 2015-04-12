# Git Style Guide

이 Git Style Guide는 [*How to Get Your Change Into the Linux 
Kernel*](https://www.kernel.org/doc/Documentation/SubmittingPatches) 과 
[git man pages](http://git-scm.com/doc), 그 커뮤니티에서 여러 유명한 
문제들로 부터 영감을 얻었습니다.

이 가이드의 번역판은 다음과 같습니다.

* [Chinese Simplified](https://github.com/aseaday/git-style-guide)
* [Portuguese](https://github.com/guylhermetabosa/git-style-guide)

가이드에 참여하고 싶다면 프로젝트를 Fork하고 Pull Request 하세요.

# 목차

1. [브랜치 (Branches)](#브랜치)
2. [커밋 (Commits)](#커밋)
  1. [메세지 (Messages)](#메세지)
3. [병합 (Merging)](#병합)
4. [기타 (Misc.)](#기타)

## 브랜치 

* **짧고 이해하기 쉬운 이름**을 선택하세요.

  ```shell
  # 좋음
  $ git checkout -b oauth-migration

  # 나쁨
  $ git checkout -b login_fix
  ```

* Identifiers from corresponding tickets in an external service (eg. a GitHub
  issue) are also good candidates for use in branch names. For example:

  ```shell
  # GitHub issue #15
  $ git checkout -b issue-15
  ```

* 단어를 분리할 때 **대쉬**를 사용하세요.

* 몇몇 사람들이 **같은 저장소**에서 작업하는 경우, **개인**과 **팀**은 
  각각의 브랜치를 가지도록 해야합니다.
  다음 네이밍을 참고하세요:

  ```shell
  $ git checkout -b feature-a/master # team-wide branch
  $ git checkout -b feature-a/maria  # Maria's personal branch
  $ git checkout -b feature-a/nick   # Nick's personal branch
  ```

  Merge at will the personal branches to the team-wide branch (see ["Merging"](#merging)).
  Eventually, the team-wide branch will be merged to "master".

* Delete your branch from the upstream repository after it's merged (unless
  there is a specific reason not to).

  Tip: Use the following command while being on "master", to list merged
  branches:

  ```shell
  $ git branch --merged | grep -v "\*"
  ```

## 커밋

* 각각의 커밋은 하나의 **논리적 변경**만 허용돼야 합니다. 하나의 커밋에서 여러 파일을  
  변경하지 마세요. 예를 들어, 버그 패치나 성능 최적화 커밋은 분할되어야 합니다.

* 하나의 **논리적 변경**을 여러개로 분산시키지 마세요. 예를 들어, 
  기능 구현과 테스트는 하나의 커밋에서 처리되어야 합니다.

* **자주** 커밋하세요. 작고, 독립된 커밋들은 문제가 생겼을 때 이해하기 
  쉽고 되돌리기 편합니다.

* 커밋은 논리적으로 정렬 되어야 합니다. 예를 들어, **커밋 X**가 **커밋 Y**에 
  의존하는 경우, **커밋 Y**가 **커밋 X**보다 선행되어야 합니다.

### 메세지

* 커밋 메세지를 작성할 때, 터미널 대신 편집기를 사용하세요.:

  ```shell
  # 좋음
  $ git commit

  # 나쁨
  $ git commit -m "Quick fix"
  ```

  Committing from the terminal encourages a mindset of having to fit everything
  in a single line which usually results in non-informative, ambiguous commit
  messages.

* The summary line (ie. the first line of the message) should be
  *descriptive* yet *succinct*. Ideally, it should be no longer than
  *50 characters*. It should be capitalized and written in imperative present
  tense. It should not end with a period since it is effectively the commit
  *title*:

  ```shell
  # good - imperative present tense, capitalized, fewer than 50 characters
  Mark huge records as obsolete when clearing hinting faults

  # bad
  fixed ActiveModel::Errors deprecation messages failing when AR was used outside of Rails.
  ```

* After that should come a blank line followed by a more thorough
  description. It should be wrapped to *72 characters* and explain *why*
  the change is needed, *how* it addresses the issue and what *side-effects*
  it might have.

  It should also provide any pointers to related resources (eg. link to the
  corresponding issue in a bug tracker):

  ```shell
  Short (50 chars or fewer) summary of changes

  More detailed explanatory text, if necessary. Wrap it to
  72 characters. In some contexts, the first
  line is treated as the subject of an email and the rest of
  the text as the body.  The blank line separating the
  summary from the body is critical (unless you omit the body
  entirely); tools like rebase can get confused if you run
  the two together.

  Further paragraphs come after blank lines.

  - Bullet points are okay, too

  - Use a hyphen or an asterisk for the bullet,
    followed by a single space, with blank lines in
    between

  Source http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html
  ```

  Ultimately, when writing a commit message, think about what you would need
  to know if you run across the commit in a year from now.

* If a *commit A* depends on another *commit B*, the dependency should be
  stated in the message of *commit A*. Use the commit's hash when referring to
  commits.

  Similarly, if *commit A* solves a bug introduced by *commit B*, it should
  be stated in the message of *commit A*.

* If a commit is going to be squashed to another commit use the `--squash` and
  `--fixup` flags respectively, in order to make the intention clear:

  ```shell
  $ git commit --squash f387cab2
  ```

  *(Tip: Use the `--autosquash` flag when rebasing. The marked commits will be
  squashed automatically.)*

## 병합

* **Do not rewrite published history.** The repository's history is valuable in
  its own right and it is very important to be able to tell *what actually
  happened*. Altering published history is a common source of problems for
  anyone working on the project.

* However, there are cases where rewriting history is legitimate. These are
  when:

  * You are the only one working on the branch and it is not being reviewed.

  * You want to tidy up your branch (eg. squash commits) and/or rebase it onto
    the "master" in order to merge it later.

  That said, *never rewrite the history of the "master" branch* or any other
  special branches (ie. used by production or CI servers).

* Keep the history *clean* and *simple*. *Just before you merge* your branch:

    1. Make sure it conforms to the style guide and perform any needed actions
       if it doesn't (squash/reorder commits, reword messages etc.)

    2. Rebase it onto the branch it's going to be merged to:

       ```shell
       [my-branch] $ git fetch
       [my-branch] $ git rebase origin/master
       # then merge
       ```

       This results in a branch that can be applied directly to the end of the
       "master" branch and results in a very simple history.

       *(Note: This strategy is better suited for projects with short-running
       branches. Otherwise it might be better to occassionally merge the
       "master" branch instead of rebasing onto it.)*

* If your branch includes more than one commit, do not merge with a
  fast-forward:

  ```shell
  # good - ensures that a merge commit is created
  $ git merge --no-ff my-branch

  # bad
  $ git merge my-branch
  ```

## 기타

* There are various workflows and each one has its strengths and weaknesses.
  Whether a workflow fits your case, depends on the team, the project and your
  development procedures.

  That said, it is important to actually *choose* a workflow and stick with it.

* *Be consistent.* This is related to the workflow but also expands to things
  like commit messages, branch names and tags. Having a consistent style
  throughout the repository makes it easy to understand what is going on by
  looking at the log, a commit message etc.

* *Test before you push.* Do not push half-done work.

* Use [annotated tags](http://git-scm.com/book/en/v2/Git-Basics-Tagging#Annotated-Tags) for
  marking releases or other important points in the history.

  Prefer [lightweight tags](http://git-scm.com/book/en/v2/Git-Basics-Tagging#Lightweight-Tags) for personal use, such as to bookmark commits
  for future reference.

* Keep your repositories at a good shape by performing maintenance tasks
  occasionally, in your local *and* remote repositories:

  * [`git-gc(1)`](http://git-scm.com/docs/git-gc)
  * [`git-prune(1)`](http://git-scm.com/docs/git-prune)
  * [`git-fsck(1)`](http://git-scm.com/docs/git-fsck)

# 라이센스

![cc license](http://i.creativecommons.org/l/by/3.0/88x31.png)

This work is licensed under a Creative Commons Attribution 4.0
International license.

# Credits

Agis Anastasopoulos / [@agisanast](https://twitter.com/agisanast) / http://agis.io

# 번역

Hyun Jun, An / [@cryptosdev](https://twitter.com/cryptosdev)