# Rebase

## Rebase by example

```
A---B---C---M---N---O---P---Q---R HEAD -> master, origin/master
         \            \
          \             S---T---U cc, origin/cc
           \
            D---E---F---J---K---L bb, origin/bb
                      \
                        G---H---I aa, origin/aa
```

```sh
git checkout cc
```

```
A---B---C---M---N---O---P---Q---R master, origin/master
         \            \
          \             S---T---U HEAD -> cc, origin/cc
           \
            D---E---F---J---K---L bb, origin/bb
                      \
                        G---H---I aa, origin/aa
```

```sh
git rebase bb aa --onto master
```

```
                                   G'--H'--I' HEAD -> aa
                                  /
A---B---C---M---N---O---P---Q---R             master, origin/master
         \            \
          \             S---T---U             cc, origin/cc
           \
            D---E---F---J---K---L             bb, origin/bb
                      \
                        G---H---I             origin/aa

```

```sh
git checkout -B aa origin/aa
```

```
A---B---C---M---N---O---P---Q---R master, origin/master
         \            \
          \             S---T---U cc, origin/cc
           \
            D---E---F---J---K---L bb, origin/bb
                      \
                        G---H---I HEAD -> aa, origin/aa
```

```sh
git rebase cc aa --onto master
```

```
                                   D'--E'--F'--G'--H'--I' HEAD -> aa
                                  /
A---B---C---M---N---O---P---Q---R                         master, origin/master
         \            \
          \             S---T---U                         cc, origin/cc
           \
            D---E---F---J---K---L                         bb, origin/bb
                      \
                        G---H---I                         origin/aa

```

## Equivalence and default values

All the following examples starts from this configuration (HEAD points to aa, which is following upstream origin/aa):

```
A---B---C---M---N---O---P---Q---R master, origin/master
         \            \
          \             S---T---U cc, origin/cc
           \
            D---E---F---J---K---L bb, origin/bb
                      \
                        G---H---I HEAD -> aa, origin/aa
```

All the following lines are equivalent (It will put G',H',I' after R with aa pointing to I'):
```
git rebase bb aa --onto master
git rebase bb    --onto master
```

All the following lines are equivalent (It will put D',E',F',G',H',I' after R with aa pointing to I'):
```
git rebase cc aa --onto master
git rebase cc    --onto master
git rebase master aa --onto master
git rebase master    --onto master
```

# Implicit arguments and gotchas

All the following examples starts from this configuration (HEAD points to aa, which is following upstream origin/aa):

```
A---B---C---M---N---O---P---Q---R master, origin/master
         \            \
          \             S---T---U cc, origin/cc
           \
            D---E---F---J---K---L bb, origin/bb
                      \
                        G---H---I HEAD -> aa, origin/aa
```

## Simple example

```
git rebase cc bb --onto master
```

It will first `git checkout bb` and then rebase all nodes from **bb** history that are not in **cc** history starting from **master**. The branch **bb** is moved.

**Note** that **aa** is totally ignored.

```
                                    D'--E'--F'--J'--K'--L' HEAD -> bb
                                  /
A---B---C---M---N---O---P---Q---R                          master, origin/master
         \            \
          \             S---T---U                          cc, origin/cc
           \
            D---E---F---J---K---L                          origin/bb
                      \
                        G---H---I                          aa, origin/aa
```

## Only one argument to rebase

```
git rebase cc --onto master
```

This is equivalent to `git rebase cc aa --onto ZZZ`.

It will rebase all nodes from **aa** history that are not in **cc** history starting from **master**. The branch **aa** is moved.

```
                                    D'--E'--F'--G'--H'--I' HEAD -> aa
                                  /
A---B---C---M---N---O---P---Q---R                          master, origin/master
         \            \
          \             S---T---U                          cc, origin/cc
           \
            D---E---F---J---K---L                          bb, origin/bb
                      \
                        G---H---I                          origin/aa
```

## No --onto provided

```
git rebase cc bb
```

This is equivalent to `git rebase cc bb --onto cc`.

It will first `git checkout bb` and then rebase all nodes from **bb** history that are not in **cc** history starting from **cc**. The branch **bb** is moved.

**Note** that **aa** is totally ignored.

```
A---B---C---M---N---O---P---Q---R                          master, origin/master
         \           \
          \           \             D'--E'--F'--J'--K'--L' HEAD -> bb
          /            \          /
          \             S---T---U                          cc, origin/cc
           \
            D---E---F---J---K---L                          origin/bb
                      \
                        G---H---I                          aa, origin/aa
```

## Only one argument to rebase and no --onto provided

```
git rebase cc
```

This is equivalent to `git rebase cc aa --onto cc`.

```
A---B---C---M---N---O---P---Q---R                          master, origin/master
         \           \
          \           \             D'--E'--F'--G'--H'--I' HEAD -> aa
          /            \          /
          \             S---T---U                          cc, origin/cc
           \
            D---E---F---J---K---L                          bb, origin/bb
                      \
                        G---H---I                          origin/aa
```

## No argument to rebase, but --onto provided

```
git rebase --onto cc
```

This is equivalent to `git rebase origin/aa aa --onto cc`.

It will rebase all nodes from **aa** history that are not in **origin/aa** history starting from **cc**. The branch **aa** is moved.

**Note**: Really double check that this is what you intended, as what you may really want is `git rebase cc`.

```
A---B---C---M---N---O---P---Q---R master, origin/master
         \            \
          \             S---T---U HEAD -> aa, cc, origin/cc
           \
            D---E---F---J---K---L bb, origin/bb
                      \
                        G---H---I origin/aa
```

If you start from this:

```
A---B---C---M---N---O---P---Q---R             master, origin/master
         \            \
          \             S---T---U             cc, origin/cc
           \
            D---E---F---J---K---L             bb, origin/bb
                      \
                        G---H---I             origin/aa
                                  \
                                    V---W---X HEAD -> aa
```

You'll endup with:

```
A---B---C---M---N---O---P---Q---R             master, origin/master
         \            \
          \             S---T---U             cc, origin/cc
          /                       \
          \                         V---W---X HEAD -> aa
           \
            D---E---F---J---K---L             bb, origin/bb
                      \
                        G---H---I             origin/aa
```

**Note**: In doubt, **always prefer to specify all arguments**.
