# Add issue id hook

Git commit hook for adding related JIRA issue ids to commit messages.

It supports [conventional commits](https://www.conventionalcommits.org/en/v1.0.0/) by adding issue id after the type.

## How does it work?
Issue id is parsed from the current branch name and prepended to your commit message.


    $ git checkout -b EXAMPLE-123-new-feature
    $ git add some_code.py
    $ git commit -m "Added some pretty code."
    $ git log
        ...
        EXAMPLE-123 Added some pretty code.

Here's the specification generated from ``spec.py`` tests:

#### AddIssueIdHook:
 - prepends issue id from branch name to commit message
 - prepends first matching issue id to commit message if multiple ids in branch name exist
 - prepends issue id from branch name to commit message after type
 - prepends issue id from branch name to commit message after type with no space
 - prepends issue id from branch name to multiline commit message
 - prepends issue id to commit message when message contains a different issue id
 - doesnt modify commit message if in detached HEAD state
 - doesnt modify commit message if issue id is in anywhere in commit message
 - doesnt modify commit message if issue id not in branch name
 - doesnt modify commit message if it already contains this issue id
 - supports aborting a commit by exiting from editor without making changes
 - supports aborting a commit by providing an empty message

## Installation

### Manual
1. Copy ``commit-msg`` file into ``.git/hooks/`` directory of your project's repository.
1. Make sure the ``commit-msg`` file has execution mode flag set (``chmod +x commit-msg``).

This plugin requires having Python 3.x installed (comes pre-installed on OS X and Ubuntu). Verified against versions 3.4 and 3.12.

### One-liner

```bash
cd my-repo
curl https://raw.githubusercontent.com/kkocel/add-issue-id-hook/master/commit-msg -o ./.git/hooks/commit-msg && chmod +x ./.git/hooks/commit-msg
```

### Global installation

It's possible to apply this hook to every newly cloned or initialized repository using git's [template directory](http://git-scm.com/docs/git-init#_template_directory) feature:

1. Create a directory for your git templates and put this ``commit-msg`` script into it, e.g. into ``~/.git-templates/hooks/commit-msg``.
2. Configure git to use your template directory when initializing repositories: ``git config --global init.templatedir ~/.git-templates``.

### Customizations

Customizations are supported by opening your copy of ``commit-msg`` file and adjusting the configuration variables.

#### JIRA project key pattern

By default, issue ids matching the default JIRA project key pattern are discovered. You can customize the project key pattern or explicitly specify the project key to look for by adjusting ``project_format`` variable.

#### Not a JIRA project?

JIRA issue ids are supported by default, but the hook can support any other issue id patterns. Simply change the ``issue_pattern`` variable to a regular expression that matches the issue ids from your ticket system.

## Known limitations
As this commit hook depends on parsing the current branch name, it won't work when committing in detached HEAD state, e.g. when doing a *reword* operation during ``git rebase --interactive``.

