---
title: "The Joy of Using SSH"
date: 2025-08-16
tags: ["SSH", "Productivity", "Linux"]
description: "A walkthrough of my SSH workflow, why it feels good to use, and some resources if you want to dive deeper."
listed: false
---

When I first started working, I had to SSH into servers all the time — checking logs, restarting services, changing configurations, you name it.  
To do that, we had to go through a bastion server first, and only then hop into the actual application servers. As a fresh graduate, I naturally looked around to see how my colleagues were managing this.  

The answer? A **Notepad++ tab** with all the commands, copy-pasted into the terminal.  
Every bastion login needed our Active Directory password. We had multiple SSH keys — some for infrastructure, some for applications — which we had to copy over manually, either via `scp` or by creating files directly on the bastion. And of course, the first attempt always failed because the key permissions were too open. Then came the guessing game of which `chmod` permissions were needed. Finally — we’re in.  

And if Infosec or DevOps decided to replace the bastion servers? We had to do it all over again. To make things worse, during an AWS account migration, we ended up with almost 10 different bastions. You get the picture — lots of wasted time. And in production incidents, under pressure, this workflow just wasn’t sustainable.  

For my own sanity, I had to fix this.  

---

## Problems and Solutions

### Problem: Entering the bastion password every time
**Solution: Use SSH keys**  

The SSH protocol is brilliant — generate a personal public/private key pair, and with the magic of asymmetric cryptography, everything just works.  
You can even use the same keys for GitHub/Bitbucket to clone repos over SSH.  

Steps (high-level):  
- Generate an SSH keypair  
- Add it to your SSH agent  
- Install it on your server with `ssh-copy-id`  

Voilà — no more typing credentials every time you connect.  

---

### Problem: Bastion server DNS names are impossible to remember
**Solution: SSH config file**  

From here on, everything revolves around the SSH config file.  

We can create aliases for frequently accessed servers. Example:  

```
Host accounta-dev-us-west
  HostName your-very-long-dns.com

Host accounta-dev-eu-east
  HostName your-very-long-dns.com

Host accountb-prod-us-west
  HostName your-very-long-dns.com

Host accountb-prod-eu-east
  HostName your-very-long-dns.com
```

Now you can simply run:  

```

ssh accounta-dev-us-west

```

You can also add usernames and keys per host:  

```

Host accounta-dev-us-west
  HostName your-very-long-dns.com
  User login-username
  IdentityFile ~/.ssh/id_ed25519

```

So far: aliases + key-based auth = fast, painless bastion access.  

---

### Problem: Copying keys to servers is a pain (and insecure)
**Solution: ProxyJump**  

Instead of copying keys to bastions, keep them on your machine and hop through the bastion using `ProxyJump`:  

```sh
Host accounta-dev-us-west
  HostName your-very-long-dns.com
  User login-username
  IdentityFile ~/.ssh/id_ed25519

Host app-server
  HostName app-server-ip
  User app-user
  ProxyJump accounta-dev-us-west
  IdentityFile ~/.ssh/app-server-key.pub
````

```
ssh app-server
```

This way, your keys never leave your machine, and you avoid long `ssh` commands and constant permission issues.

---

### Problem: Application server IPs keep changing

**Solution: Override with `-o`**

If you don’t have DNS entries or your servers keep getting redeployed, override `HostName` at runtime:

```sh
ssh app-server -o 'HostName=172.168.2.3'
```

---

### Problem: Database servers only accessible via SSH tunnel

**Solution: LocalForward**

All our DBs were behind bastions, requiring SSH tunnels. Sure, you can configure tunnels directly in tools like DBeaver or TablePlus, but doing that across 10 bastions — and across multiple tools — is messy.

Instead, configure the tunnel in your SSH config:

```sh
Host cassandra-accounta-dev-us-west
  HostName your-very-long-dns.com
  User login-username
  IdentityFile ~/.ssh/id_ed25519
  LocalForward 9042 cassandra-node.dev.example.com:9042
  RequestTTY no
```

Run it like this:

```
ssh -N cassandra-accounta-dev-us-west
```

Now, your DB client just connects to `localhost:9042`. Clean and simple.

(I tried reusing the bastion config with ProxyJump for tunnels, but couldn’t get it working cleanly. If you figure that out, let me know!)

---

## Wrapping Up

That’s how I turned SSH from a painful chore into something I actually enjoy using.

Here are some resources I leaned on while figuring this out (written by people far more capable than me, and worth reading as guides):

* [SSH Config — ssh.com](https://www.ssh.com/academy/ssh/config)
* [Using the SSH Config File — linuxize.com](https://linuxize.com/post/using-the-ssh-config-file/)

I also leaned on ChatGPT quite a bit — for asking configuration questions and even to help edit this article.

If you have improvements, suggestions, or cool SSH tricks of your own, reach out! I’d love to learn from how others are using SSH configs in their workflows.
