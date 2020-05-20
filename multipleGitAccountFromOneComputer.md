# Setup multiple Git accounts from one computer

## where github user info is stored?

### remove credentials via Keychain Access

Open "Keychain Access" app and delete githu credentials there.

### command line

```
$ git credential-osxkeychain erase
host=github.com
protocol=https
> [Press Return]
```


## Add first github account

### generate the keys

```
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

### edit ~/.ssh/config

```
Host *
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_rsa
```

### start the agent

```
$ eval "$(ssh-agent -s)"
Agent pid 25068
```

### add the key

```
$ ssh-add -K ~/.ssh/id_rsa_warmbit
```

### Add the key to Github settings->SSH keys

Copy the key first, then paste the key to the settings
```
$ pbcopy < ~/.ssh/id_rsa_warmbit.pub 
```

### Testing

```
$ ssh -T git@github.com
The authenticity of host 'github.com (140.82.112.4)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'github.com,140.82.112.4' (RSA) to the list of known hosts.
Hi warmbit! You've successfully authenticated, but GitHub does not provide shell access.
```

## Modify first account

### Try the following after modify .ssh/config

```
$ ssh-add -D
All identities removed
$ ssh-add id_rsa_warmbit
Identity added: id_rsa_warmbit (warmbit@gmail.com)
$ ssh -T git@github.com
Hi warmbit! You've successfully authenticated, but GitHub does not provide shell access.
```

## Use either of them by config

```
$ git config --global user.name "warmbit" 
$ git config --global user.email "warmbit@gmail.com"
```

## clone with ssh

```
git clone git@github.com:account1_name/repo_name.git
```

## modify .ssh/config with this

```
[remote "origin"]
        url = git@github.com-account1_name:account1_name/repo_name.git
```
