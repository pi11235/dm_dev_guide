############################################################
TSSW Development Workflow with Git, GitHub, JIRA and Jenkins
############################################################

This page describes our procedures for collaborating on LSST TSSW software and documentation with `Git <http://git-scm.org>`_, `GitHub <https://github.com>`_ and JIRA_:

1. :ref:`Configuring Git for TSSW development <git-setup>`.
2. :ref:`Using JIRA for agile development <workflow-jira>`.
3. :ref:`TSSW GitHub organizations <github-orgs>`.
4. :ref:`Policies for naming and using Git branches <git-branching>`.
5. :ref:`Preparing code for review <review-preparation>`.
6. :ref:`Reviewing and merging code <workflow-code-review>`.

In appendices, we suggest some *best practices* for maximizing the usefulness of our Git development history:

- :ref:`Commit organization best practices <git-commit-organization-best-practices>`.
- :ref:`Commit message best practices <git-commit-message-best-practices>`.

Other related pages:

- :doc:`/git/setup`.
- :doc:`/git/git-lfs`.
- :doc:`jira-tips`.

.. _git-setup:

Git & GitHub Setup
==================

You need to install Git version 1.8.2, or later to work with our repositories.

.. and the :ref:`Git LFS client <git-lfs-install>` to work with our data repositories.

.. IS THIS TRUE FOR TSSW ?

.. 
  Follow these steps to configure your Git environment for TSSW work:
  1. :ref:`Install Git LFS <git-lfs-install>` with authenticated access.
  2. :ref:`Set Git and GitHub to use your institution-hosted email address <git-setup-institutional-email>`.
  3. :ref:`Set Git to use 'plain' pushes <git-setup-plain-pushes>`.

     *See also:* :doc:`/git/setup`.

.. _workflow-jira:

Agile development with JIRA
===========================

We use JIRA_ to plan, coordinate and report our work.
Your Manager is the best resource for help with JIRA within your local group.
The prioritization of tasks for a given subsystem (e.g. M1M3) is to be decided by the product owner for that system, whereas the balance of tasks between subsystems for a given developer is decided by the TSSW Manager.
This section provides a high-level orientation for everyday TSSW development work.

*See also:* :doc:`jira-tips`.

.. _workflow-jira-concepts:

Agile concepts
--------------

Issue
   Issues are the fundamental units of work/planning information in JIRA.
Story Points
   Story points are how we estimate and account for time and effort.
   One story point is an idealized full day of uninterrupted work by a competent developer.
Velocity
   No developer works a 5 story point week.
   Communication overhead, review work, and other activities will invariably eat into your day.
   *Velocity* is the fraction of a story point that you can reasonably achieve in a day.
   We aim for a velocity in TSSW of 0.8, so that you nominally accomplish 0.8 story points in a day.
Sprint
   The sprint is a 2-week period where issues are worked on and tasks are accomplished.
   Prior to the beginning of each sprint, each developer is to work with the TSSW Manager
   and associated product owners to determine which tasks will be worked on during the sprint.
   Each developer is to load 8 story points in their sprint.
Epic
   Epics are a special type of issue, created by the TSSW Manager, that guide your work over longer term cycles
   At the start of each cycle, your T/CAM will create an epic (or several) and allocate *story points* to that epic.
   You don't work directly on an epic; rather you work on *tasks* (below) that cumulatively accomplish the epic.

.. _workflow-jira-issues:

Tickets
-------

All development work is done on these three types of **JIRA issues** that are generically referred to as **tickets**:

Task
   Tasks are for work that accomplish your main goals for a given sprint.
   Tasks are part of regular epics and stored in the Backlog. These tasks are then
   pulled into the sprint before the start of each cycle.
Bug
   A ticket of type bug describes “emergent” work: it was not planned at the start of a development cycle,
   but rather is a response to an unexpected problem report.

..   Bugs are associated with special epics designated for addressing emergent work.
.. Improvement
   An improvement is essentially a feature request.
   Like a *bug*, an improvement is emergent, and hence belongs in a special epic.
   Unlike a bug, an improvement adds new functionality.


As a developer, you can create tickets to work on.
You can also create bug or improvement tickets and assign them to others (ideally with some consultation).
All code that is to be developed and merged into the develop and master branches *require* a ticket.

.. _workflow-jira-ticket-creation:

Creating a ticket
-----------------

You can create a ticket from the `JIRA web app <https://jira.lsstcorp.org>`_ toolbar using the **Create** button.
For more general information, you can consult `Atlassian's docs for JIRA <https://confluence.atlassian.com/jirasoftwarecloud/jira-software-documentation-764477791.html>`_ and `JIRA Agile <https://confluence.atlassian.com/agile067>`_.

JIRA allows a myriad of metadata to be specified when creating a ticket.
At a minimum, you should specify:

Project
   For normal work, this should be set to **Telescope and Site Software**.
   It may occasionally be appropriate to use another project; for example,
   when requesting work from another LSST subsystem.
Issue Type
   If the work is associated with an epic, the issue type is a 'Task.'
   For emergent work, 'Bug' or 'Task' can be used (see above for semantics).
Summary
   This is the ticket's title and should be written to help colleagues browsing JIRA dashboards.
Description
   The description should provide a clear description of the deliverable that can serve as a definition of 'Done.'
   This will prevent scope creep in your implementation and the code review.
   For tasks, you can outline your implementation design in this field.
   For bug reports, include any information needed to diagnose and reproduce the issue.
   Feel free to use `Atlassian markup syntax <https://jira.lsstcorp.org/secure/WikiRendererHelpAction.jspa?section=texteffects>`_.

In addition, you may be able to provide some or all of the following. While, in general, it's helpful to provide as much information as you can, don't worry about leaving some fields blank: the TSSW Manager (or scrum master) will ensure the
work gets picked up and assigned to the right place, and empty metadata is better than bad medadata.

Components
   You should choose from the pre-populated list of components to specify what part of the TSSW system the ticket relates to.
   If in doubt, ask your TSSW Manager.
Assignee
   Typically you will assign yourself (or your Manager or product owner will assign you) to a ticket.
   You can also assign tickets to others.
   If you are uncertain about who the assignee should be you can allow the ticket to be automatically assigned.
Story Points
   Use this field, at ticket creation time, to **estimate** the amount of effort involved to accomplish the work.
   Keep in mind how *velocity* (see above) converts story points into real-world days.
Labels
   *NOT SURE HOW WE USE LABELS IN TSSW*
   Think of labels as tags that you can use to sort your personal work.
   Unlike the Component and Epic fields, you are free to create and use labels in any way you see fit, but you should also refer to this list of :ref:`common labels <jira-labels>`.
Linked Issues
   You can express relationships between JIRA issues with this field.
   You can also express dependencies to other work using a 'is Blocked by' relationship.
Epic Link
   If the ticket is a task, you must specify what epic it belongs to with this field.
   By definition, bug tickets are not associated with an epic.

.. _workflow-jira-ticket-status:

Ticket status
-------------

Tickets are created with a status of **Todo.**

Once a ticket is being actively worked on you can upgrade the ticket's status to **In Progress.**

It's also possible that you may decide not to implement a ticket after all.
In that case, change the ticket's status to **Won't Fix.**

If you discover that a ticket duplicates another one, you can retire the duplicate ticket by marking it as **Invalid.**
Name the duplicate ticket in the status change comment field.

.. _github-orgs:

TSSW GitHub Organizations
=========================

TBR


Personal GitHub repositories
----------------------------

Use personal repositories for side projects done after hours or on "science time."
Work by TSSW staff that is delivered to LSST in ticketed work **can not** be developed in personal GitHub repositories.

.. Community contributors can of course use personal repositories (and forks of LSST repositories) to make contributions to LSST.

.. _git-branching:

TSSW Git Branching Policy
=========================


It is essential that TSSW developers adhere to the following naming conventions for branches.

See `RFC-21 <https://jira.lsstcorp.org/browse/RFC-21>`_ for discussion. *IS THIS APPLICABLE?*

.. _git-branch-integration:

The master branch
-----------------

``master`` is the branch for our repositories which is always stable and deployable.
In some circumstances, a ``release`` integration branch may be used by the release manager. **I DONT THINK WE USE THIS?**

Documentation edits and additions are the only scenarios where working directly on ``master`` and by-passing the code review process is permitted.
In most cases, documentation writing benefits from peer editing (code review) and *can* be done on a ticket branch.

Development is not done directly on the ``master`` branch, but instead on *ticket branches*. These tickets are then merged into the ``develop`` branch after a unit testing and a code review.

Merging to master is performed when decided by product owner, either based on a time window or a significant increase in functionality (e.g. one to several features have been included). The process of merging to master is managed by the quality assurance person, or when unavailable, the TSSW Manager. **THIS NEEDS BETTER DEFINITION AS ITS A SMPF**

Upon merging to master, a version is tagged and released.

The Git history of ``master`` **must never be re-written** with force pushes.

.. _git-branch-develop:

The develop branch
------------------

``develop`` is the main integration branch for our repositories. This is the branch used for cross-repository continuous integration activities.
Development is not done directly on the ``develop`` branch, but instead on *ticket branches*. These tickets are then merged into the ``develop`` branch after undergoing unit testing and a subsequent code review.

The Git history of ``develop`` **must never be re-written** with force pushes.  **IS THIS STILL TRUE??**


.. _git-branch-user:

User branches
-------------

You can do experimental, proof-of-concept work in 'user branches.'

These branches are named

.. code-block:: text

   u/{{username}}/{{topic}}

User branches can be pushed to GitHub to enable collaboration and communication.
Before offering unsolicited code review on your colleagues' user branches, remember that the work is intended to be an early prototype.

Developers can feel free to rebase and force push work to their personal user branches.

A user branch *cannot* be merged into master; it must be converted into a *ticket branch* first.

.. _git-branch-ticket:

Ticket branches
---------------

Ticket branches are associated with a JIRA ticket.
Only ticket branches can be merged into ``develop``.
(In other words, developing on a ticket branch is the only way to record earned value for code development.)

If the JIRA ticket is named ``TSS-NNNN``, then the ticket branch will be named

.. code-block:: text

   tickets/TSS-NNNN

A ticket branch can be made by branching off the develop branch.
.. This is a great way to formalize and shape experimental work into an LSST software contribution.

When code on a ticket branch is ready for review and merging, follow the :ref:`code review process documentation <workflow-code-review>`.


.. _review-preparation:

Review Preparation
==================

When development on your ticket branch is complete, we use a standard process for reviewing and merging your work.
This section describes how to prepare your work for review.

.. _workflow-pushing:

Pushing code
------------

We recommend that you organize commits, improve commit messages, and ensure that your work is made against the latest commits on ``develop`` with an `interactive rebase <https://help.github.com/articles/about-git-rebase/>`_. Your code must also have gone through :ref:`an appropriate level of testing <workflow-testing>`.

A common pattern is:

.. code-block:: bash

   CHECK THIS!!!
   git checkout develop
   git pull
   git checkout tickets/TSS-NNNN
   git rebase -i develop
   # interactive rebase
   git push --force

.. _workflow-testing:

Testing at the Branch Level
----------------------------

All software branches must go through an appropriate level of testing prior to making the pull request to merge to develop. At a minimum, user tests must be run manually by the developer, however, whenever possible, the Jenkins CI framework should be utilized. Part of the development process is the creation of tests to verify functionality of the branch. Examination of these tests is part of the review process.

**DESCRIBE HOW TO USE JENKINS TO DO THIS HERE**

**STANDARD PROCEDURE SHOULD BE FOR THE DEVELOPER TO RUN THE TESTS AND POINT THE REVIEWER TO THE RESULTS RATHER THAN HAVE THE REVIEWER BUILD THEM**

.. Start a :doc:`stack-os-matrix Jenkins job </stack/jenkins-stack-os-matrix>` to run the Stack's tests with your ticket branch work.

.. To learn more about DM's Jenkins continuous integration service, see :doc:`/jenkins/getting-started`.
.. Then follow the steps listed in :doc:`/stack/jenkins-stack-os-matrix` to run the tests.

.. Ensure that you **do not** skip the demo before submitting a pull request.
.. Otherwise, your testing may be incomplete.

.. _workflow-pr:

Make a pull request
------------------------------

On GitHub, `create a pull request <https://help.github.com/articles/creating-a-pull-request/>`_ for your ticket branch.

The pull request's name should be formatted as

.. code-block:: text

   TSS-NNNN: {{JIRA Ticket Title}}

This helps you and other developers find the right pull request when browsing repositories on GitHub.

The pull request's description shouldn't be exhaustive; only include information that will help frame the review.
Background information should already be in the JIRA ticket description, commit messages, and code documentation.

.. _workflow-code-review:

TSSW Code Review and Merging Process
====================================

.. _workflow-review-purpose:

The Scope and Purpose of Code Reviews
-------------------------------------

We review work before it is merged to ensure that code is maintainable and usable by someone other than the author.

- Is the code well commented, structured for clarity, and consistent with TSSW's code style?
- Is there adequate unit testing coverage for the code?
- Is the documentation augmented or updated to be consistent with the code changes?
- Are the Git commits well organized and well annotated to help future developers understand the code development?

.. well- hyphenation? no http://english.stackexchange.com/a/65632

Code reviews should also address whether the code fulfills design and performance requirements.

Ideally, the code review *should not be a design review.*
Before serious coding effort is committed to a ticket, the developer should either undertake an informal design review while creating the JIRA story, or more formally use the :abbr:`RFC (Request for Comment)` and :abbr:`RFD (Request for Discussion)` processes (see :doc:`/processes/decision_process`) for key design decisions.

.. TODO: link to RFC/RFC process doc

.. _workflow-review-assign:

Assigning a Reviewer or Reviewers
---------------------------------

On your ticket's JIRA page, use the **Workflow** button to switch the ticket's state to **In Review**.
JIRA will ask you to assign reviewers.

Depending on the situation, multiple reviewers may be required:

``Tasks`` - Require review by the associated product owner *and* another developer. However, should the product owner wish and be able to provide an adequate code review then only the product owner is required to perform the review.

``Bugs`` - Require review only by another developer unless functionality or behaviour requires modification.

In your JIRA message requesting review, indicate how involved the review work will be ("quick" or "not quick").
The reviewer should promptly acknowledge the request, indicate whether they can do the review, and give a timeline for when they will be able to accomplish the request.
This allows the developer to seek an alternate reviewer if necessary.

Any team member in TSSW can review code.
For major changes, it is good to choose someone more experienced than yourself.
For minor changes, it may be good to choose someone less experienced than yourself.
For large changes, more than one reviewer may be assigned, possibly split by area of the code.
In this case, establish in the review request what each reviewer is responsible for.

**Do not assign multiple reviewers as a way of finding someone to review your work more quickly.**
It is better to communicate directly with potential reviewers directly to ascertain their availability.

Code reviews performed by peers are useful for a number of reasons:

- Peers are a good proxy for maintainability.
- It's useful for everyone to be familiar with other parts of the system.
- Good practices can be spread; bad practices can be deprecated.
- Performing a review is a fantastic way to learn new coding techiques/tips.

All developers are expected to make time to perform reviews as this is part of the overhead.
The TSSW Manager can intervene, however, if a developer is overburdened with review responsibility.

.. _workflow-code-review-process:

Code Review Discussion
----------------------

Using GitHub Pull Requests
^^^^^^^^^^^^^^^^^^^^^^^^^^

Code review discussion should happen on the GitHub pull request (PR), with the reviewer giving a discussion summary and conclusive thumbs-up on the JIRA ticket.

When conducting an extensive code review in a pull request, reviewers should use GitHub's `"Start a review" feature <github-review>`_.
This mode lets the reviewer queue multiple comments that are only sent once the review is submitted.
Note that GitHub allows a reviewer to classify a code review: "Comment," "Approve," or "Request changes."
While useful, this feature doesn't replace JIRA for formally :ref:`marking a ticket as being reviewed <workflow-resolving-review>` and a manual changing of the ticket status is required by the review.

.. _github-review: https://help.github.com/articles/reviewing-proposed-changes-in-a-pull-request/

Reviewers should use GitHub's `line comments`_ to discuss specific pieces of code.
As line comments are addressed, the developer may use GitHub's `emoji reactions`_ to indicate that the work is done (the "👍" works well).
Responding to each line comment isn't required, but it can help a developer track progress in addressing comments.
We discourage replies that merely say "Done" since *text* replies generate email traffic; emoji reactions aren't emailed.
Of course, use text replies if a discussion is required.

.. _line comments: https://help.github.com/articles/commenting-on-a-pull-request/#adding-line-comments-to-a-pull-request
.. _emoji reactions: https://help.github.com/articles/about-discussions-in-issues-and-pull-requests/

.. figure:: /_static/processes/workflow/reaction@2x.gif

   GitHub PR reactions are recommended for checking off completion of individual comments.

Another effective way to track progress towards addressing general review comments is with `Markdown task lists`_.

.. _Markdown task lists: https://help.github.com/articles/about-task-lists/

.. _workflow-resolving-review:

Resolving a review
^^^^^^^^^^^^^^^^^^

Code reviews are a collaborative check-and-improve process.
Reviewers do not hold absolute authority, nor can developers ignore the reviewer's suggestions.
The aim is to discuss, iterate, and improve the pull request until the work is ready to be deployed on ``develop``.

If the review becomes stuck on a design decision, that aspect of the review can be elevated into an RFC to seek team-wide consensus. **WE DON'T HAVE A MECHANISM FOR THIS**

If an issue is outside the ticket's scope, the reviewer should file a new ticket.

Once the iterative review process is complete, the reviewer should switch the JIRA ticket's state to **Reviewed**.

Note that in many cases the reviewer will mark a ticket as **Reviewed** before seeing the requested changes implemented.
This convention is used when the review comments are non-controversial; the developer can simply implement the necessary changes and self-merge.
The reviewer does not need to be consulted for final approval in this case.

Resolving with multiple reviewers
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If there are multiple reviewers, our convention is that each review removes their name from the Reviewers list to indicate sign-off; the final reviewer switches the status to **Reviewed.**
This indicates the ticket is ready to be merged.

.. _workflow-code-review-merge-develop:

Merging to Develop
------------------

Putting a ticket in a **Reviewed** state gives the developer the go-ahead to merge the ticket branch to ``develop``.
If it has not been done already, the developer should rebase the ticket branch against the latest master and rerun the CI-tests on Jenkins.
During this rebase, we recommend squashing any fixup commits into the main commit implementing that feature.
Git commit history should not record the iterative improvements from code review.

We **always use non-fast forward merges** so that the merge point is marked in Git history, with the merge commit containing the ticket number:

.. code-block:: bash

   git checkout develop
   git pull  # Sanity check; rebase ticket if master was updated.
   git merge --no-ff tickets/TSS-NNNN
   git push

**GitHub pull request pages also offer a 'big green button' for merging a branch to master.** We discourage you from using this button since there isn't a convenient way of knowing that the merged development history graph will be linear from GitHub's interface.

Rebasing the ticket branch against ``develop`` and doing the non-fast forward merging on the command line is the safest workflow.

The ticket branch may be deleted from the GitHub remote if its name is in the merge commit comment (which it is by default).

.. _workflow-CI-testing-develop:

Continuous Integration Testing on Develop
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The develop branch is where the full set of continuous integration tests occur and the inter-dependencies of the full code base is evaluated. This set of CI-tests is managed by the TSSW Quality Assurance (QA) assignee and supported by the developers of the repo. 

.. _workflow-fixing-breakage-develop:

Fixing a Breakage on Develop
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In rare cases, despite the pre-merge integration testing process described :ref:`above <workflow-testing>`, a merge to develop might accidentally contain an error and "break the build".
If this occurs, the merge may be reverted by anyone who notices the breakage and verifies that the merge is the cause -- unless a fix can be created, tested, reviewed, and merged very promptly. The parties involved in the fix at this level are the developer and QA person. The product owner is not required to be involved unless a change in functionality or behaviour of the code is modified.

.. _workflow-announce:

Announce the Change
-------------------

Once the merge has been completed, the developer should mark the JIRA ticket as **Done**.
If this ticket adds a significant feature or fixes a significant bug, it should be announced in the `TBR`.
In addition, if this update affects users, a short description of its effects from the user point of view should be prepared for the release notes that accompany each major release.
(Release notes are currently collected via team-specific procedures.)

.. _workflow-code-review-merge-master:

Merging to Master
------------------

Upon reaching a milestone where significant changes have been incorporated to perform a new release of the code (which shall be dictated by the product owner and/or TSSW Manager), a ticket is created to perform a release and issued to the QA person. The release branch is cut from develop to undergo a series of testing by the QA group prior to release. Any bug fixes are done to this release only and are not to be performed on the develop branch with a subsequent re-branching for release.

Once the release candidate has passed all tests, the release must undergo a subsequent review by the product owner via a pull request. **AND A SENIOR DEVELOPER?**

Upon successful review, the ticket is marked as **Reviewed** giving the QA person the go-ahead to merge the release branch to both develop and master and create a tag of the master.

The ticket is then marked as **Done** by the QA person.


**NO REBASE HAPPENS HERE, CORRECT?**

Git commit history should not record the iterative improvements from code review.

Upon creation of the new tag, it should be announced via the same mechanism described :ref:`above <workflow-announce>`.

.. _workflow-fixing-breakage-master:

Fixing a breakage on master
^^^^^^^^^^^^^^^^^^^^^^^^^^^

In rare cases, despite the pre-merge integration testing process described :ref:`above <workflow-testing>`, a merge to master might accidentally contain an error and "break the build".
If this occurs, the merge may be reverted by anyone who notices the breakage and verifies that the merge is the cause -- unless a fix can be created, tested, reviewed, and merged very promptly.

**SHOULD DISCUSS THIS**

.. _git-commit-organization-best-practices:

Appendix: Commit Organization Best Practices
============================================

.. _git-commit-organization-logical-units:

Commits should represent discrete logical changes to the code
-------------------------------------------------------------

`OpenStack has an excellent discussion of commit best practices <https://wiki.openstack.org/wiki/GitCommitMessages#Structural_split_of_changes>`_; this is recommended reading for all TSSW developers.
This section summarizes those recommendations.

Commits on a ticket branch should be organized into discrete, self-contained units of change.
In general, we encourage you to err on the side of more granular commits; squashing a pull request into a single commit is an anti-pattern.
A good rule-of-thumb is that if your commit *summary* message needs to contain the word 'and,' there are too many things happening in that commit.

Associating commits to a single logical change makes debugging and code audits easier:

- Git bisect is more effective for zeroing in on the change that introduced a regression.
- Git blame is more helpful for explaining why a change was made.
- Better commit organization guides reviewers through your pull request, making for more effective code reviews.
- A bad commit can more easily be reverted later with fewer side-effects.

Some edits serve only to fix white space or code style issues in existing code.
Those whitespace and style fixes should be made in separate commits from new development.
Usually it makes sense to fix whitespace and style issues in code *before* embarking on new development (or rebase those fixes to the beginning of your ticket branch).

Rebase commits from code reviews rather than having 'review feedback' commits
-----------------------------------------------------------------------------

Code review will result in additional commits that address code style, documentation and implementation issues.
Authors should rebase (i.e., ``git rebase -i master``) their ticket branch to squash the post-review fixes to the pre-review commits.
The end-goal is that a pull request, when merged, should have a coherent development story and look as if the code was written correctly the first time.

There is *no need* to retain post-review commits in order to preserve code review discussions.
So long as comments are made in the 'Conversation' and 'Files changed' tabs of the pull request GitHub will preserve that content.

.. _git-commit-message-best-practices:

Appendix: Commit Message Best Practices
=======================================

Generally you should write your commit messages in an editor, not at the prompt.
Reserve the ``git commit -m "messsage"`` pattern for 'work in progress' commits that will be rebased before code review.

We follow standard conventions for Git commit messages, which consist of a short summary line followed by body of discussion.
`Tim Pope wrote about commit message formatting <http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html>`_.

.. _git-commit-message-summary:

Writing commit summary lines
----------------------------

**Messages start with a single-line summary of 50 characters or less**.
Consider 50 characters as a hard limit; your summary will be truncated in the  GitHub UI otherwise.
Write the message in the **imperative** tense, not the past tense.
For example, "Add feature ..." and "Fix issue ..." rather than "Added feature..." and "Fixed feature...."
Ensure the summary line contains the right keywords so that someone examining `a commit listing <https://github.com/lsst/afw/commits/master>`_ can understand what parts of the codebase are being changed.
For example, it is useful to prefix the commit summary with the area of code being addressed.

.. _git-commit-message-body:

Writing commit message body content
-----------------------------------

**The message body should be wrapped at 72 character line lengths**, and contain lists or paragraphs that explain the code changes.
The commit message body describes:

- What the original issue was; the reader shouldn't have to look at JIRA to understand what prompted the code change.
- What the changes actually are and why they were made.
- What the limitations of the code are. This is especially useful for future debugging.

Git commit messages *are not* used to document the code and tell the reader how to use it.
Documentation belongs in code comments, docstrings and documentation files.

If the commit is trivial, a multi-line commit message may not be necessary.
Conversely, a long message might suggest that the :ref:`commit should be split <git-commit-organization-best-practices>`.
The code reviewer is responsible for giving feedback on the adequacy of commit messages.

The `OpenStack docs have excellent thoughts on writing great commit messages <https://wiki.openstack.org/wiki/GitCommitMessages#Information_in_commit_messages>`_.

.. _JIRA: https://jira.lsstcorp.org/
