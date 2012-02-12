========================
 Mercurial チートシート
========================

はじめに
--------
- mercurialのチートシートは `bitbucketに移動しました`_

.. _`bitbucketに移動しました`: https://bitbucket.org/troter/mercurial-cheatsheet

Mercurialとは
-------------
TODO:

最初の設定
----------

設定ファイルの場所
^^^^^^^^^^^^^^^^^^

.. list-table::
   :widths: 10 10
   :header-rows: 1

   * - Windows
     - Linux/MacOSX/Cygwin
   * - %USERPROFILE%\Mercurial.ini
     - $HOME/.hgrc

ユーザ名の設定
^^^^^^^^^^^^^^
設定ファイルに次の項目を追加します。

  ::

    [ui]
    username=Takumi IINO <takumi@timedia.co.jp>

helpの参照
----------

help(hg help)
^^^^^^^^^^^^^
  ::

    % hg help

サブコマンドのhelp(hg help [COMMAND])
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  ::

    % hg help [コマンド]

  ::

    % hg help help           # helpのhelpを表示

グローバルオプションを表示
^^^^^^^^^^^^^^^^^^^^^^^^^^
hgのオプションに-vを追加する

  ::

    % hg -v help log

リポジトリを用意する
--------------------

リポジトリの新規作成(hg init)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  ::

    % mkdir hello
    % cd hello
    % hg init

リポジトリのクローン(hg clone)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  ::

    % hg clone http://selenic.com/hg mercurial-repo
    requesting all changes
    adding changesets
    adding manifests
    adding file changes
    added 13537 changesets with 26617 changes to 1971 files
    updating to branch default
    872 files updated, 0 files merged, 0 files removed, 0 files unresolved
    %

確認を行う
----------

コミットログを確認する(hg log)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  ::

    % hg log       # 全てのログを表示
    % hg log -l 10 # 最新10個のログを表示
    % hg log -l 10 --branch default # ブランチを指定してログを表示

サマリー(hg summary)
^^^^^^^^^^^^^^^^^^^^
  ::

    % hg summary
    parent: 13536:fac040b7e822 tip
     merge: drop resolve state for mergers with identical contents (issue2680)
    branch: default
    commit: (clean)
    update: (current)
    %

現在のリビジョン(hg parents)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  ::

    % hg parents
    changeset:   13536:fac040b7e822
    tag:         tip
    user:        Matt Mackall <mpm@selenic.com>
    date:        Sat Mar 05 16:34:59 2011 -0600
    summary:     merge: drop resolve state for mergers with identical contents (issue2680)
    
    %

最新のリビジョン(hg tip)
^^^^^^^^^^^^^^^^^^^^^^^^
  ::

    % hg tip
    changeset:   13536:fac040b7e822
    tag:         tip
    user:        Matt Mackall <mpm@selenic.com>
    date:        Sat Mar 05 16:34:59 2011 -0600
    summary:     merge: drop resolve state for mergers with identical contents (issue2680)
    
    %

現在のブランチ(hg branch)
^^^^^^^^^^^^^^^^^^^^^^^^^
  ::

    % hg branch
    default
    %

ブランチの一覧とブランチ毎の最新のリビジョン(hg branches)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  ::

    % hg branches
    default                    13536:fac040b7e822
    stable                     13534:4ec34de8bbb1 (inactive)
    %

移動を行う
----------

指定したリビジョンに移動(hg update [REV])
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  ::

    % hg update [リビジョン]
    #+END_SRC
    #+BEGIN_SRC sh
    % hg parent --template "{rev}\n"
    13536
    % hg update 13524 # リビジョン 13524に移動
    10 files updated, 0 files merged, 0 files removed, 0 files unresolved
    % hg parent --template "{rev}\n"
    13524
    %

最新のリビジョンに移動(hg update)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  ::

    % hg update
    #+END_SRC
    #+BEGIN_SRC sh
    % hg parent --template "{rev}\n"
    13524
    % hg update # 最新のリビジョンに移動
    10 files updated, 0 files merged, 0 files removed, 0 files unresolved
    % hg parent --template "{rev}\n"
    13536
    %

ブランチの移動(hg update [BRANCH])
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  ::

    % hg update [ブランチ名]

  ::

    % hg branch
    default
    % hg update stable
    22 files updated, 0 files merged, 0 files removed, 0 files unresolved
    % hg branch
    stable
    %

ファイルの操作
--------------
操作のための新しいリポジトリを作りましょう

  ::

    % mkdir hello-repo
    % cd hello-repo
    % hg init

ファイルを追加する(hg add)
^^^^^^^^^^^^^^^^^^^^^^^^^^
  ::

    % echo 'puts "Hello, mercurial."' > hello.rb
    % hg add hello.rb
    %

コミットする(hg commit)
^^^^^^^^^^^^^^^^^^^^^^^
  ::

    % hg tip
    changeset:   -1:000000000000
    tag:         tip
    user:
    date:        Thu Jan 01 00:00:00 1970 +0000
    
    % hg commit -m "add hello.rb"
    % hg tip
    changeset:   0:c0d1b673238b
    tag:         tip
    user:        Takumi IINO <takumi@timedia.co.jp>
    date:        Sun Mar 06 22:27:01 2011 +0900
    summary:     add hello.rb
    
    %

変更を確認する(hg diff)
^^^^^^^^^^^^^^^^^^^^^^^
  ::

    % sed -i -e s/m/M/ hello.rb
    % hg diff
    diff -r c0d1b673238b hello.rb
    --- a/hello.rb  Sun Mar 06 22:27:01 2011 +0900
    +++ b/hello.rb  Sun Mar 06 22:34:35 2011 +0900
    @@ -1,1 +1,1 @@
    -puts "Hello, mercurial."
    +puts "Hello, Mercurial."
    %
    % # もう一つ追加してみる
    % echo 'print "Hello, Mercurial.\n";' > hello.pl
    % hg add hello.pl
    % hg diff hello.pl
    diff -r c0d1b673238b hello.pl
    --- /dev/null   Thu Jan 01 00:00:00 1970 +0000
    +++ b/hello.pl  Sun Mar 06 22:36:56 2011 +0900
    @@ -0,0 +1,1 @@
    +print "Hello, Mercurial.\n";
    %

変更されたファイル一覧(hg status)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  ::

    % hg status
    M hello.rb
    A hello.pl
    %

変更を取り消す(hg revert)
^^^^^^^^^^^^^^^^^^^^^^^^^
  ::

    % hg revert hello.pl
    % hg status
    M hello.rb
    ? hello.pl
    %
    % hg add hello.pl # またaddしておこう

コミットを取り消す(hg rollback)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  ::

    % hg commit -m "add perl sample" # 二つの変更をコミットしてしまった
    % hg diff -c 1
    diff -r c0d1b673238b -r 30b4e1e501a3 hello.pl
    --- /dev/null   Thu Jan 01 00:00:00 1970 +0000
    +++ b/hello.pl  Sun Mar 06 22:42:41 2011 +0900
    @@ -0,0 +1,1 @@
    +print "Hello, Mercurial.\n";
    diff -r c0d1b673238b -r 30b4e1e501a3 hello.rb
    --- a/hello.rb  Sun Mar 06 22:27:01 2011 +0900
    +++ b/hello.rb  Sun Mar 06 22:42:41 2011 +0900
    @@ -1,1 +1,1 @@
    -puts "Hello, mercurial."
    +puts "Hello, Mercurial."
    %
    % hg rollback
    repository tip rolled back to revision 0 (undo commit)
    working directory now based on revision 0
    %
    % hg commit -m "camelize" hello.rb
    % hg commit -m "add perl sample"
    %

最新のコミットのみrollback可能

  ::

    % hg log --template "{rev}:{node}: {desc}\n"
    2:c0266fae871b5783d4f4a50faf0694d41df01418: add perl sample
    1:f491ca2a61140034ed906d7d45893838493246c8: camelize
    0:c0d1b673238bd257f79a7c2779f1e0d8e24d3524: add hello.rb
    %
    % hg rollback
    repository tip rolled back to revision 1 (undo commit)
    working directory now based on revision 1
    %
    % hg rollback
    no rollback information available
    %
    % hg log --template "{rev}:{node}: {desc}\n"
    1:f491ca2a61140034ed906d7d45893838493246c8: camelize
    0:c0d1b673238bd257f79a7c2779f1e0d8e24d3524: add hello.rb
    %
    % hg commit -m "add perl sample"
    % hg log --template "{rev}:{node}: {desc}\n"
    2:c0266fae871b5783d4f4a50faf0694d41df01418: add perl sample
    1:f491ca2a61140034ed906d7d45893838493246c8: camelize
    0:c0d1b673238bd257f79a7c2779f1e0d8e24d3524: add hello.rb
    %

multiple headsに関わる操作
--------------------------
multiple headsとは名前無しブランチが複数ある状態の事である。

multiple headsを作る(hg update [REV] & hg commit)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  ::

    % hg log --template "{rev}:{node}: {desc}\n"
    2:c0266fae871b5783d4f4a50faf0694d41df01418: add perl sample
    1:f491ca2a61140034ed906d7d45893838493246c8: camelize
    0:c0d1b673238bd257f79a7c2779f1e0d8e24d3524: add hello.rb
    %
    % # 一つ前に戻る
    % hg update 1
    0 files updated, 0 files merged, 1 files removed, 0 files unresolved
    % hg parents --template "{rev}:{node}\n"
    1:f491ca2a61140034ed906d7d45893838493246c8
    %
    % # 二つ目のheadsを作る
    % echo 'print "Hello, Mercurial."' > hello.py
    % hg add hello.py
    % hg commit -m "add python sample"
    created new head
    %

multiple headsの確認(hg heads)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  ::

    % hg heads
    changeset:   3:980f8866917a
    tag:         tip
    parent:      1:f491ca2a6114
    user:        Takumi IINO <takumi@timedia.co.jp>
    date:        Mon Mar 07 00:10:18 2011 +0900
    summary:     add python sample
    
    changeset:   2:46f0166b17d8
    user:        Takumi IINO <takumi@timedia.co.jp>
    date:        Sun Mar 06 22:53:57 2011 +0900
    summary:     add perl sample
    
    %

2つのmultiple headsの統合(hg merge)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  ::

    % hg merge
    1 files updated, 0 files merged, 0 files removed, 0 files unresolved
    (branch merge, don't forget to commit)
    % hg status
    M hello.pl
    %
    % # この状態でparentsを確認すると二つあることがわかる。
    % hg parents --template "{rev}:{node}\n"
    3:980f8866917a1098d08f1e1b85dc396fecbc83ad
    2:46f0166b17d886637c30e6f486b23043be56b22e
    %
    % hg commit -m "merge changeset: 2:46f0166b17d8"
    % hg heads
    changeset:   4:4b83e608a7d0
    tag:         tip
    parent:      3:980f8866917a
    parent:      2:46f0166b17d8
    user:        Takumi IINO <takumi@timedia.co.jp>
    date:        Mon Mar 07 00:17:50 2011 +0900
    summary:     merge changeset: 2:46f0166b17d8
    
    % ls
    hello.pl  hello.py  hello.rb
    %

3つのmultiple headsの統合(hg merge -r [REV])
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

3つheadの作成
"""""""""""""
  ::

    % # 二つ目のheadを作る
    % hg update 3
    0 files updated, 0 files merged, 1 files removed, 0 files unresolved
    % echo '(display "Hello, Mercurial.")(newline)' > hello.scm
    % hg add hello.scm
    % hg commit -m "add scheme sample"
    created new head
    %
    % # 三つ目のheadを作る
    % hg update 3
    0 files updated, 0 files merged, 1 files removed, 0 files unresolved
    % echo '(princ (format nil "Hello, Mercurial.~%"))' > hello.cl
    % hg add hello.cl
    % hg commit -m "add common lisp sample"
    created new head
    %
    % hg heads
    changeset:   6:6a0eac3064c9
    tag:         tip
    parent:      3:980f8866917a
    user:        Takumi IINO <takumi@timedia.co.jp>
    date:        Mon Mar 07 00:34:33 2011 +0900
    summary:     add common lisp sample
    
    changeset:   5:bcb5dec879f9
    parent:      3:980f8866917a
    user:        Takumi IINO <takumi@timedia.co.jp>
    date:        Mon Mar 07 00:22:44 2011 +0900
    summary:     add scheme sample
    
    changeset:   4:4b83e608a7d0
    parent:      3:980f8866917a
    parent:      2:46f0166b17d8
    user:        Takumi IINO <takumi@timedia.co.jp>
    date:        Mon Mar 07 00:17:50 2011 +0900
    summary:     merge changeset: 2:46f0166b17d8
    
    %

統合
""""
単純なmergeは失敗する

  ::

    % hg merge
    abort: branch 'default' has 3 heads - please merge with an explicit rev
    (run 'hg heads .' to see heads)

リビジョンを指定してmergeを行う

  ::

    % hg parents --template "{rev}:{node}\n"
    6:6a0eac3064c9543384538a5f3ce8e28ad21f5db1
    %
    % # 一つ目のmerge
    % hg merge -r 4
    1 files updated, 0 files merged, 0 files removed, 0 files unresolved
    (branch merge, don't forget to commit)
    %
    % # いっぺんに複数のマージは行えない
    % hg merge -r 5
    abort: outstanding uncommitted merges
    %
    % # 一つ目をコミット
    % hg commit -m "Merged changes"
    %
    % # 二つ目のmergeとコミット
    % hg merge -r 5
    1 files updated, 0 files merged, 0 files removed, 0 files unresolved
    (branch merge, don't forget to commit)
    % hg commit -m "Merged changes"
    %
    % # headの統合が完了
    % hg heads
    changeset:   8:48d139b4230f
    tag:         tip
    parent:      7:89f3c6e6d974
    parent:      5:bcb5dec879f9
    user:        Takumi IINO <takumi@timedia.co.jp>
    date:        Mon Mar 07 00:47:37 2011 +0900
    summary:     Merged changes
    
    %

衝突の解決(hg resolve)
^^^^^^^^^^^^^^^^^^^^^^

衝突するシュチュエーション
""""""""""""""""""""""""""
二つのheadで別々の修正を行う

  ::

    % hg parents --template "{rev}:{node}\n"
    8:48d139b4230f7db36105b605d5f85e01a1b0efb0
    %
    % echo "all: scm\n\nscm:\n\tgosh hello.scm\n" > Makefile
    % hg add Makefile
    % hg ci -m "run with gosh"
    %
    % hg update 8
    % echo "all: scm\n\nscm:\n\tguile hello.scm\n" > Makefile
    % hg add Makefile
    % hg ci -m "run with guile"
    %
    % hg heads --template "{rev}:{node} {desc}\n"
    10:47589976d454f75dc26bd8f99a786fac408e8b14 run with guile
    9:821a2430ed2f5607bb5da42ee6ffb77d7a88fa55 run with gosh
    %

衝突の発生
""""""""""
  ::

    % hg merge
    merging Makefile
    warning: conflicts during merge.
    merging Makefile failed!
    0 files updated, 0 files merged, 0 files removed, 1 files unresolved
    use 'hg resolve' to retry unresolved file merges or 'hg update -C .' to abandon
    %

コンフリクト時の状態を詳しく見てみる

  ::

    % hg status
    M Makefile        # svnのように C ではない
    ? Makefile.orig
    %
    % # 衝突したファイル一覧
    % hg resolve -l
    U Makefile
    %
    % cat Makefile
    all: scm
    
    scm:
    <<<<<<< local
            guile hello.scm
    =======
            gosh hello.scm
    >>>>>>> other
    
    % hg commit -m "解決しないとコミットできない"
    abort: unresolved merge conflicts (see hg resolve)
    %

衝突の解決
""""""""""
  ::

    % vi Makefile
    % hg diff Makefile
    diff -r 47589976d454 Makefile
    --- a/Makefile  Mon Mar 07 21:13:58 2011 +0900
    +++ b/Makefile  Mon Mar 07 21:42:16 2011 +0900
    @@ -1,5 +1,5 @@
     all: scm
    
     scm:
    -       guile hello.scm
    +       gosh hello.scm
    
    %
    % # 解決済みのマークをつける
    % hg resolve -m Makefile
    % hg resolve -l
    R Makefile
    %
    % # コミット
    % hg commit -m "guileがelispを置き換えるなら考える"
    % hg heads
    changeset:   11:1597cc35cade
    tag:         tip
    parent:      10:47589976d454
    parent:      9:821a2430ed2f
    user:        Takumi IINO <takumi@timedia.co.jp>
    date:        Mon Mar 07 21:52:53 2011 +0900
    summary:     guileがelispを置き換えるなら考える
    
    %

ブランチの操作
--------------

ブランチの作成(hg branch [NAME])
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  ::

    % hg branch makefile_fix
    marked working directory as branch makefile_fix
    % hg branch
    makefile_fix
    %
    % # コミット前はブランチ一覧には登場しない
    % hg branches
    default                       11:1597cc35cade
    %
    % # hg summaryでブランチの次のコミットの確認
    % hg summary
    parent: 11:1597cc35cade tip
     guileがelispを置き換えるなら考える
    branch: makefile_fix
    commit: 1 unknown (new branch)
    update: (current)
    %
    % hg commit -m "start makefile_fix branch"
    % hg branches
    makefile_fix                  12:7293a6112d50
    default                       11:1597cc35cade (inactive)
    %

"別のブランチ"の変更の取り込み(hg merge)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

makefile_fixブランチに変更を加える
""""""""""""""""""""""""""""""""""
  ::

    % hg update makefile_fix
    % vi Makefile
    % hg diff
    diff -r 84d4c7bf2648 Makefile
    --- a/Makefile  Tue Mar 08 00:11:47 2011 +0900
    +++ b/Makefile  Tue Mar 08 00:16:29 2011 +0900
    @@ -3,3 +3,6 @@
     scm:
            gosh hello.scm
    
    +rb:
    +       ruby hello.rb
    +
    % hg commit -m "run ruby"

makefile_fixの変更をdefaultブランチに取り込む
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  ::

    % hg up default
    1 files updated, 0 files merged, 0 files removed, 0 files unresolved

makefile_fixの変更をdefaultブランチに取り込む

  ::

    % hg merge makefile_fix
    1 files updated, 0 files merged, 0 files removed, 0 files unresolved
    (branch merge, don't forget to commit)
    % hg ci -m "merge makefile_fix"
    % hg log -l 1
    changeset:   15:4e0ddd138f6b
    tag:         tip
    parent:      11:1597cc35cade
    parent:      14:3cb402ea1e44
    user:        Takumi IINO <takumi@timedia.co.jp>
    date:        Sun Mar 20 21:58:05 2011 +0900
    summary:     merge makefile_fix
    
    %

ブランチを閉じる(hg commit --close-branch)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  ::

    % hg branch
    default
    [takumi@takumi-THINK:~/sandbox/hello-repo.back(2)]
    % hg up makefile_fix
    0 files updated, 0 files merged, 0 files removed, 0 files unresolved
    % hg commit --close-branch -m "finish."
    % hg branch
    makefile_fix
    % hg branches
    default                       15:4e0ddd138f6b
    % hg branches --closed
    default                       15:4e0ddd138f6b
    makefile_fix                  16:f976730a0346 (closed)
    % hg up default
    0 files updated, 0 files merged, 0 files removed, 0 files unresolved
    %

リポジトリ間の操作
------------------
まずリポジトリをクローンする
  ::

    % hg clone hello-repo hello-repo-haskell
    updating to branch default
    6 files updated, 0 files merged, 0 files removed, 0 files unresolved

"別のリポジトリの同じブランチ"の変更の取り込み(hg pull)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

元リポジトリに変更を加える
""""""""""""""""""""""""""
  ::

    % cd hello-repo
    % vi Makefile
    % hg di Makefile
    diff -r f976730a0346 Makefile
    --- a/Makefile  Sun Mar 20 22:00:08 2011 +0900
    +++ b/Makefile  Sun Mar 20 22:26:50 2011 +0900
    @@ -6,3 +6,6 @@
     rb:
            ruby hello.rb
    
    +py:
    +       python hello.py
    +
    % hg ci -m "run python"

変更を取り込む
""""""""""""""
  ::

    % cd ../hello-repo-haskell
    % hg pull
    pulling from /home/takumi/sandbox/hello-repo
    searching for changes
    adding changesets
    adding manifests
    adding file changes
    added 1 changesets with 1 changes to 1 files
    (run 'hg update' to get a working copy)
    % hg update
    1 files updated, 0 files merged, 0 files removed, 0 files unresolved
    %

すでに変更を加えていた場合の"別のリポジトリの同じブランチ"の変更の取り込み(hg pull)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

再度hello-repoに変更を加える
""""""""""""""""""""""""""""
  ::

    % cd ../hello-repo
    % vi Makefile
    % hg diff Makefile
    diff -r 7cd901509845 Makefile
    --- a/Makefile  Sun Mar 20 22:29:22 2011 +0900
    +++ b/Makefile  Sun Mar 20 22:39:33 2011 +0900
    @@ -9,3 +9,6 @@
     py:
            python hello.py
    
    +pl:
    +       perl hello.pl
    +
    % hg ci -m "run perl"
    %

hello-repo-haskellに変更を加える
""""""""""""""""""""""""""""""""
  ::

    % cd ../hello-repo-haskell
    % echo 'main :: IO()\nmain = putStrLn "Hello, Mercurial."' > hello.hs
    % hg add hello.hs
    % hg ci -m "add haskell sample"
    %

hello-repoの変更を取り込む
""""""""""""""""""""""""""
  ::

    % hg pull
    pulling from /home/takumi/sandbox/hello-repo
    searching for changes
    adding changesets
    adding manifests
    adding file changes
    added 1 changesets with 1 changes to 1 files (+1 heads)
    (run 'hg heads' to see heads, 'hg merge' to merge)
    %
    % hg heads --template "{rev}:{node} {desc|firstline}\n"
    19:e706d4fd6805afef5118761f33e2e5605da97a8a run perl
    18:ad81adf49b423cc6d2e262d89c7e72c3781e04d5 add haskell sample
    %
    % hg parents --template "{rev}:{node}\n"
    18:ad81adf49b423cc6d2e262d89c7e72c3781e04d5
    %

新しいheadsができた。mercurialでは複数人で協調する場合に常に常にmultipleheadsを意識する必要がある。というわけでマージする。 [rebase]_

  ::

    % hg merge
    1 files updated, 0 files merged, 0 files removed, 0 files unresolved
    (branch merge, don't forget to commit)
    % hg ci -m "merged"
    % hg heads
    changeset:   20:768d84182c71
    tag:         tip
    parent:      18:ad81adf49b42
    parent:      19:e706d4fd6805
    user:        Takumi IINO <takumi@timedia.co.jp>
    date:        Sun Mar 20 22:51:00 2011 +0900
    summary:     merged
    
    %

headsがひとつになったことが確認できる。 [fetch]_

"別のリポジトリの同じブランチ"へ変更の送信(hg push)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

headsが一つの場合
"""""""""""""""""
  ::

    % pwd
    /home/takumi/sandbox/hello-repo-haskell
    % hg push
    pushing to /home/takumi/sandbox/hello-repo
    searching for changes
    adding changesets
    adding manifests
    adding file changes
    added 2 changesets with 1 changes to 1 files
    %

headsが二つの場合
"""""""""""""""""
  ::

    % hg parents --template "{rev}:{node}\n"
    20:768d84182c71aee58e321cc2c152a0c8484d7cc5
    % hg up 19
    % vi Makefile
    n% hg di
    diff -r e706d4fd6805 Makefile
    --- a/Makefile  Sun Mar 20 22:39:57 2011 +0900
    +++ b/Makefile  Sun Mar 20 23:06:08 2011 +0900
    @@ -12,3 +12,6 @@
     pl:
            perl hello.pl
    
    +hs:
    +       hugs hello.hs
    +
    % hg ci -m "run hugs"
    created new head
    %
    % hg push
    pushing to /home/takumi/sandbox/hello-repo
    searching for changes
    abort: push creates new remote heads on branch 'default'!
    (did you forget to merge? use push -f to force)

通常、別のリポジトリにmultipleheadsを強要する事は不適切である（ブランチを作成するべき）。諦めてmergeする。

  ::

    % hg merge
    1 files updated, 0 files merged, 0 files removed, 0 files unresolved
    (branch merge, don't forget to commit)
    % hg ci -m "merged"
    % hg push
    pushing to /home/takumi/sandbox/hello-repo
    searching for changes
    adding changesets
    adding manifests
    adding file changes
    added 2 changesets with 1 changes to 1 files
    %

タグの操作
----------

タグをつける(hg tag)
^^^^^^^^^^^^^^^^^^^^
  ::

    % hg parents --template "{rev}:{node}\n"
    22:650e447b9fb133f3013b61acb2d36145f1c3e6cd
    %
    % hg tag v0.0.1
    % hg log -l 1
    changeset:   23:5d7a5fdc0b1a
    tag:         tip
    user:        Takumi IINO <takumi@timedia.co.jp>
    date:        Sun Mar 20 23:12:38 2011 +0900
    summary:     Added tag v0.0.1 for changeset 650e447b9fb1
    
    % 

よく見ると.hgtagsというファイルが新規作成されている。

  ::

    % ls -a
    ./  ../  .hg/  .hgtags  hello.cl  hello.hs  hello.pl  hello.py  hello.rb  hello.scm  Makefile
    % cat .hgtags
    650e447b9fb133f3013b61acb2d36145f1c3e6cd v0.0.1
    %

次に読むべき記事
----------------
- `Git使いがMercurial使いに転職するとき設定しておくべきMercurial拡張`_

.. _`Git使いがMercurial使いに転職するとき設定しておくべきMercurial拡張`: http://labs.timedia.co.jp/2011/03/mercurial-extensions-we-should-setup-for-gituser.html

用語
----
tip
  最新のリビジョンの事

defaultブランチ
  svnのtrunk、gitのmasterの事

multiple heads
  名前無しブランチが複数できている状態の事

ファイルの操作、multiple headsに関わる操作、ブランチの操作、リポジトリ間の操作、タグの操作 の履歴
-------------------------------------------------------------------------------------------------
`GraphlogExtension`_ の結果を貼り付けておく。hash値が異なるのはご愛敬で、、、

.. _`GraphlogExtension`: http://mercurial.selenic.com/wiki/GraphlogExtension

  ::

    @  changeset:   23:5d7a5fdc0b1a
    |  tag:         tip
    |  user:        Takumi IINO <takumi@timedia.co.jp>
    |  date:        Sun Mar 20 23:12:38 2011 +0900
    |  summary:     Added tag v0.0.1 for changeset 650e447b9fb1
    |
    o    changeset:   22:650e447b9fb1
    |\   tag:         v0.0.1
    | |  parent:      21:0c9819be6417
    | |  parent:      20:768d84182c71
    | |  user:        Takumi IINO <takumi@timedia.co.jp>
    | |  date:        Sun Mar 20 23:08:59 2011 +0900
    | |  summary:     merged
    | |
    | o  changeset:   21:0c9819be6417
    | |  parent:      19:e706d4fd6805
    | |  user:        Takumi IINO <takumi@timedia.co.jp>
    | |  date:        Sun Mar 20 23:06:36 2011 +0900
    | |  summary:     run hugs
    | |
    o |  changeset:   20:768d84182c71
    |\|  parent:      18:ad81adf49b42
    | |  parent:      19:e706d4fd6805
    | |  user:        Takumi IINO <takumi@timedia.co.jp>
    | |  date:        Sun Mar 20 22:51:00 2011 +0900
    | |  summary:     merged
    | |
    | o  changeset:   19:e706d4fd6805
    | |  parent:      17:7cd901509845
    | |  user:        Takumi IINO <takumi@timedia.co.jp>
    | |  date:        Sun Mar 20 22:39:57 2011 +0900
    | |  summary:     run perl
    | |
    o |  changeset:   18:ad81adf49b42
    |/   user:        Takumi IINO <takumi@timedia.co.jp>
    |    date:        Sun Mar 20 22:44:19 2011 +0900
    |    summary:     add haskell sample
    |
    o  changeset:   17:7cd901509845
    |  parent:      15:4e0ddd138f6b
    |  user:        Takumi IINO <takumi@timedia.co.jp>
    |  date:        Sun Mar 20 22:29:22 2011 +0900
    |  summary:     run python
    |
    | o  changeset:   16:f976730a0346
    | |  branch:      makefile_fix
    | |  parent:      14:3cb402ea1e44
    | |  user:        Takumi IINO <takumi@timedia.co.jp>
    | |  date:        Sun Mar 20 22:00:08 2011 +0900
    | |  summary:     finish.
    | |
    o |  changeset:   15:4e0ddd138f6b
    |\|  parent:      11:1597cc35cade
    | |  parent:      14:3cb402ea1e44
    | |  user:        Takumi IINO <takumi@timedia.co.jp>
    | |  date:        Sun Mar 20 21:58:05 2011 +0900
    | |  summary:     merge makefile_fix
    | |
    | o  changeset:   14:3cb402ea1e44
    | |  branch:      makefile_fix
    | |  user:        Takumi IINO <takumi@timedia.co.jp>
    | |  date:        Tue Mar 08 00:16:52 2011 +0900
    | |  summary:     run ruby
    | |
    | o  changeset:   13:84d4c7bf2648
    | |  branch:      makefile_fix
    | |  user:        Takumi IINO <takumi@timedia.co.jp>
    | |  date:        Tue Mar 08 00:11:47 2011 +0900
    | |  summary:     fix ... orz
    | |
    | o  changeset:   12:7293a6112d50
    |/   branch:      makefile_fix
    |    user:        Takumi IINO <takumi@timedia.co.jp>
    |    date:        Mon Mar 07 23:00:46 2011 +0900
    |    summary:     start makefile_fix branch
    |
    o    changeset:   11:1597cc35cade
    |\   parent:      10:47589976d454
    | |  parent:      9:821a2430ed2f
    | |  user:        Takumi IINO <takumi@timedia.co.jp>
    | |  date:        Mon Mar 07 21:52:53 2011 +0900
    | |  summary:     guileがelispを置き換えるなら考える
    | |
    | o  changeset:   10:47589976d454
    | |  parent:      8:48d139b4230f
    | |  user:        Takumi IINO <takumi@timedia.co.jp>
    | |  date:        Mon Mar 07 21:13:58 2011 +0900
    | |  summary:     run with guile
    | |
    o |  changeset:   9:821a2430ed2f
    |/   user:        Takumi IINO <takumi@timedia.co.jp>
    |    date:        Mon Mar 07 21:12:36 2011 +0900
    |    summary:     run with gosh
    |
    o    changeset:   8:48d139b4230f
    |\   parent:      7:89f3c6e6d974
    | |  parent:      5:bcb5dec879f9
    | |  user:        Takumi IINO <takumi@timedia.co.jp>
    | |  date:        Mon Mar 07 00:47:37 2011 +0900
    | |  summary:     Merged changes
    | |
    | o    changeset:   7:89f3c6e6d974
    | |\   parent:      6:6a0eac3064c9
    | | |  parent:      4:4b83e608a7d0
    | | |  user:        Takumi IINO <takumi@timedia.co.jp>
    | | |  date:        Mon Mar 07 00:45:07 2011 +0900
    | | |  summary:     Merged changes
    | | |
    | | o  changeset:   6:6a0eac3064c9
    | | |  parent:      3:980f8866917a
    | | |  user:        Takumi IINO <takumi@timedia.co.jp>
    | | |  date:        Mon Mar 07 00:34:33 2011 +0900
    | | |  summary:     add common lisp sample
    | | |
    o---+  changeset:   5:bcb5dec879f9
      | |  parent:      3:980f8866917a
     / /   user:        Takumi IINO <takumi@timedia.co.jp>
    | |    date:        Mon Mar 07 00:22:44 2011 +0900
    | |    summary:     add scheme sample
    | |
    o |  changeset:   4:4b83e608a7d0
    |\|  parent:      3:980f8866917a
    | |  parent:      2:46f0166b17d8
    | |  user:        Takumi IINO <takumi@timedia.co.jp>
    | |  date:        Mon Mar 07 00:17:50 2011 +0900
    | |  summary:     merge changeset: 2:46f0166b17d8
    | |
    | o  changeset:   3:980f8866917a
    | |  parent:      1:f491ca2a6114
    | |  user:        Takumi IINO <takumi@timedia.co.jp>
    | |  date:        Mon Mar 07 00:10:18 2011 +0900
    | |  summary:     add python sample
    | |
    o |  changeset:   2:46f0166b17d8
    |/   user:        Takumi IINO <takumi@timedia.co.jp>
    |    date:        Sun Mar 06 22:53:57 2011 +0900
    |    summary:     add perl sample
    |
    o  changeset:   1:f491ca2a6114
    |  user:        Takumi IINO <takumi@timedia.co.jp>
    |  date:        Sun Mar 06 22:43:47 2011 +0900
    |  summary:     camelize
    |
    o  changeset:   0:c0d1b673238b
       user:        Takumi IINO <takumi@timedia.co.jp>
       date:        Sun Mar 06 22:27:01 2011 +0900
       summary:     add hello.rb

参考文献
--------
- `初心者向けガイド`_
- `hgbook 日本語版`_
- `hgtip`_
- `BPMERCURIAL-WORKFLOW ドキュメント`_

.. _`初心者向けガイド`: http://mercurial.selenic.com/wiki/JapaneseBeginnersGuides
.. _`hgbook 日本語版`: http://foozy.bitbucket.org/hgbook-ja/index.ja.html
.. _`hgtip`: http://ja.hgtip.com/
.. _`BPMERCURIAL-WORKFLOW ドキュメント`: http://beproud.bitbucket.org/bpmercurial-workflow/ja/

.. rubric:: 脚注
.. [rebase] 無駄なマージばかり増えていくことに抵抗があれば `RebaseExtension`_ を調べてみるといい。
.. [fetch] pull merge commitの一連の流れを行う `FetchExtension`_ も存在する。

.. _`RebaseExtension`: http://mercurial.selenic.com/wiki/RebaseExtension
.. _`FetchExtension`: http://mercurial.selenic.com/wiki/RebaseExtension

