## What is version control?

Processes and systems to manage changes to files, programs, and directories


## What can version control do?

- Track files in different states
- Combine different versions of files
- Identify a particular version
- Revert changes


## Git commands

### Creating a new repo

> git init mental-health-workspace

### Git version

> git --version

## [Conventional Commit Messages](https://gist.github.com/qoomon/5dfcdf8eec66a051ecd85625518cfd13)


### Types
- API or UI relevant changes
    - `feat` Commits, that add or remove a new feature to the API or UI
    - `fix` Commits, that fix a API or UI bug of a preceded `feat` commit
- `refactor` Commits, that rewrite/restructure your code, however do not change any API or UI behaviour
    - `perf` Commits are special `refactor` commits, that improve performance
- `style` Commits, that do not affect the meaning (white-space, formatting, missing semi-colons, etc)
- `test` Commits, that add missing tests or correcting existing tests
- `docs` Commits, that affect documentation only
- `build` Commits, that affect build components like build tool, ci pipeline, dependencies, project version, ...
- `ops` Commits, that affect operational components like infrastructure, deployment, backup, recovery, ...
- `chore` Miscellaneous commits e.g. modifying `.gitignore`



### Description

The description contains a concise description of the change.

- Is a mandatory part of the format
- Use the imperative, present tense: "change" not "changed" nor "changes"
- Think of This commit will... or This commit should...
- **Don't capitalize the first letter**
- **No dot (.) at the end**

### Body

The body should include the motivation for the change and contrast this with previous behavior.

**Is an optional part of the format**
Use the imperative, present tense: "change" not "changed" nor "changes"
This is the place to mention issue identifiers and their relations


### Examples
- ```
  feat: add email notifications on new direct messages
  ```
- ```
  feat(shopping cart): add the amazing button
  ```
- ```
  feat!: remove ticket list endpoint

  refers to JIRA-1337

  BREAKING CHANGE: ticket enpoints no longer supports list all entites.
  ```
- ```
  fix(shopping-cart): prevent order an empty shopping cart
  ```
- ```
  fix(api): fix wrong calculation of request body checksum
  ```
- ```
  fix: add missing parameter to service call

  The error occurred because of <reasons>.
  ```
- ```
  perf: decrease memory footprint for determine uniqe visitors by using HyperLogLog
  ```
- ```
  build: update dependencies
  ```
- ```
  build(release): bump version to 1.0.0
  ```
- ```
  refactor: implement fibonacci number calculation as recursion
  ```
- ```
  style: remove empty line
  ```



What is a Git repo?

Is a storage space where your project files and their revision history are kept. It allows you to track changes to your files over time, collaborate with others, and manage different versions of your project. A Git repo can be hosted locally on your computer or remotely on a server, such as GitHub, GitLab, or Bitbucket.

When you initialize a Git repository, Git creates a hidden directory called `.git` in the root of your project. This directory contains all the metadata and object database for your project, including information about commits, branches, and tags. The `.git` directory is what makes a directory a Git repository.

## Creating a new repo


> git init mental-health-workspace

> cd mental-health-workspace

> git status


![alt text](image.png)


###Git log:

![alt text](image-1.png)

#### Restricting the file

To only look at the commit history of one file:

> git log report.md

#### Finding a particular commit

Only need the first 8-10 characters of the hash

> git show c27fa856

#### git diff

- Compare last committed version of report.md with the version in the staging area

> git diff --staged rep


- Comparing two commits

> git diff 35f4b4d 186398f

- Compare second most recent with the most recent commit

> git diff HEAD~1 HEAD

### Reverting files

> git revert HEAD 

OR

> git revert 5er5a

### Revert a single file

> git checkout 5er5a -- report.md


### Unstaging a single file

> git restore --staged statistics.csv

### Unstaging all files

> git restore --staged


### Listing all branches


> git branch

### Move to the speed-test branch

> git switch speed-test

### Creating a new branch and switch to it

> git switch -c speed-test

### Renaming a branch

> git branch -m feature_dev chatbot


### Deleting a branch

> git branch -d chatbot

OR 

> git branch -D chatbot

For deleting a branch that hasn't been merged


### Diferença entre git checkout e git switch

Principais diferenças:
Comando	Propósito
git checkout	Multifuncional (muda de branch, restaura arquivos, etc.)
git switch	Exclusivo para troca/criação de branches (mais seguro e intuitivo)

### git merge

Une branches localmente, combinando as mudanças de uma branch na outra.

- Usado para integrar mudanças de uma branch em outra.
- Afeta apenas o repositório local.


### Remote repo

No Git, um remote repo (repositório remoto) é um repositório armazenado em um servidor ou em um serviço de hospedagem como GitHub, GitLab ou Bitbucket. Ele serve como um ponto central onde múltiplos desenvolvedores podem colaborar, sincronizando suas mudanças através de comandos como push, pull e fetch.

> git remote -v → Lista os repositórios remotos configurados.

> git remote add <nome> <URL> → Adiciona um novo repositório remoto.

#### Criar um Repositório no GitHub/GitLab/Bitbucket

1. Acesse a plataforma de sua escolha (GitHub, GitLab, Bitbucket).
2. Crie um novo repositório sem inicializar com README, .gitignore ou licença.
3. Copie a URL do repositório remoto.

>> git remote add origin <URL_DO_REPOSITORIO>

>> git remote -v → Para confirmar se o remoto foi adicionado corretamente

>> git push -u origin main


### Diferença entre git fetch e git pull 

- git fetch	Apenas baixa as alterações do repositório remoto, sem aplicá-las na sua branch atual.
- git pull	Baixa as alterações do repositório remoto e as mescla automaticamente na sua branch local.