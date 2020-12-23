# Common operations of Git

### Commit code

```bash
git add .
git commit -m "feat: xxxxxx"
```

### How to revoke after commit

```bash
git reset --soft HEAD^
```

This successfully revoked the commit. Note that only the commit operation is
withdrawn, and the code modification remains.

#### git reset parameters

`--mixed`
do not delete the workspace to change the code, cancel the commit, and cancel
the `git add .` operation. This is the default parameter.

`--soft`
do not delete the workspace to change the code, cancel the commit, and do not
cancel the `git add .`.

`--hard`
delete workspace change code, revoke commit, revoke `git add .`. After
completing this operation, it is restored to the last commit state.