# GIT

## Comandi da terminale

### Aggiungere file ad un commit precedente non pushato

```BASH
git add nomefile
git commit --amend
```

### Vedere tutte le branch, sia remote che locali

```BASH
git branch -a
```

### Vedere solo le branch locali

```BASH
git branch -l
```

### Vedere solo le branch remote

```BASH
git branch -r
```

### Vedere quali branch sono state mergiate dentro *nomebranch*

```BASH
git branch -a --merged nomebranch***
```

## Pattern

## Aggiungere file ad un commit locale passato

**Nota:** Fare attenzione a non modificare o rimuovere file che non esistevano al tempo del commit con cui vogliamo interagire. Assicurarsi che il commit sia locale e non remoto

Stashare i file da committare:
`git stash`

Mostrare gli ultimi N commit (in questo esempio gli ultimi 3):
`git rebase -i HEAD~3`

Si aprirà l'editor con la lista degli ultimi 3 commit

```bash
pick a1b2c3d First commit
pick d4e5f6g Second commit
pick h7i8j9k Third commit
```

Editare la riga con il commit da modificare sostituendo `pick` ad `edit`, poi salvare e chiudere il file

```bash
pick a1b2c3d First commit
edit d4e5f6g Second commit
pick h7i8j9k Third commit
```

Ora ripristiniamo i file in stash

`git stash pop`

Aggiungiamo i file al commit usando `amend`

```bash
git add -A
git commit --amend
```

Continuamo e terminiamo il rebase

`git rebase --continue`
