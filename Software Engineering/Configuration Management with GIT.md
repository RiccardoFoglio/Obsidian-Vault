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






