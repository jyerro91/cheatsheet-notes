---
title: SSH Keys
date: 2023-05-17
tags: [ssh-keys, cli, linux, snippets]
---

# SSH Keys
---



#### Generating SSH key with `ed25519` encoding
```bash
ssh-keygen -o -a 100 -t ed25519 -f ~/.ssh/"SSHNAME" -C "EMAIL"
```

#### Starting ssh-agent
```bash
eval `ssh-agent -s`
```

#### Adding ssh-key
```bash
ssh-add ~/.ssh/"SSHNAME" private-key
```
