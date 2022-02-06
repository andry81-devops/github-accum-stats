<!-- -->
<p align="center">
  <a href="#"><img src="https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2Fandry81%2Fgithub-accum-stat&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false" valign="middle" alt="hits" /></a>
</p>
<!-- -->

<p align="center">
  <a href="https://github.com/andry81/github-accum-stats--gh-stats/commits/master/traffic/views">
    <img src="https://img.shields.io/badge/dynamic/json?color=success&label=Github%20views|all&query=count&url=https://github.com/andry81/github-accum-stats--gh-stats/raw/master/traffic/views/latest-accum.json?raw=True&logo=github" valign="middle" alt="GitHub views|any|total" />
    <img src="https://img.shields.io/badge/dynamic/json?color=success&label=14d&query=count&url=https://github.com/andry81/github-accum-stats--gh-stats/raw/master/traffic/views/latest.json?raw=True" valign="middle" alt="GitHub views|any|14d" /></a>
• <a href="https://github.com/andry81/github-accum-stats--gh-stats/commits/master/traffic/views">
    <img src="https://img.shields.io/badge/dynamic/json?color=success&label=Github%20views|unq&query=uniques&url=https://github.com/andry81/github-accum-stats--gh-stats/raw/master/traffic/views/latest-accum.json?raw=True&logo=github" valign="middle" alt="GitHub views|unique per day|total" />
    <img src="https://img.shields.io/badge/dynamic/json?color=success&label=14d&query=uniques&url=https://github.com/andry81/github-accum-stats--gh-stats/raw/master/traffic/views/latest.json?raw=True" valign="middle" alt="GitHub views|unique per day|14d" /></a>
</p>

<p align="center">
  <a href="https://github.com/andry81/github-accum-stats--gh-stats/commits/master/traffic/clones">
    <img src="https://img.shields.io/badge/dynamic/json?color=success&label=Github%20clones|all&query=count&url=https://github.com/andry81/github-accum-stats--gh-stats/raw/master/traffic/clones/latest-accum.json?raw=True&logo=github" valign="middle" alt="GitHub clones|any|total" />
    <img src="https://img.shields.io/badge/dynamic/json?color=success&label=14d&query=count&url=https://github.com/andry81/github-accum-stats--gh-stats/raw/master/traffic/clones/latest.json?raw=True" valign="middle" alt="GitHub clones|any|14d" /></a>
• <a href="https://github.com/andry81/github-accum-stats--gh-stats/commits/master/traffic/clones">
    <img src="https://img.shields.io/badge/dynamic/json?color=success&label=Github%20clones|unq&query=uniques&url=https://github.com/andry81/github-accum-stats--gh-stats/raw/master/traffic/clones/latest-accum.json?raw=True&logo=github" valign="middle" alt="GitHub clones|unique per day|total" />
    <img src="https://img.shields.io/badge/dynamic/json?color=success&label=14d&query=uniques&url=https://github.com/andry81/github-accum-stats--gh-stats/raw/master/traffic/clones/latest.json?raw=True" valign="middle" alt="GitHub clones|unique per day|14d" /></a>
</p>

---

> This implementation is based on that: https://github.com/andry81/github-clone-count-badge

With additional features:

1. Can count traffic clones or/and views.
2. Can use GitHub composite action to reuse workflow code base: https://docs.github.com/en/actions/creating-actions/creating-a-composite-action

> :warning: Not all features of a generic GitHub action is supported: https://github.com/actions/runner/issues/646

>> **What does Composite Run Steps Not Support**
>>
>> We don't support setting conditionals, continue-on-error, timeout-minutes, "uses", and secrets on individual steps within a composite action right now.
>>
>> (Note: we do support these attributes being set in workflows for a step that uses a composite run steps action)

You need 4 repositories:

1. Repository which clone statistic you want to track: `myrepo`.
   > :information_source: See <a href="#reuse">REUSE</a> section for details if you have multiple repositories and want to store all workflow scripts in a single repository.
2. Repository, where clone statistic will be saved: `myrepo--gh-stats`.
   > :information_source: You still can use a single repository to request and to store, but it is not convenient and will distort the clone statistic (at least until the GitHub action user who is used to checkout the statistic output repository won't be involved into clones counter change), so is not recommended.
3. Repository, where to store github workflow scripts: `gh-workflow`.<br>
   You can fork it from here: https://github.com/andry81/gh-workflow
4. Repository, where to store github composite action: `gh-action--accum-gh-stats`.<br>
   You can fork it from here: https://github.com/andry81/gh-action--accum-gh-stats

   There is other actions you can use (`gh-action--*`):
   https://github.com/andry81?tab=repositories&q=gh-action--

You need to attach a personal access token (PAT) into the repository being requested for statistic and obtain the push permission (`repo`->`public_repo`):

* `myrepo` -> `READ_STATS_TOKEN`

> :information_source: The `myrepo--gh-stats` repository does not require a separate PAT token as long as it is owned by the same repository owner.

To generate PAT: https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token

To attach PAT: https://docs.github.com/en/actions/reference/encrypted-secrets#creating-encrypted-secrets-for-a-repository

The `myrepo--gh-stats` repository should contain 2 files per each statistic entity:

`traffic/clones/latest.json`:

```
{
  "count" : 0,
  "uniques" : 0,
  "clones" : []
}
```

`traffic/clones/latest-accum.json`:

```
{
  "count_outdated" : 0,
  "uniques_outdated" : 0,
  "count" : 0,
  "uniques" : 0,
  "clones" : []
}
```

`traffic/views/latest.json`:

```
{
  "count" : 0,
  "uniques" : 0,
  "views" : []
}
```

`traffic/views/latest-accum.json`:

```
{
  "count_outdated" : 0,
  "uniques_outdated" : 0,
  "count" : 0,
  "uniques" : 0,
  "views" : []
}
```

The `myrepo` repository should contain 1 file per statistic entity:

* traffic/clones:
  [.github/workflows/accum-gh-clone-stats.yml](https://github.com/andry81/github-accum-stats/blob/master/.github/workflows/accum-gh-clone-stats.yml)
* traffic/views:
  [.github/workflows/accum-gh-view-stats.yml](https://github.com/andry81/github-accum-stats/blob/master/.github/workflows/accum-gh-view-stats.yml)

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

## REUSE<a name="reuse"></a>

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

## Known Issues<a name="known_issues"></a>

### `git fetch` error: `could not read Username for 'https://github.com': terminal prompts disabled`

```
Fetching the repository
  /usr/bin/git -c protocol.version=2 fetch --no-tags --prune --progress --no-recurse-submodules --depth=1 origin +refs/heads/master*:refs/remotes/origin/master* +refs/tags/master*:refs/tags/master*
  Error: fatal: could not read Username for 'https://github.com': terminal prompts disabled
  The process '/usr/bin/git' failed with exit code 128
  Waiting 14 seconds before trying again
```

You missed to update the repo `secrets` token.

### The GitHub composite action has supported not all features of a generic GitHub action: https://github.com/actions/runner/issues/646

> **What does Composite Run Steps Not Support**
>
> We don't support setting conditionals, continue-on-error, timeout-minutes, "uses", and secrets on individual steps within a composite action right now.
>
> (Note: we do support these attributes being set in workflows for a step that uses a composite run steps action)
