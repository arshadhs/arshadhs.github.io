+++
title = "SSH key for GitHub account"
date = 2018-11-07T21:45:00
draft = false
tags = ["GitHub", "Git"]
categories = ["tutorials", "git", "github"]
+++

# Adding a SSH key to GitHub account

```ls -al ~/.ssh```

Generate a key
```ssh-keygen -t rsa -b 4096 -C <user mail id>
eval $(ssh-agent -s)
ssh-add ~/.ssh/id_rsa```

Clip or copy manually
```clip < ~/.ssh/id_rsa.pub
cat ~/.ssh/id_rsa.pub```

Go to GitHub, add the public key to settings -
```https://github.com/settings/keys```
