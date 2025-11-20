# Generating a new SSH key
1. Open Terminal
2. Paste the text below
```shell
ssh-keygen -t ed25519 -C "your_email@example.com"
```
3. you can press Enter to accept the default setting
4. file location
```shell
> Enter a file in which to save the key (/Users/YOU/.ssh/id_ALGORITHM): [Press enter]
```
- Change **id_ALGORITHM** to your file's name
5. Config file
- First, check to see if your ~/.ssh/config file exists in the default location.
```shell
open ~/.ssh/config
```
- If the file doesn't exist, create the file.
```shell
touch ~/.ssh/config
```
- Open your ~/.ssh/config file, then modify the file to contain the following lines
```shell
Host github.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519
```
6. Add your SSH private key to the ssh-agent and store your passphrase in the keychain
```shell
ssh-add --apple-use-keychain ~/.ssh/id_ALGORITHM
```
7. Add the SSH public key to your account on GitHub.  
- Copy ssh key
```shell
pbcopy < ~/.ssh/id_ALGORITHM.pub
```
- visit github.com -> Your profile -> SSH keys -> new ssh key -> paste your ssh key
# Multiple SSH Keys settings for different github account
## Create different public key
- for example, 2 keys created at:
```shell
~/.ssh/id_rsa_user1
~/.ssh/id_rsa_user2
```
## Modify the ssh config
```shell
#user1 account
Host github.com-activehacker
	HostName github.com
	User git
	IdentityFile ~/.ssh/id_rsa_user1

#user2 account
Host github.com-jexchan
	HostName github.com
	User git
	IdentityFile ~/.ssh/id_rsa_user2
```
## Clone you repo and modify your Git config
- modify git config
```shell
git config user.name "user1"
git config user.email "user1@gmail.com"

git config user.name "user2"
git config user.email "user2@gmail.com"
```
- global git config
```shell
git config --global user.name "user1"
```
```shell
git config --global user.email "user1@gmail.com"
```

## Clone a repository, just do:

```shell
 git clone git@pqhuy87it-GitHub:pqhuy87it/project1.git /path/to/project1
```

## Change the URL of origin

```shell
 git remote set-url origin git@pqhuy87it-GitHub:pqhuy87it/flutter_project_samples.git
```

## Reference
- https://gist.github.com/jexchan/2351996
- https://gist.github.com/oanhnn/80a89405ab9023894df7

# Source Tree
```shell
# --- Sourcetree Generated ---
Host pqhuy87it-GitHub
	HostName github.com
	IdentityFile /Users/phamhuy/.ssh/pqhuy87it-GitHub
	IdentitiesOnly yes
# ----------------------------

# --- Sourcetree Generated ---
Host tungnh-expand-GitHub
	HostName github.com
	IdentityFile /Users/phamhuy/.ssh/tungnh-expand-GitHub
	IdentitiesOnly yes
# ----------------------------
```
