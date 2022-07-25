# GitHub Action: Create Pull Request for Submodule Update

This action updates a submodule of the target repository in a feature branch and 
opens a pull request against the main branch to commit changes.

If there is an open PR already, the action will just update and force-push the 
feature branch.

## Parameters

- `github_token` — GitHub token of a user with `write` permissions in the
  target repository. Provide this token as an action secret.
- `target_repository` — the full name of the target repository: 
  `<owner>/<repo>`.
- `checkout_branch` — the branch to checkout.
- `feature_branch` — the feature branch.
- `pr_against_branch` - the branch to open PR against; usually the same as 
  `CHECKOUT_BRANCH`.
- `pr_title` - used as the PR title and PR body.
- `commit_user_name` - the user name for the commit.
- `commit_user_email` - the user email for the commit.
- `commit_message` - the message for the commit.

Example workflow:

```yml
---
name: Submodule update

on:
  push:
    branches: 
    - master
    - main

jobs:
  submodule-update:
    name: Submodule update
    runs-on: docker
    env:
      TARGET_REPOSITORY: 'org/repo'
      CHECKOUT_BRANCH: 'main'
      FEATURE_BRANCH: 'update-submodule'
      PR_AGAINST_BRANCH: 'main'
      PR_TITLE: 'Update submodule <name> on branch <checkout branch>'
      COMMIT_USER_NAME: SomeBot
      COMMIT_USER_EMAIL: bot@example.com
      COMMIT_MASSAGE: Bump submodule to new version

    steps:
      - name: Create PR with submodule update
        uses: tarantool/actions/update-submodule@master
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          target_repository: ${{ env.TARGET_REPOSITORY }}
          checkout_branch: ${{ env.CHECKOUT_BRANCH }}
          feature_branch: ${{ env.FEATURE_BRANCH }}
          pr_against_branch: ${{ env.PR_AGAINST_BRANCH }}
          pr_title: ${{ env.PR_TITLE }}
          commit_user_name: ${{ env.COMMIT_USER_NAME }}
          commit_user_email: ${{ env.COMMIT_USER_EMAIL }}
          commit_message: ${{ env.COMMIT_MESSAGE }}
```
