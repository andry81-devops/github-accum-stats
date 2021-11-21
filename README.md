<!-- -->
<p align="center">
  <a href="#"><img src="https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2Fandry81%2Fgithub-accum-stat&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false" valign="middle" alt="hits" /></a>
</p>
<!-- -->

---

> This implementation is based on that: https://github.com/andry81/github-clone-count-badge

With additional features:

1. Can count traffic clones or/and views.
2. Can use GitHub composite action to reuse workflow code base: https://docs.github.com/en/actions/creating-actions/creating-a-composite-action

You need 4 repositories:

1. Repository which clone statistic you want to track: `myrepo`
2. Repository, where clone statistic will be saved: `myrepo--gh-stats`
3. Repository, where to store github workflow scripts: `gh-workflow`.<br>
   You can fork it from here: https://github.com/andry81/gh-workflow
4. Repository, where to store github composite action: `gh-action--accum-stats`.<br>
   You can fork it from here: https://github.com/andry81/gh-action--accum-stats

You need to attach a personal access token (PAT) into the repository being requested for statistic and obtain the push permission (`repo`->`public_repo`):

* `myrepo` -> `SECRET_TOKEN`

> :information_source: The `myrepo--gh-stats` repository does not require a separate PAT token as long as it is owned by the same repository owner.

To generate PAT: https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token

To attach PAT: https://docs.github.com/en/actions/reference/encrypted-secrets#creating-encrypted-secrets-for-a-repository

The `myrepo--gh-stats` repository should contain 1 file per each statistic entity:

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

:warning: You must replace all placeholder into respective values:

* `{{REPO_OWNER}}` -> repository owner
* `{{REPO}}` -> `myrepo`

After the github workflow yaml file is commited and pushed, you can run the action from the `Actions` tab in the `myrepo` repository.

After that you can add badges to reference a repository statistic:

```
<p align="center">
  <a href="https://github.com/{{REPO_OWNER}}/{{REPO}}--gh-stats/commits/master/traffic/clones">
    <img src="https://img.shields.io/badge/dynamic/json?color=success&label=Github%20clones|all&query=count&url=https://github.com/{{REPO_OWNER}}/{{REPO}}--gh-stats/raw/master/traffic/clones/latest-accum.json?raw=True&logo=github" valign="middle" alt="GitHub clones|all" />
  </a>
• <a href="https://github.com/{{REPO_OWNER}}/{{REPO}}--gh-stats/commits/master/traffic/clones">
    <img src="https://img.shields.io/badge/dynamic/json?color=success&label=Github%20clones|unq&query=uniques&url=https://github.com/{{REPO_OWNER}}/{{REPO}}--gh-stats/raw/master/traffic/clones/latest-accum.json?raw=True&logo=github" valign="middle" alt="GitHub clones|unique per day" />
  </a>
</p>
```
