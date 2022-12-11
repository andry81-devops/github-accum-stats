<p align="center">
  <a href="https://github.com/andry81-stats/github-accum-stats--gh-stats/commits/master/traffic/views">
    <img src="https://github.com/andry81-cache/andry81-devops--gh-content-cache/raw/master/repo/andry81-devops/github-accum-stats/badges/traffic/views/all.svg" valign="middle" alt="GitHub views|any|total" />
    <img src="https://github.com/andry81-cache/andry81-devops--gh-content-cache/raw/master/repo/andry81-devops/github-accum-stats/badges/traffic/views/all-14d.svg" valign="middle" alt="GitHub views|any|14d" /></a>
• <a href="https://github.com/andry81-stats/github-accum-stats--gh-stats/commits/master/traffic/views">
    <img src="https://github.com/andry81-cache/andry81-devops--gh-content-cache/raw/master/repo/andry81-devops/github-accum-stats/badges/traffic/views/unq.svg" valign="middle" alt="GitHub views|unique per day|total" />
    <img src="https://github.com/andry81-cache/andry81-devops--gh-content-cache/raw/master/repo/andry81-devops/github-accum-stats/badges/traffic/views/unq-14d.svg" valign="middle" alt="GitHub views|unique per day|14d" /></a>
</p>

<p align="center">
  <a href="https://github.com/andry81-stats/github-accum-stats--gh-stats/commits/master/traffic/clones">
    <img src="https://github.com/andry81-cache/andry81-devops--gh-content-cache/raw/master/repo/andry81-devops/github-accum-stats/badges/traffic/clones/all.svg" valign="middle" alt="GitHub clones|any|total" />
    <img src="https://github.com/andry81-cache/andry81-devops--gh-content-cache/raw/master/repo/andry81-devops/github-accum-stats/badges/traffic/clones/all-14d.svg" valign="middle" alt="GitHub clones|any|14d" /></a>
• <a href="https://github.com/andry81-stats/github-accum-stats--gh-stats/commits/master/traffic/clones">
    <img src="https://github.com/andry81-cache/andry81-devops--gh-content-cache/raw/master/repo/andry81-devops/github-accum-stats/badges/traffic/clones/unq.svg" valign="middle" alt="GitHub clones|unique per day|total" />
    <img src="https://github.com/andry81-cache/andry81-devops--gh-content-cache/raw/master/repo/andry81-devops/github-accum-stats/badges/traffic/clones/unq-14d.svg" valign="middle" alt="GitHub clones|unique per day|14d" /></a>
</p>

<p align="center">
  <a href="https://github.com/andry81/donate"><img src="https://github.com/andry81-cache/andry81--gh-content-cache/raw/master/common/badges/donate/donate.svg" valign="middle" alt="donate" /></a>
</p>

---

## Tutorial to setup accumulation of various GitHub repository/account statistic

> This implementation is based on that: https://github.com/andry81-devops/github-clone-count-badge

> **Warning** This tutorial does contain content related to the GitHub itself. All other description has moved into other tutorials.
>

All tutorials: https://github.com/andry81/index#tutorials

**Features**:

1. Implementation can accumulate GitHub traffic clones or/and views, GitHub account rate limits of an authenticated user.

2. Workflow has used a bash script to accumulate statistic:

   * GitHub traffic clones/views: [accum-stats.sh](https://github.com/andry81-devops/gh-workflow/blob/master/bash/github/accum-stats.sh)

   * GitHub account rate limits: [accum-rate-limits.sh](https://github.com/andry81-devops/gh-workflow/blob/master/bash/github/accum-rate-limits.sh)

3. Basically most of the scripts does accumulate the response. For example, the clones accumulator script does accumulate statistic both into a single file: `traffic/clones/latest-accum.json`,
   and into a set of files grouped by year and allocated per day: `traffic/clones/by_year/YYYY/YYYY-MM-DD.json`.

4. A repository based accumulator script has a repository to track and a repository to store traffic statistic and they may be different. If they is different, then you can directly point the statistic as a standalone commits list: `https://github.com/{{REPO_OWNER}}/{{REPO}}--gh-stats/commits/master/traffic/clones`.

5. All scripts does use GitHub composite action to reuse workflow code base: https://docs.github.com/en/actions/creating-actions/creating-a-composite-action

> **Warning** Not all features of a generic GitHub action is supported: [Known Issues](#known-issues)

You need setup 3-4 repositories:

1. Repository which statistic you want to track: `myrepo`.<br />
   > **Note** This repository is only required for repository based statistic scripts.

2. Repository, where statistic will be saved: `myrepo--gh-stats`.<br />
   > **Note** You still can use a single repository to request and to store, but it is not convenient and will distort the clone statistic (at least until the GitHub action user who is used to checkout the statistic output repository won't be involved into clones counter change), so is not recommended.

3. Repository, where to store github workflow support scripts: `gh-workflow`.<br />
   You can fork or use it from here: https://github.com/andry81-devops/gh-workflow

4. Repository, where to store github composite action:

   * GitHub composite action to request and accumulate a repository clones and/or views statistic:<br />
     https://github.com/andry81-devops/gh-action--accum-gh-stats

   * GitHub composite action to request and accumulate an account rate limits:<br />
     https://github.com/andry81-devops/gh-action--accum-gh-rate-limits

   All action scripts:<br />
   https://github.com/andry81/index#action-scripts

> **Note** See <a href="#reuse">REUSE</a> section for details if you have multiple repositories and want to store all GitHub workflow scripts (`.github/workflows/*.yml`) in a single repository.

> **Note** You need to attach a personal access token (PAT) into a repository used to run a GitHub action script (`.github/workflows/*.yml`) to read content from another repository and obtain the read permission from that repository: `repo`->`public_repo`.

> **Note** You need to attach a personal access token (PAT) into a repository used to run a GitHub action script (`.github/workflows/*.yml`) to write content into another repository and obtain the push permission into that repository: `repo`.

> **Note** You need to attach a personal access token (PAT) into a repository used to run a GitHub action script (`.github/workflows/*.yml`) to read statistic of another repository and obtain the push permission into that repository: `repo`.

> **Note** A separate personal access token (PAT) does not require to be attached into a repository used to run a GitHub action script for another repository as long as that another repository is owned by the same owner as a repository which runs a GitHub action script.

* `myrepo` -> needs read statistic permission
* `myrepo--gh-stats` -> needs read/write content permission

To generate PAT: https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token

To attach PAT: https://docs.github.com/en/actions/reference/encrypted-secrets#creating-encrypted-secrets-for-a-repository

> **Warning** Beginning from the `gh-workflow` `1.3.0` as a dependency to all action scripts the checkout action script https://github.com/actions/checkout can be replaced by a wrapper script https://github.com/andry81-devops/gh-action--git-checkout which does support checkout from an empty repository and you can leave an output repository empty before the first checkout.
> Now the initial output repository state is not required as input. The information about initial output repository state is removed from here.

The `myrepo` repository should contain 1 file per statistic entity:

* [.github/workflows/accum-gh-clone-stats.yml example](https://github.com/andry81-devops/gh-action--accum-gh-stats#accum-gh-clone-stats-yml)

* [.github/workflows/accum-gh-view-stats.yml example](https://github.com/andry81-devops/gh-action--accum-gh-stats#accum-gh-view-stats-yml)

* [.github/workflows/accum-gh-rate-limits.yml example](https://github.com/andry81-devops/gh-action--accum-gh-rate-limits#accum-gh-rate-limits-yml)

> **Warning** You must replace all placeholder into respective values:

* `{{REPO_OWNER}}` -> repository owner
* `{{REPO}}` -> `myrepo`

After the github workflow yaml file is commited and pushed, you can run the action from the `Actions` tab in the `myrepo` repository.

After that you can add badges to reference a repository statistic:

```xml
<p align="center">
  <a href="https://github.com/{{REPO_OWNER}}/{{REPO}}--gh-stats/commits/master/traffic/views">
    <img src="https://img.shields.io/badge/dynamic/json?color=success&label=Github%20views|all&query=count&url=https://github.com/{{REPO_OWNER}}/{{REPO}}--gh-stats/raw/master/traffic/views/latest-accum.json?raw=True&logo=github" valign="middle" alt="GitHub views|any|total" />
    <img src="https://img.shields.io/badge/dynamic/json?color=success&label=14d&query=count&url=https://github.com/{{REPO_OWNER}}/{{REPO}}--gh-stats/raw/master/traffic/views/latest.json?raw=True" valign="middle" alt="GitHub views|any|14d" /></a>
• <a href="https://github.com/{{REPO_OWNER}}/{{REPO}}--gh-stats/commits/master/traffic/views">
    <img src="https://img.shields.io/badge/dynamic/json?color=success&label=Github%20views|unq&query=uniques&url=https://github.com/{{REPO_OWNER}}/{{REPO}}--gh-stats/raw/master/traffic/views/latest-accum.json?raw=True&logo=github" valign="middle" alt="GitHub views|unique per day|total" />
    <img src="https://img.shields.io/badge/dynamic/json?color=success&label=14d&query=uniques&url=https://github.com/{{REPO_OWNER}}/{{REPO}}--gh-stats/raw/master/traffic/views/latest.json?raw=True" valign="middle" alt="GitHub views|unique per day|14d" /></a>
</p>

<p align="center">
  <a href="https://github.com/{{REPO_OWNER}}/{{REPO}}--gh-stats/commits/master/traffic/clones">
    <img src="https://img.shields.io/badge/dynamic/json?color=success&label=Github%20clones|all&query=count&url=https://github.com/{{REPO_OWNER}}/{{REPO}}--gh-stats/raw/master/traffic/clones/latest-accum.json?raw=True&logo=github" valign="middle" alt="GitHub clones|any|total" />
    <img src="https://img.shields.io/badge/dynamic/json?color=success&label=14d&query=count&url=https://github.com/{{REPO_OWNER}}/{{REPO}}--gh-stats/raw/master/traffic/clones/latest.json?raw=True" valign="middle" alt="GitHub clones|any|14d" /></a>
• <a href="https://github.com/{{REPO_OWNER}}/{{REPO}}--gh-stats/commits/master/traffic/clones">
    <img src="https://img.shields.io/badge/dynamic/json?color=success&label=Github%20clones|unq&query=uniques&url=https://github.com/{{REPO_OWNER}}/{{REPO}}--gh-stats/raw/master/traffic/clones/latest-accum.json?raw=True&logo=github" valign="middle" alt="GitHub clones|unique per day|total" />
    <img src="https://img.shields.io/badge/dynamic/json?color=success&label=14d&query=uniques&url=https://github.com/{{REPO_OWNER}}/{{REPO}}--gh-stats/raw/master/traffic/clones/latest.json?raw=True" valign="middle" alt="GitHub clones|unique per day|14d" /></a>
</p>
```

> **Note** There is https://github.com/andry81-devops/gh-action--accum-content action script which can periodically download all the dynamic badges and other files into a content cache repository.

You can add links pointing a content cache repository instead, to leave the `README.md` a bit simplier:

```xml
<p align="center">
  <a href="https://github.com/{{REPO_OWNER}}/{{REPO}}--gh-stats/commits/master/traffic/views">
    <img src="https://github.com/{{REPO_OWNER}}-cache/{{REPO_OWNER}}--gh-content-cache/raw/master/repo/{{REPO_OWNER}}/{{REPO}}/badges/traffic/views/all.svg" valign="middle" alt="GitHub views|any|total" />
    <img src="https://github.com/{{REPO_OWNER}}-cache/{{REPO_OWNER}}--gh-content-cache/raw/master/repo/{{REPO_OWNER}}/{{REPO}}/badges/traffic/views/all-14d.svg" valign="middle" alt="GitHub views|any|14d" /></a>
• <a href="https://github.com/{{REPO_OWNER}}/{{REPO}}--gh-stats/commits/master/traffic/views">
    <img src="https://github.com/{{REPO_OWNER}}-cache/{{REPO_OWNER}}--gh-content-cache/raw/master/repo/{{REPO_OWNER}}/{{REPO}}/badges/traffic/views/unq.svg" valign="middle" alt="GitHub views|unique per day|total" />
    <img src="https://github.com/{{REPO_OWNER}}-cache/{{REPO_OWNER}}--gh-content-cache/raw/master/repo/{{REPO_OWNER}}/{{REPO}}/badges/traffic/views/unq-14d.svg" valign="middle" alt="GitHub views|unique per day|14d" /></a>
</p>

<p align="center">
  <a href="https://github.com/{{REPO_OWNER}}/{{REPO}}--gh-stats/commits/master/traffic/clones">
    <img src="https://github.com/{{REPO_OWNER}}-cache/{{REPO_OWNER}}--gh-content-cache/raw/master/repo/{{REPO_OWNER}}/{{REPO}}/badges/traffic/clones/all.svg" valign="middle" alt="GitHub clones|any|total" />
    <img src="https://github.com/{{REPO_OWNER}}-cache/{{REPO_OWNER}}--gh-content-cache/raw/master/repo/{{REPO_OWNER}}/{{REPO}}/badges/traffic/clones/all-14d.svg" valign="middle" alt="GitHub clones|any|14d" /></a>
• <a href="https://github.com/{{REPO_OWNER}}/{{REPO}}--gh-stats/commits/master/traffic/clones">
    <img src="https://github.com/{{REPO_OWNER}}-cache/{{REPO_OWNER}}--gh-content-cache/raw/master/repo/{{REPO_OWNER}}/{{REPO}}/badges/traffic/clones/unq.svg" valign="middle" alt="GitHub clones|unique per day|total" />
    <img src="https://github.com/{{REPO_OWNER}}-cache/{{REPO_OWNER}}--gh-content-cache/raw/master/repo/{{REPO_OWNER}}/{{REPO}}/badges/traffic/clones/unq-14d.svg" valign="middle" alt="GitHub clones|unique per day|14d" /></a>
</p>
```

**Features of a standalone content cache repository**:

https://github.com/andry81-devops/accum-content#features-of-a-standalone-content-cache-repository

## REUSE

> **Warning** The GitHub does disable all workflow scripts in case of repository main branch inactivity for a period of time: [Known Issues](#known-issues)

You can reuse all workflow scripts from a single repository to ease the maintain and scripts edit.

For example, if you have 3 repositories `myrepo1`, `myrepo2`, `myrepo3` and the repository owner is `user`, then you can put all workflow scripts into a single repository `github.com/user/user/.github/workflows` in this way:

* `myrepo1.accum-gh-clone-stats.yml`
* `myrepo1.accum-gh-view-stats.yml`
* `myrepo2.accum-gh-clone-stats.yml`
* `myrepo2.accum-gh-view-stats.yml`
* `myrepo3.accum-gh-clone-stats.yml`
* `myrepo3.accum-gh-view-stats.yml`

And use slightly different naming:

```yml
name: "myrepo1: GitHub clones counter for 14 days at every 8 hours and clones accumulator"
```

```yml
name: "myrepo1: GitHub views counter for 14 days at every 8 hours and views accumulator"
```

> **Note** If you have multiple repositories to store the statistic, then you can create a github organization account like `<owner>-stats` and move all statistic repositories into organization's account.
> It will leave the repositories page of the original account untouched on each commit into an organization account repository.

## Known Issues

### GitHub Actions does automatically disable all workflow scripts after a period of time of inactivity

* Last known period: 60 days

For example, if GitHub repository has workflow scripts and was inactive a period of time, then all workflow scripts does disable.
You have to either make a commit to update the period and reenable the workflow scripts manually.

https://stackoverflow.com/questions/67184368/prevent-scheduled-github-actions-from-becoming-disabled

### `git fetch` error: `could not read Username for 'https://github.com': terminal prompts disabled`

```
Fetching the repository
  /usr/bin/git -c protocol.version=2 fetch --no-tags --prune --progress --no-recurse-submodules --depth=1 origin +refs/heads/master*:refs/remotes/origin/master* +refs/tags/master*:refs/tags/master*
  Error: fatal: could not read Username for 'https://github.com': terminal prompts disabled
  The process '/usr/bin/git' failed with exit code 128
  Waiting 14 seconds before trying again
```

Variants:

* You missed to update the repo `secrets` token.

### `git fetch` error: `The process '/usr/bin/git' failed with exit code 1`

```
Fetching the repository
  /usr/bin/git -c protocol.version=2 fetch --no-tags --prune --progress --no-recurse-submodules --depth=1 origin +refs/heads/master*:refs/remotes/origin/master* +refs/tags/master*:refs/tags/master*
  The process '/usr/bin/git' failed with exit code 1
```

Variants:

* The checkout performs on an empty repository:
  https://github.com/actions/checkout/issues/746

### The GitHub composite action has supported not all features of a generic GitHub action: https://github.com/actions/runner/issues/646

> **What does Composite Run Steps Not Support**
>
> We don't support setting conditionals, continue-on-error, timeout-minutes, "uses", and secrets on individual steps within a composite action right now.
>
> (Note: we do support these attributes being set in workflows for a step that uses a composite run steps action)

### Cygwin error: `Error: open /tmp/tmp.XXXXXXXXXX/...: The system cannot find the path specified.`

Variants:

* You have had try to run not Cygwin executable from the Cygwin shell. The Cygwin does not convert a posix path into the Windows path automatically.

  To workaround the error do use the `TEMP_DIR` variable to declare explicit temporary directory out of Cygwin `/tmp` directory.

### Linux error: `Error: open /tmp/...: no such file or directory`

Variants:

* You have had try to run `yq` from https://github.com/mikefarah/yq.

  Related issue:

  * `yq fails to process files in /tmp` : https://github.com/mikefarah/yq/discussions/1299

  To workaround the error do use the `TEMP_DIR` variable to declare explicit temporary directory out of Cygwin `/tmp` directory.

## Last known issues updates

### Updates on composite actions features

* `add conditional execution of action steps`: https://github.com/actions/runner/issues/834

  * https://github.blog/changelog/2021-11-09-github-actions-conditional-execution-of-steps-in-actions/

    Actions written in YAML, also known as composite actions, now support if conditionals.

* `Add ability to use continue-on-error from composite Action steps`: https://github.com/actions/runner/issues/1457

* `unable to inject shell to composite action`: https://github.com/actions/runner/issues/835

* `Boolean inputs are not actually booleans` : https://github.com/actions/runner/issues/1483
