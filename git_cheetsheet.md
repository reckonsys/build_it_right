# GIT CHEATSHEET
# Branching
1. Pull down branches from a remote: `git fetch`
1. Viewing all (local and remote) branches: `git branch -a`
1. Deleting a local branch: `git branch -d <branch name>`
1. Deleting a remote branch: `git push origin --delete <branch name>`
1. Pushing a branch to a remote: `git push origin <branch name>`
1. References:
    1. [Git Branching - Branches in a Nutshell](https://git-scm.com/book/id/v2/Git-Branching-Branches-in-a-Nutshell)
    1. [Git housekeeping tutorial: clean-up outdated branches in local and remote repositories](https://railsware.com/blog/2014/08/11/git-housekeeping-tutorial-clean-up-outdated-branches-in-local-and-remote-repositories/)

# Tags
1. Viewing: `git tag`
1. Tagging a branch:
    1. Switch to branch: `git checkout <branch name>`
    1. Create a tag of the current state of the branch: `git tag -a <tag name> -m "<commit message>"`
1. Tagging a commit:
    1. Find the hash you want to tag
    1. Create the tag: `git tag -a <tag name> <hash> -m "<commit message>"`
1. Sharing tags (pushing to another repo, e.g. remote):
    1. Push a single tag: `git push origin v1.5`
    1. Push all tags from local to remote: `git push origin --tags`
1. Viewing a tag message: `git show <tag name>`
    1. *Note*: Bitbucket currently does not display tag messages, but they can be
       viewed in a CLI
1. Showing the commit a tag references: `git rev-list -n 1 <tag name>`
1. Viewing the commit details: `git show <hash>`
1. Read more here: [Git Basics - Tagging](https://git-scm.com/book/en/v2/Git-Basics-Tagging)

# Merging a branch back into a master
1. Pull latest from the remote: `git fetch`
1. Switch to the branch to merge into: `git checkout master`
1. Create a new branch: `git checkout -b merge_master`
1. Merge: `git merge --no-ff origin release-1.2.0`
1. Push changes to the remote: 'git push origin merge_master'
1. Create a pull request

# Rebasing
Rebase is useful when you want to pull shared changes into your local changes. In this case shared changes would be in master and your local changes would be in a feature branch. Rebasing will move your new commits in the feature branch to begin on the tip of master.

**Note:** Before rebasing, make sure you have changes either checked in to your feature branch or stashed.

1. First pull the latest from master:
```
git checkout master
git pull origin master
```
1. Then switch to your feature branch: `git checkout <feature branch name>`
1. Now rebase: `git rebase origin/master`

Then resolve any merge commits if needed.

See also: [Merging vs. Rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)

# Cherry Picking
Cherry picking allows you to move specific commits from one branch to another. At Reckonsys we use this process to update release branches. Changes should be pushed to **master** first, then cherry picked into a **release** branch. We do this to ensure master is always up to date and we do not merge changes from a release branch back into master. Normally a cherry pick should not occur until after the change has been reviewed and merged into master, so ideally you should be picking from the master branch.

## Locate the commit to cherry pick

1. Pull the latest from master so you'll have your merged changed
1. Use `git log` to read the commit log
1. Find your change and the commit hash for your change


## Setup a branch to insert the cherry pick into
Create a feature branch from a **release** branch. This will be the branch used to pick your change into.

1. Fetch the latest: `git fetch`
1. Checkout the branch: `git checkout <release branch name>`
1. Make sure you have the latest changes: `git pull origin <release branch name>`
1. Create a new branch for the pull request. Example: `git checkout -b cherrypick_ticket1350`
    1. **Note:** Using a naming convention like *cherrypick_* makes it clearer to the pull request reviewer that the change has already been merged into master.
1. Cherry pick the commit: `git cherry-pick <commit hash from step 1>`
1. Push the new branch: `git push origin cherrypick_ticket1350`
1. Create a pull request, setting the destination to the **release** branch.
