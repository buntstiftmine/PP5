# PP5

## Goal

In this exercise you will:

* Use Git **locally** on your own machine: create branches, make commits, and merge them back.
* Set up and interact with a **bare** Git repository on an SSH‐accessible server (e.g. **vorlesungsserver**, or any machine you have SSH access to).
* Push and pull to/from **GitHub** and the **THGA GitLab** server.
* Practice **forking** an existing repo, making changes, and submitting a **Pull Request** (on GitHub) or **Merge Request** (on GitLab).

**Important:** Start a stopwatch when you begin and work uninterruptedly for **90 minutes**. Once time is up, stop immediately and document exactly where you had to pause.

---

## Workflow

1. **Fork** this repository
2. **Modify & commit** your solution
3. **Submit your link for Review**

---

## Prerequisites

* Several starter repos are available here:
  [https://github.com/orgs/STEMgraph/repositories?q=Git%3A](https://github.com/orgs/STEMgraph/repositories?q=Git%3A)
* Ensure you have **SSH access** to a remote server (e.g., “vorlesungsserver”).
* Throughout, consult Git’s built-in man-pages (`git help <command>`) for explanations and options.

---

## Tasks

### Task 1: Local Git – Branching & Merging

1. Create a new directory in your home directory and initialize a repository in it. 
2. In one of your cloned repos, initialize a new branch called `feature-1`.
3. On `feature-1`, create a file `feature.txt` containing a short description of what you’re doing.
4. Commit your changes, then switch back to `master` (or `main`), and merge `feature-1` into your `master`.
5. Resolve any merge conflicts (if they arise) by hand, then commit the merge. 

**Your Commands & Output**

```bash
mine@MinesOmen:/home$ cd /home/mine
mine@MinesOmen:~$ mkdir newdir
mine@MinesOmen:~$ cd newdir
mine@MinesOmen:~/newdir$ ls
mine@MinesOmen:~/newdir$ git init
Initialized empty Git repository in /home/mine/newdir/.git/
mine@MinesOmen:~/newdir$ git status
On branch master

No commits yet

nothing to commit (create/copy files and use "git add" to track)
mine@MinesOmen:~/newdir$ git log
fatal: your current branch 'master' does not have any commits yet
mine@MinesOmen:~/newdir$ git checkout -b feature-1
Switched to a new branch 'feature-1'
mine@MinesOmen:~/newdir$ touch feature.txt
mine@MinesOmen:~/newdir$ vim feature.txt
mine@MinesOmen:~/newdir$ cat feature.txt
I created this file to write a description of what I am doing.
mine@MinesOmen:~/newdir$ git add feature.txt
mine@MinesOmen:~/newdir$ git status
On branch feature-1

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   feature.txt

mine@MinesOmen:~/newdir$ git commit -m "featurecommit"
Author identity unknown

*** Please tell me who you are.

Run

  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"

to set your account's default identity.
Omit --global to set the identity only in this repository.

fatal: empty ident name (for <mine@MinesOmen.>) not allowed
mine@MinesOmen:~/newdir$ git config --global user.email "marie.schmalz@stud.thga.de"
mine@MinesOmen:~/newdir$ git config --global user.name "Mine"
mine@MinesOmen:~/newdir$ git commit -m "firstcommit"
[feature-1 (root-commit) 992ae39] firstcommit
 1 file changed, 1 insertion(+)
 create mode 100644 feature.txt



???
mine@MinesOmen:~/newdir$ git checkout master
error: pathspec 'master' did not match any file(s) known to git
mine@MinesOmen:~/newdir$ git checkout main
error: pathspec 'main' did not match any file(s) known to git
mine@MinesOmen:~/newdir$ git checkout master
error: pathspec 'master' did not match any file(s) known to git
mine@MinesOmen:~/newdir$ git checkout -b master
Switched to a new branch 'master'
mine@MinesOmen:~/newdir$ git checkout feature-1
Switched to branch 'feature-1'
mine@MinesOmen:~/newdir$ git checkout master
Switched to branch 'master'
mine@MinesOmen:~/newdir$ git merge fuature-1 master
merge: fuature-1 - not something we can merge
mine@MinesOmen:~/newdir$ git merge feature-1 master
Already up to date.
mine@MinesOmen:~/newdir$ git log
commit dc437bd40af3448821eff888999b2d4d78f511b7 (HEAD -> master, feature-1)
Author: Mine <marie.schmalz@stud.thga.de>
Date:   Sun May 18 17:37:12 2025 +0200

    featurecommit

```

---

### Task 2: Bare Repository on an SSH Server

1. On your SSH server (e.g., “vorlesungsserver”), create a **bare** repo at `~/repos/myproject.git`:

   ```bash
   ssh youruser@vorlesungsserver \
     "mkdir -p ~/repos/myproject.git && cd ~/repos/myproject.git && git init --bare"
   ```
2. On your local machine, add it as a remote named `origin-ssh`:

   ```bash
   git remote add origin-ssh youruser@vorlesungsserver:~/repos/myproject.git
   ```
3. Push your `master` branch to `origin-ssh`, then clone it into a fresh directory to verify.

**Your Commands & Output**

```bash
mine@MinesOmen:~$ ssh testserver
Enter passphrase for key '/home/mine/testkey':
Linux Uranus 5.10.0-0.deb10.16-amd64 #1 SMP Debian 5.10.127-2~bpo10+1 (2022-07-28) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Tue May 13 20:44:21 2025 from 192.168.2.203
root@Uranus:~# mkdir mine
root@Uranus:~# cd mine
root@Uranus:~/mine# ls
root@Uranus:~/mine# mkdir pp5
root@Uranus:~/mine# cd pp5
root@Uranus:~/mine/pp5# mkdir -p ~/mine/pp5/repos/myproject.git && cd ~/mine/pp5/repos/myproject.git && git init --bare
Leeres Git-Repository in /root/mine/pp5/repos/myproject.git/ initialisiert
root@Uranus:~/repos/myproject.git#
root@Uranus:~/repos/myproject.git# exit
Abgemeldet
Connection to 192.168.2.110 closed.
mine@MinesOmen:~$ cd newdir
mine@MinesOmen:~/newdir$ git push -u origin-ssh master
Enter passphrase for key '/home/mine/testkey':
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Delta compression using up to 24 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 273 bytes | 273.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To testserver:~/mine/pp5/repos/myproject.git
 * [new branch]      master -> master
branch 'master' set up to track 'origin-ssh/master'.
mine@MinesOmen:~/newdir$ cd ~
mine@MinesOmen:~$ mkdir newdirclone
mine@MinesOmen:~$ cd newdirclone
mine@MinesOmen:~/newdirclone$ git clone testserver:~/mine/pp5/repos/myproject.git
Cloning into 'myproject'...
Enter passphrase for key '/home/mine/testkey':
remote: Objekte aufzählen: 3, Fertig.
remote: Zähle Objekte: 100% (3/3), Fertig.
remote: Komprimiere Objekte: 100% (2/2), Fertig.
remote: Gesamt 3 (Delta 0), Wiederverwendet 0 (Delta 0)
Receiving objects: 100% (3/3), done.
mine@MinesOmen:~/newdirclone$ ls
myproject
mine@MinesOmen:~/newdirclone$ cd myproject
mine@MinesOmen:~/newdirclone/myproject$ ls
feature.txt
mine@MinesOmen:~/newdirclone/myproject$ git log
commit dc437bd40af3448821eff888999b2d4d78f511b7 (HEAD -> master, origin/master, origin/HEAD)
Author: Mine <marie.schmalz@stud.thga.de>
Date:   Sun May 18 17:37:12 2025 +0200

    featurecommit
```

---

### Task 3: GitHub & THGA GitLab

1. On [GitHub](github.com), create a new empty repo under your account named `myproject-gh`.
2. On [THGA GitLab](gitlab.thga.de), create a new project named `myproject-gl`.
3. In your local repository, add these as remotes:

   ```bash
   git remote add github  git@github.com:YOUR_USERNAME/myproject-gh.git
   git remote add gitlab  git@gitlab.thga.de:YOUR_USERNAME/myproject-gl.git
   ```
4. Push your `master` branch to both `github` and `gitlab` remotes.

> Hint: You are just adding two more bare-remotes to your already existing repository!

**Your Commands & Output**

```bash
# Paste here the remote‐adding & push outputs
```

---

### Task 4: Fork, Modify, and Pull/Merge Request

1. On GitHub, **fork** one of the repos from the STEMgraph org.
2. Clone your fork locally, create a branch `pp5-changes`, and make a small change (e.g., update the README).
3. Commit and push `pp5-changes` to **your** fork, then open a **Pull Request** against the original repo.
4. Repeat on THGA GitLab: fork another students repository, clone, branch, change, push, and open a **Merge Request**. If your peers opened a merge request to your repository, review and merge or decline it!

**Your PR/MR Links & Descriptions**

* GitHub PR: *paste URL and a one-sentence summary*
* GitLab MR: *paste URL and a one-sentence summary*

---

**Remember:** Stop working after **90 minutes** and record where you stopped!
