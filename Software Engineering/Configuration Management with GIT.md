![[Screenshot 2025-03-28 at 2.27.41 PM.png|400]]
![[Screenshot 2025-03-28 at 2.28.03 PM.png|400]]

Need Configuration Management to handle the software

- many people working on the same documents for a long period of time
- who can change what? who modified what and when? which is the last working version?

- Documents have dependencies which can be not formalized:
	- import class are formal
	- requirement documents, design documents etc... are non formal dependencies

![[Screenshot 2025-03-28 at 2.30.49 PM.png|400]]

- Config Item (CI) : unit under configuration management, can be composed of one or more files
- Configuration : set of CIs
- Repository : logical / physical place where CIs are

- versioning = capability of storing / rebuilding all past versions of a CI or configuration
- change control = capability of granting right of reading / writing a CI to a defined user

- Check-in / Check-Out = formal operations made by a user to notify the CMS that a CI is being accessed and modified
	- Checkout : notification that CIs is being accessed by a certain user
	- Checkin : notification that a a CIs is no longer accessed, and has been modified (commit)

![[Screenshot 2025-03-28 at 2.33.57 PM.png|400]]

2 main control models:
- **lock modify unlock** --> not currently used, obliges to serialize work, first user often forgets to unlock, but there will never be conflicts (because no concurrent modifications)
- **copy modify merge** --> many users can checkout in parallel and on copies, but have to deal with conflicts and merge operations

CM Choices:
- Not all documents need to become CI
	A CI has advantages (versioning access control etc...) but at a cost of an overhead (checkin and checkout)
- lock model or not?
- how often commit work?
- CMS tools
- CM manager role: defined or not

## Configuration Management System (CMS)

Software application capable of storing and versioning CIs, storing and versioning configurations, change control

CMS Functionalities:
- Revert CI back to previous version
- Revert configuration back to previous version
- Compare changes over time
- Control access (who can read / write what CIs)
- Track who modified what when

Documents --> Repo, CIs, configurations
Who can change what when --> change control
Who did modify what --> Tracking
Which was the last working version --> configurations, versioning, tagging

Taxonomy:
- **Local CMS** = only one machine, repo and workspace on the same machine (RCS)
- **Centralized CMS** = server contains repo, clients contain copy (CVS, Subversion, Perforce)
- **Distributed CMS** = server contains repo, each client mirrors the repo locally (Git, Mercurial, Bazaar)

Storage Model:
- Deltas : for CI the root version is stored, for next versions only the differences are stored:
	PRO: Less Space
	CON: Overhead to reconstruct version

- Full Copies : for a CI a full copy of each version is stored
	PRO: more space is used
	CON: Each version available in time zero

Configuration management models
- Differences : a commit by a user changes the version of a CI (only)
  ![[Screenshot 2025-03-28 at 3.25.37 PM.png|400]]
- Snapshot (ex git) : a commit by a user defines a new snapshot, including all CIs. A snapshot is made of all CIs in the project. If a CI didn't change, a link t the prev one is used.
  ![[Screenshot 2025-03-28 at 3.26.03 PM.png|400]]
# Intro to GIT

Started in 2005 to support Linux Development
Radically different from previous CMSs
Focus on:
- Speed
- Support for large projects, with thousand of branches
- Support for distributed non linear development model

GIT:
- Distributed CMS
- Snapshots Configuration --> all files together, not one by one
- Local operations --> no info is needed from other computers : speed
- Integrity --> check-summed before it's stored and is referred to by that checksum --> any change is recognized, no untracked change of any file or directory
- Additive --> information is added, never deleted

GitHub = online code hosting service built on top of git, mainly used by OSS projects, Commercial use is growing

GitLab = another online code hosting service built on top of git

How it works:

- Working Copy (WC)
- Local Repo
- Remote Repo
![[Screenshot 2025-03-28 at 3.47.34 PM.png]]
![[Screenshot 2025-03-28 at 3.48.02 PM.png|300]]

## Client Side

folder and file system, or working directory or working tree --> files and folders

WC or index or staging area --> files and folders to be committed but not yet versioned
- add : enters a file from file system into working copy
- rm : remove file from working copy

Local repo --> files and folders versioned
- Commit : enters all files from WC to local repo
- fetch, rebase : copies snapshot from local repo to WC, merge them

![[Screenshot 2025-03-28 at 4.24.50 PM.png|500]]

Commit:
Modifies the local repo
Atomic operation : either succeeds completely or fails completely
The integrity of the repo is assured
It's mandatory to provide a log message with the commit, to explain the changes made, it becomes part of the history of the repo

States:
- Untracked : in the working folder but not tracked by git yet
- Staged : in the WC (staged area) via the `add` command
- Committed : versioned, in the local repo via the `commit` command

It's possible to exclude files from version control using the `.gitignore` file so that such files and folders will not be staged

![[Screenshot 2025-03-28 at 4.42.49 PM.png|500]]

Snapshot:
Set all files (CIs) in a repo, in a certain version
Identified by a unique ID (commit hash) computed at commit time

Remote repo shared by all clients
- `push` uploads snapshot from local repo to remote repo (sync of repos)
- `pull` downloads snapshot from remote repo to local

Set up user
```
config –global user.name «my_name»
config –global user.email «my_email»
```

Set up repo
- local
```
init
```
- remote
```
remote add origin <link>
```

Work on local repo:
```
add / rm / mv
commit
checkout <hashcode>
fetch / rebase
log / status / diff
```

Work on remote repo: 
```
push / pull
```

### Common Scenarios

Project from scratch:
- git init
- work on files
- add files
- commit
- remote add
- push

Project already been created by others, sync locally, work on it, sync remotely
- Git clone
- Pull
- Work on files
- Add
- Commit
- Push

Modify a project, build fails, can't understand why. Retrieve previous version that did build
- git checkout \<commit id\>
- git checkout -b \<new branch\> \<commit id\>

## Branching

2 modes of development in a project: linear and branching
LINEAR:
![[Screenshot 2025-03-28 at 5.16.10 PM.png|400]]
If a feature needs to be deleted and keep the following, it's difficult to implement

BRANCHING:
![[Screenshot 2025-03-28 at 5.21.03 PM.png|400]]
one branch per feature, master remain unchanged, easy to delete a branch to delete a feature

```
// show all branches
git branch 

// create branch
git branch <new branch>

// create a new branch and switch to it
git checkout -b <new branch>
git switch -c <new branch>

// switch to an existing branch
git switch <existing branch>
git checkout <existing branch>

// delete branch
git branch -d <existing branch>
```

Git stores a commit object that contains:
- a pointer to the snapshot of the content you staged
- pointers to the commit or commits that directly came before this commit (its parent or parents)

Staging the files
- checksums each one
- stores that version of the file (blobs)
- adds that checksum to the staging area

A branch in GIT is simply a lightweight movable pointer to one of these commits
HEAD points to the current branch
Every commit HEAD moves forward automatically
HEAD can be moved in other ways too

![[Screenshot 2025-03-28 at 5.47.37 PM.png|400]]

`git branch testing`
![[Screenshot 2025-03-28 at 5.49.07 PM.png|400]]
Pointer to current local branch
![[Screenshot 2025-03-28 at 5.50.04 PM.png|400]]
switch to branch
`git checkout testing`
![[Screenshot 2025-03-28 at 5.51.09 PM.png|400]]
after another commit:
![[Screenshot 2025-03-28 at 5.52.53 PM.png|400]]

If u switch to master, HEAD and WC changes (files in `.git` folder change too)
Commit will diverge the history
![[Screenshot 2025-03-28 at 5.56.01 PM.png|400]]

### Merging
- no divergence
Initial situation
![[Screenshot 2025-03-28 at 5.58.57 PM.png|400]]
`git merge hotfix`
![[Screenshot 2025-03-28 at 5.59.58 PM.png|400]]
`git branch -d hotfix`
add a commit on iss53
![[Screenshot 2025-03-28 at 6.02.00 PM.png|400]]
`git checkout master`
`git merge iss53`
![[Screenshot 2025-03-28 at 6.02.56 PM.png|400]]

Final result:
![[Screenshot 2025-03-28 at 6.03.40 PM.png]]

- With divergence: instead of just moving the ranch pointer forward, git creates a new snapshot that results from this three-way merge and automatically creates a new commit that points to it. It's a merge commit and it's special because has more than one parent
	Git determines the best common ancestor to use for its merge base
	Conflicts are possible and have to be managed
	
REBASE is another way of merging : applies changes of one snapshot history to another

starting point:
![[Screenshot 2025-03-28 at 6.15.16 PM.png|500]]
`git checkout master`
`git merge experiment`
![[Screenshot 2025-03-28 at 6.16.11 PM.png|500]]
`git checkout experiment`
`git rebase master`
![[Screenshot 2025-03-28 at 6.20.50 PM.png|500]]
`git checkout master`
`git merge experiment`
![[Screenshot 2025-03-28 at 6.24.11 PM.png|500]]

Result
![[Screenshot 2025-03-28 at 6.24.32 PM.png]]
careful, rebase rewrites the repo history!

Rebase:
it's also possible to organize / edit your commits
some useful casesL
- commit message is wrong, or doesn't make sense
- order of commits is not nice regarding to git history
- there are more than one commit which make similar changes (or even the same thing)
- a commit grouped a lot of different code and it makes sense divide it in smaller commits

`git rebase -i HEAD~4` 


Rebase vs Merge: main difference are
- merge combines the commit history of two branches, while rebase creates a linear commit history
- merge creates a new merge commit, while rebase rewrites the commit history of the feature branch
- Merge is better suited for integrating changes from different branches, while rebase is better suited for keeping a cleaner commit history

Merge is generally safer than rebase because it preserves the original commit history and creates a new merge commit, which makes it easier to undo the merge if necessary

Graph structure: every merge commit increases the connectivity of the commit graph by one. A rebase by contrast doesn't change the connectivity and leads to a more linear history

cherrypick : allows to apply a specific commit from one branch to another without merging the entire branch
`git cherrypick <commit-hash>`

### Collaborative Models:

- Shared repo : individuals and teams are explicitly designated as contributors with read, write or admin access
- fork and pull : anyone can contribute, fork is a copy of a project under a dev's personal account. every dev has full control of their fork and is free to implement new stuff. Work completed can be brought to the original project via a pull request

pull (or merge) request let the dev tell others about changes they pushed to the branch in a repo
Once a request is opened, the dev can discuss and review the potential changes with collaborators and add follow-up commits before changes are merged into the base branch

# Git Flow

Branching model designed to structure and streamline software development

separates stable code from ongoing development, reducing risks
allows multiple features to be developed in parallel or isolated branches increasing efficiency
ensures quality and stability by integrating testing before release
facilitates collaboration using merge requests for code reviews
keeps track of releases using tags and structured versioning

Beneficial for projects with periodic releases

