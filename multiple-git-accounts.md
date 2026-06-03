# Using multiple git accounts on the same local instance 

Here we setup two git accounts on the same local machine which use different key files. To chose the correct key file, we use `[includeIf]` directive of git and different folder for each account.

### Step 1: create two keys

```bash 
ssh-keygen -f ~/.ssh/bob -C'bob@bob.com'
ssh-keygen -f ~/.ssh/alice -C'alice@alice.com'
```

### Step 2: create two folders for each user

```bash
mkdir ~/bob
mkdir ~/alice
```

### Step 3: define main gitconfig

In the main gitconfig we define a default user and refer to per-folder (which is equivalent to per user) gitconfigs.

```bash

$ cat ~/.gitconfig

[user]
        name = john
        email = john@john.com
[includeIf "gitdir:~/bob"]
        path = ~/.gitconfig-bob
[includeIf "gitdir:~/.gitconfig-alice"]
        path = ~/.gitconfig-alice
```

### Step 4: define each account gitconfig

```bash

$ cat ~/.gitconfig-bob
[user]
        name = "bob"
        email = "bob@bob.com"
[core]
        sshCommand = ssh -i ~/.ssh/bob -o IdentitiesOnly=yes


$ cat ~/.gitconfig-alice
[user]
        name = "alice"
        email = "alice@alice.com"
[core]
        sshCommand = ssh -i ~/.ssh/alice -o IdentitiesOnly=yes
```

### Step 4: create repos in user folder and push 

Any repo created under ~/alice or ~/bob directory will use the user's matching key. For example

```bash
$ ls ~/bob

repo1
repo2

$ cd repo1 && git push origin master 
```

The last command will use ~/.ssh/bob key. 

**END**


