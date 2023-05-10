# All about SSH Keys


## Generating SSH key with `ed25519` encoding
```bash
ssh-keygen -o -a 100 -t ed25519 -f ~/.ssh/"SSHNAME" -C "EMAIL"
```

## Adding SSH key
```bash
ssh-add ~/.ssh/"SSHNAME" private-key
```

## Starting SSH agent
```bash
eval `ssh-agent -s`
```


ssh-keygen -o -a 100 -t ed25519 -f ~/.ssh/ubuntu -C "julius.yerro@ingrammicro.com"