This repository is just a sandbox with a tree of branches in order test and demonstrate merge/branch/rebase

Here the history like a command `git log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(white)%s%C(reset) %C(dim white)%C(reset)%C(bold yellow)%d%C(reset)' --all` would display:

```
* R - Commit (R)  (HEAD -> master, origin/master)
* Q - Commit (Q)
* P - Commit (P)
| * U - Commit (U)  (origin/cc, cc)
| * T - Commit (T)
| * S - Commit (S)
|/
* O - Commit (O)
* N - Commit (N)
* M - Commit (M)
| * L - Commit (L)  (origin/bb, bb)
| * K - Commit (K)
| * J - Commit (J)
| | * I - Commit (I)  (origin/aa, aa)
| | * H - Commit (H)
| | * G - Commit (G)
| |/
| * F - Commit (F)
| * E - Commit (E)
| * D - Commit (D)
|/
* C - Commit (C)
* B - Commit (B)
* A - Initial file commit (A)
```

Here the history like you would find in git man pages

```
A---B---C---M---N---O---P---Q---R master
         \            \
          \             S---T---U cc
           \
            D---E---F---J---K---L bb
                      \
                        G---H---I aa
```
