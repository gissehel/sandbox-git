## Equivalence and default values

All the following examples starts from this configuration (HEAD points to aa, which follow origin/aa):

```
A---B---C---M---N---O---P---Q---R master, origin/master
# Implicit arguments and gotchas

All examples presupose that **WWW** is following upstream **origin/WWW**

## Simple example

If you're on branch **WWW**, and you execute:
```
git rebase XXX YYY --onto ZZZ
```

It will first `git checkout YYY` and then rebase all nodes from **YYY** history that are not in **XXX** history starting from **ZZZ**. The branch **YYY** is moved.

**Note** that **WWW** is totally ignored.

## Only one argument to rebase

If you're on branch **WWW**, and you execute:
```
git rebase XXX --onto ZZZ
```

This is equivalent to `git rebase XXX WWW --onto ZZZ`.

it will rebase all nodes from **WWW** history that are not in **XXX** history starting from **ZZZ**. The branch **WWW** is moved.

## No --onto provided

If you're on branch **WWW**, and you execute:
```
git rebase XXX YYY
```

This is equivalent to `git rebase XXX YYY --onto XXX`.
It will first `git checkout YYY` and then rebase all nodes from **YYY** history that are not in **XXX** history starting from **XXX**. The branch **YYY** is moved.

**Note** that **WWW** is totally ignored.

## Only one argument to rebase and no --onto provided

If you're on branch **WWW**, and you execute:
```
git rebase XXX
```

This is equivalent to `git rebase XXX WWW --onto XXX`.

## No argument to rebase, but --onto provided

If you're on branch **WWW**, and you execute:
```
git rebase --onto ZZZ
```

This is equivalent to `git rebase origin/WWW WWW --onto ZZZ`.
It will rebase all nodes from **WWW** history that are not in **origin/WWW** history starting from **ZZZ**. The branch **WWW** is moved.

**Note**: Really double check that this is what you intended, as what you may really want is `git rebase ZZZ`.

**Note**: In doubt, **always prefer to specify all arguments**.
