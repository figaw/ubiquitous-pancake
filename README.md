# ubiquitous-pancake
this is a demo repository

## Branch Protection Rules

- `main`
    - "Require status checks to pass before merging"
    - "Require branches to be up to date before merging"

Didn't really do anything other than protect master from force-pushes.

Which is somewhat nice on its own, but it didn't prevent pushing to master.

# ubiquitous-pancake-dev
this is a demo repository

We will

1. checkout a new branch called `dev` on this repository.
1. make `dev` the base branch, on this repository.
1. delete the `master`-branch on this repository.

- Add branch protection with 6 reviewers to discourage merging to `dev`.
- But we will "allow force pushes," because that ~~might come in handy in a second~~
allows us to update the `dev` branch with the `master` from the other repository.

## Setup

Clone the `ubiquitous-pancake-dev` (origin) repository, as this is where we do the development.

```shell
git clone git@github.com:figaw/ubiquitous-pancake-dev.git
```

Add the `ubiquitous-pancake` (prod) repository, as a remote where,

- developers have read-only access.
- "integration managers" have read-write access.

```shell
git remote add prod git@github.com:figaw/ubiquitous-pancake.git
```

## Workflow - Fake-Fork, Integration-Manager

See: <https://git-scm.com/book/en/v2/Distributed-Git-Distributed-Workflows>

### The developer will...

1. Checkout the "truth": `git checkout main` (there is only `main` on prod)
1. Update the "truth": `git pull`
1. Start doing work: `git checkout -b feature/NG-123/newthing`
1. Do work: `touch file`, `git add .`, `git commit -m "ADD stuff"`
1. Deliver draft: `git push -u origin HEAD`
    - Branch is pushed to `dev` (origin) repository
1. A PR is created on the dev repository,
    at some point it's ready to be merged and
    an intergration manager takes over.

### The Integration Manager will...

1. Checkout the branch of the PR: `git checkout feature/NG-123/newthing`.
1. Deliver change `git push prod HEAD`. (`HEAD` means "current branch/commit")
1. Create a PR on the prod repository. (Maybe reference the PR on `-dev`..)
1. Merge to master on the production repository.
1. Close the PR on `-dev`
1. Switch to the local truth: `git checkout master`
1. Update the local truth: `git pull`
1. Update the remote dev with truth: `git push origin master:dev`

> NB: at this point we could update the `-dev` repository with the changes in `prod`
> but it's not really that interesting because all the developers are checking out the
> prod repository when they start making changes.

> NB: I'm slightly uncertain as to the problems of not keeping the `dev` branch up to date.
> [Update] the problems are, PR's on `-dev` will contain all the "missing" commits.
> Steps have been updated above.
