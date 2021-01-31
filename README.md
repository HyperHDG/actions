# GitHub Action: wiki

GitHub action that deploys a wiki (which is administrated within a certain branch) of a repository.

GitHub offers the possibility to write a wiki per repository. This wiki usually is contained in the
repository `your_repo_name.wiki` which automatically publishes its content to the wiki page of the
reposiyory `your_repo_name`. You may checkout and adapt `your_repo_name.wiki` directly or change its
content via the GitHub web interface. However, if you would like to track its history using the
GitHub page and treat `your_repo_name.wiki` like any other of your repositories (including the
options to enforce pull-requests, etc.), this is hardly possible.

The GitHub Action `wiki` provided in this branch closes this gap and allows you to have the content
of your repository's wiki either in an independent repository or a separate branch of your
repository. Whenever your branch/repository is changed the wiki pages are automatically updated.

This can be achieved by adding a GitHub action to the repository/branch containing the wiki pages.
Thus, you need to add a file `deploy_wiki.yml` to the folder `.github/workflow/`. The file might
look like

```
name: Deploy Wiki

on:
  push:
    branches:
    - gh-wiki

jobs:
  deploy_wiki:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout current branch
      uses: actions/checkout@v2
    - name: Run deploy_wiki script
      uses: HyperHDG/actions@wiki
      with:
        ci_repo_token: ${{ secrets.CI_REPO_TOKEN }}
        ci_user_name: ci_account
        ci_user_mail: ci_account_mail
        wiki_repo_organization: repo_name_orga
        wiki_repo_name: repo_name.wiki
```

which deploys the wiki of the repository `repo_name` whose content is stored in the branch
`gh-wiki` (of some---possibly the same---repository). The content of the wiki will be updated,
whenever branch `gh-wiki` is updated (via a push or a merge after a pull-request).

- The first step checks out the repository.

- The second step deploys the wiki to `repo_name.wiki`. Thus, we need writing access to this
  repository. The easiest way to gain it is to create a dummy account `ci_account` with writing
  access only to the repository `repo_name.wiki`. `ci_account_mail` is the email address with which
  the account is associated. Moreover you need to create a personal access token to all repositories
  of `ci_account` and store it as a secret with name `CI_REPO_TOKEN` in your repository. The
  `repo_name_orga` denotes the organization or user that owns `repo_name`.
