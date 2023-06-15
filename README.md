# Introduction

Our vision is to create a range of digital services to support the project-lifecycle LCA needs,
that can work as one shared data platform for multiple stakeholders.

# Getting Started

To get started please make sure that the following pieces of software are installed on your machine.

## Windows

* [WSL](https://docs.microsoft.com/en-us/windows/wsl/install-win10)
* [Docker](https://docs.docker.com/desktop/windows/install/)
* [Minikube](https://minikube.sigs.k8s.io/docs/start/)
* [Skaffold](https://skaffold.dev/docs/install/#standalone-binary)
* Python 3.11
* [pipenv](https://pipenv.pypa.io/en/latest/#install-pipenv-today)
* [pre-commit](https://pre-commit.com/#installation)

## Linux

* [Docker](https://docs.docker.com/engine/install/ubuntu/)
* [Minikube](https://minikube.sigs.k8s.io/docs/start/)
* [Skaffold](https://skaffold.dev/docs/install/#standalone-binary)
* Python 3.11
* [pipenv](https://pipenv.pypa.io/en/latest/#install-pipenv-today)
* [pre-commit](https://pre-commit.com/#installation)

**Setup local Python environment**

```shell
pre-commit install
```

# Git submodules

This repo contains [git submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules). When cloning repos with
submodules, one can add `--recurse-submodules` to the `git clone` command, to automatically initialize and update each
submodule in the repo, i.e.:

`git clone --recurse-submodules <repo-url>`

If you already cloned the repo and forgot `--recurse-submodules`, you can `cd` to the cloned repo and obtain the same
results by running:

`git submodule update --init --recursive`

After running either of these commands, you will have initialized, fetched and checked out any nested submodules. The
following section contains descriptions on how to config your local git environment to work more smoothly with
submodules.

## Additional config to work smoothly with git submodules

All the `git config <...>` commands in this section can be run with either `--local` (which is the default)
or `--global` prefixes depending on your preferences for your local git setup.

### Get an overview of changes to submodule

When running `git status`, Git will also show you a short summary of changes to your submodules, if you run the
following:

`git config status.submodulesummary 1`

### Pulling submodule changes from remote (updating local submodules)

To update our local version of the submodules in this project with latest changes of *all* remote submodules, run the
following:

`git submodule update --remote`
Afterwards, you can commit and push the incoming submodule changes like any other git change - make sure to verify that
your local changes function with the submodule changes that you just pulled.

If you're only interested in updating one of the submodules, you can simply pass the name of the relevant submodule,
i.e. `git submodule update --remote "libs/utils"`

### Pushing submodule changes

To ensure that your awesome local changes to any submodules are available to other developers, make sure you configure
git to _automatically try pushing submodule changes_:

`git config push.recurseSubmodules on-demand`

This also ensures that if your local submodule changes fail to be pushed for some reason, the main project push will
also fail.

### Submodule aliases - to speed up your CLI workflow

As suggested in the git submodules guide, setting
up [git aliases](https://git-scm.com/book/en/v2/Git-Tools-Submodules#:~:text=may%20be%20useful.-,Useful%20Aliases,-You%20may%20want)
can come in very handy. A good example of setting up a git alias for a command that you will find yourself running a lot
is `git config --local alias.subupdate "submodule update --init --recursive"`. By running this command you add a local (
could be global if you prefer) alias to your gitconfig, allowing you to run the submodule update command
with `git subupdate`.

# Folder Structure

```plaintext
modules/  # Contains our modules
    documentation/  # Documentation module (git submodule)
    project/  # Project module (git submodule)
    router/  # API Gateway implemented with Apollo Router
pyproject.toml  # Config for Black and ISort
.pre-commit-config.yaml  # Config for Pre-Commit
skaffold.yaml  # Skaffold config for running all backends
```

# Run and Test

**Setup local `.env`**
Copy the contents of `.env.example` to a local `.env` file.
Populate the values and source .env before starting Skaffold
To set the content of the .env file as env vars run `export $(grep -v '^#' .env | xargs)`

## Copy GraphQL schemas

If it is your first time running Skaffold then you have to initialize the Graphql schemas for the Router.

```shell
mkdir -p modules/schemas/project
mkdir -p modules/schemas/documentation

cp modules/project/graphql/schema.graphql modules/router/schemas/project
cp modules/documentation/graphql/schema.graphql modules/router/schemas/documentation

```

## Run

```shell
# Make sure Minikube is running
minikube start

# Set env var
export ARTIFACTS_TOKEN_BACKEND_PACKAGES=<YOUR_PAT>

# Start Skaffold
skaffold dev
```

## Test

Running tests should be in each individual module. See README there.

* [Documentation](./modules/documentation/README.md)
* [Project](./modules/project/README.md)
* [Router](./modules/router/README.md)

# License

Unless otherwise described, the code in this repository is licensed under the Apache-2.0 License. Please note that some
modules, extensions or code herein might be otherwise licensed. This is indicated either in the root of the containing
folder under a different license file, or in the respective file's header. If you have any questions, don't hesitate to
get in touch with us via [email](mailto:molio@molio.dk).
