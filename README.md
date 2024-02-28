# Repo to exemplify the use of git and github using GitFlow

It uses the following tools:
- Git
- GitHub
- GitFlow
- pre-commit

The GitFlow configuration is the following:
```
Branch name for production releases: [prod]
Branch name for "next release" development: [dev]

How to name your supporting branch prefixes?
Feature branches? [feature/] feat/
Bugfix branches? [bugfix/] fix/
Release branches? [release/]
Hotfix branches? [hotfix/] fix/
Support branches? [support/]
Version tag prefix? []
```

The pull requests must be Squash and Merge

Use `commitizen` to create the commit messages.

The version is proposed to be in the format `S.R.H` where:
- `S` is the sprint number
- `R` is the release number
- `H` is the hotfix number

Use `git-flow` to manage the branches and the releases.

# Flow
## Start a new feature
```bash
git flow feature start <ticket-number>
```

### Add the changes to the feature
Create as many commits as needed. It is recommended to use `cz c` to create the commit messages.
Doesn't really matter the commit messages in this stage; later, when the feature is finished, the messages will be squashed into a single one, that will be the feature message.

### Propose the feature
Push the feature to the remote repository and create a pull request to the `dev` branch.

If the pull request is approved, the feature will be merged and squashed into a single commit (propose the message, make sure it involves the ticket number using the format `<resolve,fix> #<ticket-number>`).

### Cleanup
In your local repository, update the `dev` branch
```bash
git switch dev
git pull
```
Optionally, delete the feature branch
```bash
git branch -d <feature-name>
```


### Test the feature
If needed by the test team, move the `test` branch to the commit that contains the feature.


## Start a new release (End of the sprint)
Define the version of the release
```bash
sprint=<sprint-number>.0.0
```

```bash
git flow release start ${sprint}
```

### Last minute changes
If needed, create new commits with the changes.


### Bump the version
```bash
cz bump ${sprint}
```
This will create a CHANGELOG.md file with the changes.

### Propose the release
Push the release to the remote repository and create a pull request to the `prod` branch.

If the pull request is approved, the release will be merged.

TODO: Auto merge the release branch into the `dev` branch.
TODO: Auto create a tag with the version of the release.

## Start a new hotfix
```bash
git flow hotfix start <ticket-number>
```

### Add the changes to the hotfix
Create as many commits as needed. It is recommended to use `cz c` to create the commit messages.

### Propose the hotfix
Push the hotfix to the remote repository and create a pull request to the `prod` branch.

If the pull request is approved, the hotfix will be merged and squashed into a single commit.

TODO: Auto merge the hotfix branch into the `dev` branch.
TODO: Auto create a tag with the version of the hotfix.

### Cleanup
In your local repository, update the `dev` branch
```bash
git switch dev
git pull
```

Mandatory: delete the hotfix branch, otherwise, git-flow will not allow you to create a new hotfix.
```bash
git branch -d <hotfix-name>
```
