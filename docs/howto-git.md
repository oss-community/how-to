# Git

---

In order to download and setup git please visit its [website](https://git-scm.com).

### Windows

Download [Git](https://git-scm.com/download/win) installer, then install the setup as a Windows installer.

### Linux

[Guidance](https://git-scm.com/download/linux) of how to download and install for all linux distributions.

```shell
# ubuntu
apt-get install git
# Fedora
dnf install git
```

### Test Git

```shell
git --version
```

---

## Git and GitHub

To begin, create a repository in [GitHub](https://github.com) then, init repository in your local machine in order to
push to GitHub. It is possible to replace an organization name instead of the username in origin URL.

```shell
git init
# optional
# echo "# exclude files" > .gitignore
# echo "# Guide" > README.md
git add .
git commit -m "initializing repository"
git branch -M main
git remote add origin https://github.com/username/repository.git
git push -u origin main
```

---

## GitHub CLI

Visit [GitHub CLI](https://github.com/cli/cli) home page to see full guidance of install.

### Windows

<p align="justify">

Download zip/msi file from [download page](https://github.com/cli/cli/releases) and if you download zip file then
execute the following commands.

</p>

```shell
mkdir C:\sdk\gh
tar -xvf %HOMEPATH%\Downloads\gh_*_windows_*.zip --directory C:\sdk\gh --strip-components=1
set GH_HOME=C:\sdk\gh
setx /M GH_HOME C:\sdk\gh
setx /M PATH "%PATH%;%GH_HOME%\bin"
```

### Linux

```shell
# ubuntu
curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | tee /etc/apt/sources.list.d/github-cli.list > /dev/null
apt-get update
apt-get -y install gh
```

### Test GitHub CLI

```shell
gh –-version
```

### Login

<p align="justify">

In order to login via GitHub CLI (gh), at first generate a
[token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)
by GitHub then create a text file named `token.txt` and copy the token from GitHub and paste in the `token.txt`after
that login with the following command.

</p>

```shell
   gh auth login -p ssh -h github.com --with-token < token.txt
   gh repo list
```

Generate ssh keys by git tool (select one solution).

```shell
  #solution 1: interactive mode
  ssh-keygen -t rsa -C "comment"
```

```shell
  #solution 2: without prompt
  ssh-keygen -t rsa -C "comment" -N '' -f ~/.ssh/id_rsa
```

Deploy public keys via GitHub CLI (select one solution). Owner is a username or organization name.

```shell
  #solution 1: deploy key into repository
  gh repo deploy-key add ~/.ssh/id_rsa.pub -R owner/repository-name -t key-title -w
  ssh -T git@github.com
```

```shell
  #solution 2: deploy key into user profile
  gh ssh-key add ~/.ssh/id_rsa.pub
  ssh -T git@github.com
```

For delete the public key via GitHub CLI.

```shell
gh repo deploy-key delete $(gh repo deploy-key list -R owner/repository-name | awk '{print ($1==""?"":$1)}') -R owner/repository-name || true

```
