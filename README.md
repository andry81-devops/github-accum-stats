<!-- -->
<p align="center">
  <a href="#"><img src="https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2Fandry81%2Fgithub-accum-stat&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false" valign="middle" alt="hits" /></a>
</p>
<!-- -->

<p align="center">
  <a href="https://github.com/andry81-stats/github-accum-stats--gh-stats/commits/master/traffic/views">
    <img src="https://img.shields.io/badge/dynamic/json?color=success&label=Github%20views|all&query=count&url=https://github.com/andry81-stats/github-accum-stats--gh-stats/raw/master/traffic/views/latest-accum.json?raw=True&logo=github" valign="middle" alt="GitHub views|any|total" />
    <img src="https://img.shields.io/badge/dynamic/json?color=success&label=14d&query=count&url=https://github.com/andry81-stats/github-accum-stats--gh-stats/raw/master/traffic/views/latest.json?raw=True" valign="middle" alt="GitHub views|any|14d" /></a>
• <a href="https://github.com/andry81-stats/github-accum-stats--gh-stats/commits/master/traffic/views">
    <img src="https://img.shields.io/badge/dynamic/json?color=success&label=Github%20views|unq&query=uniques&url=https://github.com/andry81-stats/github-accum-stats--gh-stats/raw/master/traffic/views/latest-accum.json?raw=True&logo=github" valign="middle" alt="GitHub views|unique per day|total" />
    <img src="https://img.shields.io/badge/dynamic/json?color=success&label=14d&query=uniques&url=https://github.com/andry81-stats/github-accum-stats--gh-stats/raw/master/traffic/views/latest.json?raw=True" valign="middle" alt="GitHub views|unique per day|14d" /></a>
</p>

<p align="center">
  <a href="https://github.com/andry81-stats/github-accum-stats--gh-stats/commits/master/traffic/clones">
    <img src="https://img.shields.io/badge/dynamic/json?color=success&label=Github%20clones|all&query=count&url=https://github.com/andry81-stats/github-accum-stats--gh-stats/raw/master/traffic/clones/latest-accum.json?raw=True&logo=github" valign="middle" alt="GitHub clones|any|total" />
    <img src="https://img.shields.io/badge/dynamic/json?color=success&label=14d&query=count&url=https://github.com/andry81-stats/github-accum-stats--gh-stats/raw/master/traffic/clones/latest.json?raw=True" valign="middle" alt="GitHub clones|any|14d" /></a>
• <a href="https://github.com/andry81-stats/github-accum-stats--gh-stats/commits/master/traffic/clones">
    <img src="https://img.shields.io/badge/dynamic/json?color=success&label=Github%20clones|unq&query=uniques&url=https://github.com/andry81-stats/github-accum-stats--gh-stats/raw/master/traffic/clones/latest-accum.json?raw=True&logo=github" valign="middle" alt="GitHub clones|unique per day|total" />
    <img src="https://img.shields.io/badge/dynamic/json?color=success&label=14d&query=uniques&url=https://github.com/andry81-stats/github-accum-stats--gh-stats/raw/master/traffic/clones/latest.json?raw=True" valign="middle" alt="GitHub clones|unique per day|14d" /></a>
</p>

<p align="center">
  <a href="https://github.com/andry81/donate"><img src="https://github.com/andry81/andry81/raw/master/badges/donate.svg" valign="middle" alt="donate" /></a>
</p>

---

> This implementation is based on that: https://github.com/andry81-devops/github-clone-count-badge

Features:

1. Can count GitHub traffic clones or/and views, external inpage downloads (track counter), phpbb forum board view/replies (track counters), account rate limits of an authenticated user.

2. Workflow has used a bash script to accumulate statistic:

   * GitHub traffic clones/views: [accum-stats.sh](https://github.com/andry81-devops/gh-workflow/blob/master/bash/github/accum-stats.sh)

   * External inpage downloads: [accum-downloads.sh](https://github.com/andry81-devops/gh-workflow/blob/master/bash/inpage/accum-downloads.sh)

   * PhpBB forum board views/replies: [accum-stats.sh](https://github.com/andry81-devops/gh-workflow/blob/master/bash/board/accum-stats.sh)

   * Rate limits: [accum-rate-limits.sh](https://github.com/andry81-devops/gh-workflow/blob/master/bash/github/accum-rate-limits.sh)

3. Basically most of the scripts does accumulate the response. For example, the clones accumulator script does accumulate statistic both into a single file: `traffic/clones/latest-accum.json`,
   and into a set of files grouped by year and allocated per day: `traffic/clones/by_year/YYYY/YYYY-MM-DD.json`.

4. A repository based accumulator script has a repository to track and a repository to store traffic statistic and they may be different. If they is different, then you can directly point the statistic as a standalone commits list: `https://github.com/{{REPO_OWNER}}/{{REPO}}--gh-stats/commits/master/traffic/clones`.

5. All scripts does use GitHub composite action to reuse workflow code base: https://docs.github.com/en/actions/creating-actions/creating-a-composite-action

> :warning: Not all features of a generic GitHub action is supported: [Known Issues](#known_issues)

You need 4 repositories:

1. Repository which clone statistic you want to track: `myrepo`.
   > :information_source: See <a href="#reuse">REUSE</a> section for details if you have multiple repositories and want to store all workflow scripts in a single repository.
   
2. Repository, where clone statistic will be saved: `myrepo--gh-stats`.
   > :information_source: You still can use a single repository to request and to store, but it is not convenient and will distort the clone statistic (at least until the GitHub action user who is used to checkout the statistic output repository won't be involved into clones counter change), so is not recommended.
   
3. Repository, where to store github workflow scripts: `gh-workflow`.<br>
   You can fork it from here: https://github.com/andry81-devops/gh-workflow
   
4. Repository, where to store github composite action:

   * GitHub composite action to request and accumulate a repository clones and/or views statistic:<br />
     https://github.com/andry81-devops/gh-action--accum-gh-stats

   * GitHub composite action to request and accumulate downloads statistic from a value on a web page:<br />
     https://github.com/andry81-devops/gh-action--accum-inpage-downloads

   * GitHub composite action to request and accumulate a forum board post replies and views statistic:<br />
     https://github.com/andry81-devops/gh-action--accum-board-stats
     
   * GitHub composite action to request and accumulate an account rate limits:<br />
     https://github.com/andry81-devops/gh-action--accum-gh-rate-limits

   The list of actions (`gh-action--*`):
   https://github.com/orgs/andry81-devops/repositories?q=gh-action--

You need to attach a personal access token (PAT) into the repository being requested for statistic and obtain the push permission (`repo`->`public_repo`):

* `myrepo` -> `READ_STATS_TOKEN`

> :information_source: The `myrepo--gh-stats` repository does not require a separate PAT token as long as it is owned by the same repository owner.

To generate PAT: https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token

To attach PAT: https://docs.github.com/en/actions/reference/encrypted-secrets#creating-encrypted-secrets-for-a-repository

The `myrepo--gh-stats` repository should contain 2 files per each statistic entity:

> :information_source: The version `gh-workflow` `1.1.3` and higher supports empty/unexisted json files or json files with empty values.
> So you may not add these files at least for the `gh-action--accum-gh-stats` action.

> :warning: But because the checkout action acript does not support checkout from an empty repository (See the [Known Issues](#known_issues)), then you must add something to the statistic output repository before the checkout.

<details>
  <summary><tt>traffic/clones</tt>/<tt>latest.json</tt>:</summary>
  <pre lang="json">{
  "count" : 0,
  "uniques" : 0,
  "clones" : []
}</pre>
</details>

<details>
  <summary><tt>traffic/clones</tt>/<tt>latest-accum.json</tt>:</summary>
  <pre lang="json">{
  "count_outdated" : 0,
  "uniques_outdated" : 0,
  "count" : 0,
  "uniques" : 0,
  "clones" : []
}</pre>
</details>

<details>
  <summary><tt>traffic/views</tt>/<tt>latest.json</tt>:</summary>
  <pre lang="json">{
  "count" : 0,
  "uniques" : 0,
  "views" : []
}</pre>
</details>
  
<details>
  <summary><tt>traffic/views</tt>/<tt>latest-accum.json</tt>:</summary>
  <pre lang="json">{
  "count_outdated" : 0,
  "uniques_outdated" : 0,
  "count" : 0,
  "uniques" : 0,
  "views" : []
}</pre>
</details>

The `myrepo` repository should contain 1 file per statistic entity:

* traffic/clones:
  [.github/workflows/accum-gh-clone-stats.yml](https://github.com/andry81-devops/github-accum-stats/blob/master/.github/workflows/accum-gh-clone-stats.yml)
  
* traffic/views:
  [.github/workflows/accum-gh-view-stats.yml](https://github.com/andry81-devops/github-accum-stats/blob/master/.github/workflows/accum-gh-view-stats.yml)
  
* traffic/downloads/mypage:
  [.github/workflows/accum-mypage-download-stats.yml example](https://github.com/andry81-devops/gh-action--accum-inpage-downloads#examples)
  
* traffic/board/phpbb:
  [.github/workflows/accum-phpbb-board-stats.yml example](https://github.com/andry81-devops/gh-action--accum-board-stats#examples)
  
* traffic/rate/limits:
  [.github/workflows/accum-gh-rate-limits.yml example](https://github.com/andry81-devops/gh-action--accum-gh-rate-limits#examples)

> :warning: You must replace all placeholder into respective values:

* `{{REPO_OWNER}}` -> repository owner
* `{{REPO}}` -> `myrepo`

After the github workflow yaml file is commited and pushed, you can run the action from the `Actions` tab in the `myrepo` repository.

After that you can add badges to reference a repository statistic:

```
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

## <a name="reuse">REUSE</a>

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

> :information_source: You still have to attach respective `secrets.READ_STATS_TOKEN` token to the repository with workflow scripts to access another repository statistic.

> :information_source: If you have multiple repositories to store the statistic, then you can create a github organization account like `<owner>-stats` and move all statistic repositories into organization's account.
> It will leave the repositories page of the original account untouched on each commit into an organization account repository.

## <a name="known_issues">Known Issues</a>

### `git fetch` error: `could not read Username for 'https://github.com': terminal prompts disabled`

```
Fetching the repository
  /usr/bin/git -c protocol.version=2 fetch --no-tags --prune --progress --no-recurse-submodules --depth=1 origin +refs/heads/master*:refs/remotes/origin/master* +refs/tags/master*:refs/tags/master*
  Error: fatal: could not read Username for 'https://github.com': terminal prompts disabled
  The process '/usr/bin/git' failed with exit code 128
  Waiting 14 seconds before trying again
```

You missed to update the repo `secrets` token.

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

## <a name="known_issues_updates">Last known updates on composite actions features</a>

* Conditionals: https://github.com/actions/runner/issues/834

  * https://github.blog/changelog/2021-11-09-github-actions-conditional-execution-of-steps-in-actions/

    Actions written in YAML, also known as composite actions, now support if conditionals.
