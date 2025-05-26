# GIT

## Comandi da terminale

Aggiungere file ad un commit precedente non pushato:

```BASH
git add nomefile
git commit --amend
```

Vedere tutte le branch, sia remote che locali.

```BASH
git branch -a
```

Vedere solo le branch locali

```BASH
git branch -l
```

Vedere solo le branch remote

```BASH
git branch -r
```

Vedere quali branch sono state mergiate dentro *nomebranch*

```BASH
git branch -a --merged nomebranch***
```
