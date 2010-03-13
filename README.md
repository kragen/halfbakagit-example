This was an experiment to see if git-rebase would blow up merges in a particular funny case.

You can read about the background in <http://lists.canonical.org/pipermail/kragen-tol/2010-March/000910.html>.

Here's how I set it up:

    : kragen@inexorable:~ ; cd devel
    : kragen@inexorable:~/devel ; mkdir halfbakagit
    : kragen@inexorable:~/devel ; cd halfbakagit
    : kragen@inexorable:~/devel/halfbakagit ; git init
    Initialized empty Git repository in /home/kragen/devel/halfbakagit/.git/
    : kragen@inexorable:~/devel/halfbakagit ; cat > post1
    Upon the eaves of the new weasels, I spied a telltale drop of saliva.
    : kragen@inexorable:~/devel/halfbakagit ; git add post1
    : kragen@inexorable:~/devel/halfbakagit ; git commit -m '2010 post'
    Created initial commit 5929e5f: 2010 post
     1 files changed, 1 insertions(+), 0 deletions(-)
     create mode 100644 post1
    : kragen@inexorable:~/devel/halfbakagit ; echo "They sat. Waited. Still longer. No water; no food; no hope." > post2
    : kragen@inexorable:~/devel/halfbakagit ; git add post2
    : kragen@inexorable:~/devel/halfbakagit ; git commit -m '2011 post'
    Created commit d576ce6: 2011 post
     1 files changed, 1 insertions(+), 0 deletions(-)
     create mode 100644 post2
    : kragen@inexorable:~/devel/halfbakagit ; cd ..
    : kragen@inexorable:~/devel ; git clone halfbakagit halfbakagit.foocorp
    Initialized empty Git repository in /home/kragen/devel/halfbakagit.foocorp/.git/
    : kragen@inexorable:~/devel ; cd halfbakagit.foocorp
    : kragen@inexorable:~/devel/halfbakagit.foocorp ; echo "Fred felt he was a ramp, a stairway, a clothesline for others to slide down." > fpost1
    : kragen@inexorable:~/devel/halfbakagit.foocorp ; git add fpost1
    : kragen@inexorable:~/devel/halfbakagit.foocorp ; git commit -m 'fake 2012 post'
    Created commit 53e847f: fake 2012 post
     1 files changed, 1 insertions(+), 0 deletions(-)
     create mode 100644 fpost1
    : kragen@inexorable:~/devel/halfbakagit.foocorp ; cd ../halfbakagit
    : kragen@inexorable:~/devel/halfbakagit ; echo "The verdant forest exploded with arthropod life: vicious, heartless, beautiful, and unbelievably complex. Myra rubbed her hands together; to her, this was paradise." > post3
    : kragen@inexorable:~/devel/halfbakagit ; git add post3
    : kragen@inexorable:~/devel/halfbakagit ; git commit -m '2015 post'
    Created commit 3dcdc9e: 2015 post
     1 files changed, 1 insertions(+), 0 deletions(-)
     create mode 100644 post3
    : kragen@inexorable:~/devel/halfbakagit ; echo "While we open our glasses to one another's drinks, we never open our mouths to one another's pre-chewed food. Why is that?" > post4
    : kragen@inexorable:~/devel/halfbakagit ; git add post4
    : kragen@inexorable:~/devel/halfbakagit ; git commit -m '2017 post'
    Created commit 4946350: 2017 post
     1 files changed, 1 insertions(+), 0 deletions(-)
     create mode 100644 post4
    : kragen@inexorable:~/devel/halfbakagit ; echo "The ancient citrus grove felt alive, alive in a way it had never felt before, as Dan Brown's prose burned in the bonfire in its center." > post5
    : kragen@inexorable:~/devel/halfbakagit ; git add post5
    : kragen@inexorable:~/devel/halfbakagit ; git commit -m '2024 post'
    Created commit 0c370ea: 2024 post
     1 files changed, 1 insertions(+), 0 deletions(-)
     create mode 100644 post5
    : kragen@inexorable:~/devel/halfbakagit ; cd ../halfbakagit.foocorp/
    : kragen@inexorable:~/devel/halfbakagit.foocorp ; git tag our-fake-post HEAD
    : kragen@inexorable:~/devel/halfbakagit.foocorp ; git branch our-branch HEAD
    : kragen@inexorable:~/devel/halfbakagit.foocorp ; gitk&
    [1] 16459
    : kragen@inexorable:~/devel/halfbakagit.foocorp ; git fetch origin
    remote: Counting objects: 10, done.
    remote: Compressing objects: 100% (9/9), done.
    remote: Total 9 (delta 2), reused 0 (delta 0)
    Unpacking objects: 100% (9/9), done.
    From /home/kragen/devel/halfbakagit/
       d576ce6..0c370ea  master     -> origin/master
    : kragen@inexorable:~/devel/halfbakagit.foocorp ; git checkout origin/master
    Note: moving to "origin/master" which isn't a local branch
    If you want to create a new branch from this checkout, you may do so
    (now or later) by using -b with the checkout command again. Example:
      git checkout -b <new_branch_name>
    HEAD is now at 0c370ea... 2024 post
    : kragen@inexorable:~/devel/halfbakagit.foocorp ; git checkout -b our-fake-branch
    Switched to a new branch "our-fake-branch"
    : kragen@inexorable:~/devel/halfbakagit.foocorp ; git rebase our-branch
    First, rewinding head to replay your work on top of it...
    Applying 2015 post
    Applying 2017 post
    Applying 2024 post
    : kragen@inexorable:~/devel/halfbakagit.foocorp ; cd ..
    : kragen@inexorable:~/devel ; git clone halfbakagit halfbakagit.merge
    Initialized empty Git repository in /home/kragen/devel/halfbakagit.merge/.git/
    : kragen@inexorable:~/devel ; cd !$
    cd halfbakagit.merge
    : kragen@inexorable:~/devel/halfbakagit.merge ; git fetch ../halfbakagit.foocorp our-fake-branch
    remote: Counting objects: 13, done.
    remote: Compressing objects: 100% (12/12), done.
    remote: Total 12remote:  (delta 3), reused 0 (delta 0)
    Unpacking objects: 100% (12/12), done.
    From ../halfbakagit.foocorp
     * branch            our-fake-branch -> FETCH_HEAD
    : kragen@inexorable:~/devel/halfbakagit.merge ; git merge FETCH_HEAD
    Merge made by recursive.
     fpost1 |    1 +
     1 files changed, 1 insertions(+), 0 deletions(-)
     create mode 100644 fpost1
    : kragen@inexorable:~/devel/halfbakagit.merge ; gitk &
    [2] 17681
    : kragen@inexorable:~/devel/halfbakagit.merge ; more * | cat
    ::::::::::::::
    fpost1
    ::::::::::::::
    Fred felt he was a ramp, a stairway, a clothesline for others to slide down.
    ::::::::::::::
    post1
    ::::::::::::::
    Upon the eaves of the new weasels, I spied a telltale drop of saliva.
    ::::::::::::::
    post2
    ::::::::::::::
    They sat. Waited. Still longer. No water; no food; no hope.
    ::::::::::::::
    post3
    ::::::::::::::
    The verdant forest exploded with arthropod life: vicious, heartless, beautiful, and unbelievably complex. Myra rubbed her hands together; to her, this was paradise.
    ::::::::::::::
    post4
    ::::::::::::::
    While we open our glasses to one another's drinks, we never open our mouths to one another's pre-chewed food. Why is that?
    ::::::::::::::
    post5
    ::::::::::::::
    The ancient citrus grove felt alive, alive in a way it had never felt before, as Dan Brown's prose burned in the bonfire in its center.
    : kragen@inexorable:~/devel/halfbakagit.merge ; git log
    commit 4f9d76c457c38c40f2f58500a5ef9dda401fd1a0
    Merge: 0c370ea... f98d29f...
    Author: kragen <kragen@inexorable.(none)>
    Date:   Sat Mar 13 00:19:29 2010 -0300

        Merge branch 'our-fake-branch' of ../halfbakagit.foocorp

    commit f98d29f7b312b6cba8237a4de193f7b8e21e457e
    Author: kragen <kragen@inexorable.(none)>
    Date:   Sat Mar 13 00:14:14 2010 -0300

        2024 post

    commit a2a3314936d52ed4a1f8dcd0f87d65ef115e0aab
    Author: kragen <kragen@inexorable.(none)>
    Date:   Sat Mar 13 00:13:07 2010 -0300

        2017 post

    commit e77d6a10f1a8c291c99796bd8103aa9f985679ad
    Author: kragen <kragen@inexorable.(none)>
    Date:   Sat Mar 13 00:12:06 2010 -0300

        2015 post

    commit 0c370ea248efd228eabe264d9c1b625377f540d9
    Author: kragen <kragen@inexorable.(none)>
    Date:   Sat Mar 13 00:14:14 2010 -0300

        2024 post

    commit 49463501e7783f31113740d45d6d29a033b78e28
    Author: kragen <kragen@inexorable.(none)>
    Date:   Sat Mar 13 00:13:07 2010 -0300

        2017 post

    commit 3dcdc9eb353e29e4cfd4de3810756d59be0ae1b2
    Author: kragen <kragen@inexorable.(none)>
    Date:   Sat Mar 13 00:12:06 2010 -0300

        2015 post

    commit 53e847f9579c037182e58d6f9fb91eef0046a29b
    Author: kragen <kragen@inexorable.(none)>
    Date:   Sat Mar 13 00:10:41 2010 -0300

        fake 2012 post

    commit d576ce626e39c6bda90134bf928556a30da79724
    Author: kragen <kragen@inexorable.(none)>
    Date:   Sat Mar 13 00:09:17 2010 -0300

        2011 post

    commit 5929e5fbd27755abaa982b2b4fe52c1a20a70d12
    Author: kragen <kragen@inexorable.(none)>
    Date:   Sat Mar 13 00:08:31 2010 -0300

        2010 post
    63501e7783f31113740d45d6d29a033b78e28
    Author: kragen <kragen@inexorable.(none)>
    Date:   Sat Mar 13 00:13:07 2010 -0300

        2017 post

    commit 3dcdc9eb353e29e4cfd4de3810756d59be0ae1b2
    Author: kragen <kragen@inexorable.(none)>
    Date:   Sat Mar 13 00:12:06 2010 -0300

        2015 post

    commit 53e847f9579c037182e58d6f9fb91eef0046a29b
    Author: kragen <kragen@inexorable.(none)>
    Date:   Sat Mar 13 00:10:41 2010 -0300

        fake 2012 post

    commit d576ce626e39c6bda90134bf928556a30da79724
    Author: kragen <kragen@inexorable.(none)>
    Date:   Sat Mar 13 00:09:17 2010 -0300

        2011 post

    commit 5929e5fbd27755abaa982b2b4fe52c1a20a70d12
    Author: kragen <kragen@inexorable.(none)>
    Date:   Sat Mar 13 00:08:31 2010 -0300

        2010 post
    : kragen@inexorable:~/devel/halfbakagit.merge ; cat > README.md
