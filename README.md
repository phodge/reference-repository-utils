# reference-repository-utils
CLI utilities for working with git --reference repositories.

# Setup

A reference repository should be created with the following options:

* `--bare` to prevent git wasting time writing the repo files out to the reference repository
* `--no-tags` to prevent wasting time updating tag refs

It is also be a good idea to use:

* `--single-branch` to prevent churn from developer personal branches that may be rebased or deleted. To track additional branches that don't churn, use `git-also-fetch-branch`

# Commands

`update-reference-repository REPO_PATH [ --older-than=MINUTES ]`

Pull latest upstream changes into the reference repository cloned to $REPO_PATH. Ideally you want to use the `--older-than` argument to skip pulling if you have already done so in the last $MINUTES minutes. For example, you might put `update-reference-repository /path/to/reference-repo.git --older-than=720` in your `~/.profile` to ensure the reference repo is updated at the start of each day. Or if you have a script that runs `git fetch` then you might put `update-reference-repository /path/to/reference-repo.git --older-than=15` at the start of it so that it _usually_ pulls git objects to the reference repository first, but will also be fast if you repeat it multiple times in a repo.

`git-also-fetch-branch BRANCH_NAME_OR_PATTERN`

If you have used `--single-branch` to clone your reference repository or one of its subordinates, then it is likely you will want to track additional branches, such as those belonging to you or your team. You can add a full branch name: `git-also-fetch-branch "$BRANCH_NAME"`; or you can track all branches with a certain prefix: `git-also-fetch-branch 'fred/*'`.

`git-skip-fetch-branch BRANCH_NAME_OR_PATTERN`

Will turn off tracking of a branch added using `git-also-fetch-branch`.
