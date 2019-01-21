hub(1) -- make git easier with GitHub
=====================================

## Synopsis

`hub` [--noop] <COMMAND> [<OPTIONS>]  
`hub alias` [-s] [<SHELL>]  
`hub help` hub-<COMMAND>

## Description

Hub is a tool that wraps git in order to extend it with extra functionality that
makes it better when working with GitHub.

## Commands

Available commands are split into two groups: those that are already present in
git but that are extended through hub, and custom ones that hub provides.

### Extended git commands

hub-am(1)
:   Replicate commits from a GitHub pull request locally.

hub-apply(1)
:   Download a patch from GitHub and apply it locally.

hub-checkout(1)
:   Check out the head of a pull request as a local branch.

hub-cherry-pick(1)
:   Cherry-pick a commit from a fork on GitHub.

hub-clone(1)
:   Clone a repository from GitHub.

hub-fetch(1)
:   Add missing remotes prior to performing git fetch.

hub-init(1)
:   Initialize a git repository and add a remote pointing to GitHub.

hub-merge(1)
:   Merge a pull request locally with a message like the GitHub Merge Button.

hub-push(1)
:   Push a git branch to each of the listed remotes.

hub-remote(1)
:   Add a git remote for a GitHub repository.

hub-submodule(1)
:   Add a git submodule for a GitHub repository.

### New commands provided by hub

hub-alias(1)
:   Show shell instructions for wrapping git.

hub-browse(1)
:   Open a GitHub repository in a web browser.

hub-ci-status(1)
:   Display GitHub Status information for a commit.

hub-compare(1)
:   Open a GitHub compare page in a web browser.

hub-create(1)
:   Create a new repository on GitHub and add a git remote for it.

hub-delete(1)
:   Delete a repository on GitHub.

hub-fork(1)
:   Fork the current project on GitHub and add a git remote for it.

hub-pull-request(1)
:   Create a GitHub pull request.

hub-pr(1)
:   List and checkout GitHub pull requests.

hub-issue(1)
:   List and create GitHub issues.

hub-release(1)
:   List and create GitHub releases.

hub-sync(1)
:   Fetch from upstream and update local branches.

## Conventions

Most hub commands are supposed to be run in a context of an existing local git
repository. Hub will automatically detect the GitHub repository the current
project belongs to by scanning its git remotes.

In case there are multiple git remotes that are all pointing to GitHub, hub
assumes that the main one is named "upstream", "github", or "origin", in that
order of preference.

When working with forks, it's recommended that the git remote for your own fork
is named "origin" and that the git remote for the upstream repository is named
"upstream". See <https://help.github.com/articles/configuring-a-remote-for-a-fork/>

The default branch (usually "master") for the project is detected like so:

    git symbolic-ref refs/remotes/origin/HEAD

where <origin> is the name of the git remote for the upstream repository.

The destination where the currently checked out branch is considered to be
pushed to depends on the `git config push.default` setting. If the value is
"upstream" or "tracking", the tracking information for a branch is read like so:

    git rev-parse --symbolic-full-name BRANCH@{upstream}

Otherwise, hub scans git remotes to find the first one for which
`refs/remotes/REMOTE/BRANCH` exists. The "origin", "github", and "upstream"
remotes are searched last because hub assumes that it's more likely that the
current branch is pushed to your fork rather than to the canonical repo.

## Configuration

### GitHub OAuth authentication

Hub will prompt for GitHub username & password the first time it needs to access
the API and exchange it for an OAuth token, which it saves in `~/.config/hub`.

To avoid being prompted, use `GITHUB_USER` and `GITHUB_PASSWORD` environment
variables.

Alternatively, you may provide `GITHUB_TOKEN`, an access token with
**repo** permissions. This will not be written to `~/.config/hub`.

### HTTPS instead of git protocol

If you prefer the HTTPS protocol for git operations, you can configure hub to
generate all URLs with `https:` instead of `git:` or `ssh:`:

    $ git config --global hub.protocol https

This will affect `clone`, `fork`, `remote add` and other hub commands that
expand shorthand references to GitHub repo URLs.

### GitHub Enterprise

By default, hub will only work with repositories that have remotes which
point to `github.com`. GitHub Enterprise hosts need to be whitelisted to
configure hub to treat such remotes same as github.com:

    $ git config --global --add hub.host MY.GIT.ORG

The default host for commands like `init` and `clone` is still `github.com`, but
this can be affected with the `GITHUB_HOST` environment variable:

    $ GITHUB_HOST=my.git.org git clone myproject

### Environment variables

`HUB_VERBOSE`
:   Enable verbose output from hub commands.

`HUB_CONFIG`
:   The file path where hub configuration is read from and stored. If
    `XDG_CONFIG_HOME` is present, the default is `$XDG_CONFIG_HOME/hub`;
    otherwise it's `$HOME/.config/hub`. The configuration file is also
    searched for in `XDG_CONFIG_DIRS` per XDG Base Directory Specification.

`HUB_PROTOCOL`
:   Use one of "https|ssh|git" as preferred protocol for git clone/push.

`GITHUB_TOKEN`
:   OAuth token to use for GitHub API requests.

## Bugs

<https://github.com/github/hub/issues>

## Authors

<https://github.com/github/hub/contributors>

## See also

git(1), git-clone(1), git-remote(1), git-init(1),
<https://github.com/github/hub>