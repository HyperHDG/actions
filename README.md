# GitHub Action: doxygen

GitHub action that builds and deploys a doxygen to create automatically updated online manuals.

GitHub offers the opportunity to publish web pages. This can be used to publish the doxygen of your
software project. If you want to publish the doxyen of the latest push (or merged pull request to
the branch `main` of your repository, you can proceed as follows:

- Create a repository to contain the doxygen pages `auto_pages` with your account. Enable the GitHub
  pages in its `Settings` and use the `main` branch to be published.

- Create a dummy GitHub account `dummy_acc` and give it write access to `doxy_pages`. The email
  address with which the account ins created is `dummy_acc_mail`. Afterwards, create a personal
  access token of `dummy_acc` providing read and write access to all repositories.

- Store the personal access token as secret `CI_REPO_TOKEN` in the repository of which the doxygen
  will be created.

- Add a file `doxygen.yml` to your repository's folder `.github/workflows`. This file might contain

```
name: Doxygen

on: push

jobs:
  make_and_deploy_doxygen:
    name: Make and deploy doxygen
    runs-on: ubuntu-20.04
    steps:
    - name: Checkout current branch
      uses: actions/checkout@v2
      with:
        submodules: recursive
    - name: Run build and deploy action
      uses: HyperHDG/actions@doxygen
      with:
        deploy_doxygen: ${{ github.ref == 'refs/heads/main' }}
        doxygen_command: "make doxygen"
        ci_repo_token: ${{ secrets.CI_REPO_TOKEN }}
        ci_user_name: dummy_acc
        ci_user_mail: dummy_acc_mail
        pages_repo_organization: your_user_name
        pages_repo_name: auto_pages
        doxygen_path: doxygen/html
```

where `your_user_name` is the user/organization which owns the `auto_pages`, `make doxygen` is the
command which builds the doxygen if you are in the main directory of the rspository, and the 
`doxygen_path` us the local path to the html files of the doxygen.

The script will then try to build the doxygen on all pushes and check, whether this can be done, but
it will only be published if there is a successful push or merged pull-request to branch `main`.

For a similar action with different manual, please refer to the branch `wiki` of this repository.
