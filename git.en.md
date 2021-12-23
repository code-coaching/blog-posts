---
title: What is Git and what is GitHub? Explaining the basics of Git and GitHub!
slug: what-is-git-and-what-is-github-explaining-the-basics-of-git-and-github
tags:
  - Git
  - GitHub
categories:
draft: true
---

Git is a version control system. Nice, but what does that mean? A system to control versions. Also nice, but what does it mean to control versions? To explain this, it is easier to look at an example!

When there is a reference to `terminal` in the text below and Windows is used as an operating system, it means that the command needs to be executed in `Git Bash`. If CTRL+V does not work to paste the copied content in the terminal, try SHIFT+INSERT.

## Installing Git

Perhaps it is already installed. Open a terminal (Linux/macOS) or command prompt (Windows). Type `git --version` and press `Enter`.

```sh
git --version
```

If Git is installed, you should see something like this (numbers can differ): `git version 2.30.2` or `git version 2.30.0.windows.2`.

If Git is not installed, no version will be shown. To install Git:

### [Linux](https://git-scm.com/download/linux)

Execute the following command in a terminal (Ubuntu):

```sh
sudo apt install git
```

### [macOS](https://git-scm.com/download/mac)

Execute the following command in a terminal (macOS):

Installing [Homebrew](https://brew.sh/#install).

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Installing Git.

```sh
brew install git
```

### [Windows](https://gitforwindows.org/)

- Click on Windows above.
- Click on `Download`.
- Double-click on the downloaded file.
- Keep everything default and keep hitting `Next`.
- Click on `Install`.

This will also install `Git Bash`. When there is a reference to `terminal` in the text below and Windows is used as an operating system, it means that the command needs to be executed in `Git Bash`.

## Working with the terminal

The terminal can also be used to navigate the file system of the computer, creating new files and creating new folders. This will also be used to learn how to use Git. Thanks to Git Bash it is possible to use the same commands on Windows as on Linux and macOS.

### pwd

After opening a terminal, it will probably start at the home directory. This is often symbolised by `~`. To know where this directory is, type `pwd` (**p**rint **w**orking **d**irectory) and press `Enter`.

```sh
pwd
```

The output of the command:

- Linux: `/home/JohnDuck`
- macOS: `/Gebruikers/JohnDuck`
- Windows: `/c/Gebruikers/JohnDuck`
  - This is equal to: `C:/Gebruikers/JohnDuck`

Note: Unless JohnDuck is chosen as a user name, this will differ on the local system.

### cd

To be certain that the working directory is the home directory, `cd` (**c**hange **d**irectory) can be used.

```sh
cd ~
```

Note: `~` is the home directory.

Image there is a folder called `projects`. To go to this folder, type `cd projects` and press `Enter`.

```sh
cd projects
```

Note: If there are spaces in the name of the folder, either use quotes or use `\` to escape the spaces.

```sh
cd 'Folder With Spaces In The Name'
```

```sh
cd Folder\ With\ Spaces\ In\ The\ Name
```

Tip: Start typing and press `tab`, this will autocomplete the folder name.

### ls

To know which files and folders are in the current directory, type `ls` (**l**ist **s**ystem) and press `Enter`.

```sh
ls
```

It also accepts arguments.

```sh
ls -a
```

The "-a" stands for "all" argument will show hidden files and folders (files and folders that start with a dot `.`). This will also show `./` and `../`. One dot refers to the current directory. Two dots refer to the parent directory (one directory above the current directory).

### mkdir

To create a new folder, type `mkdir` (**m**ake **d**irectory) and press `Enter`.

```sh
mkdir projects
```

This will create a folder called `projects` in the current directory.

### touch

To create a new file, type `touch`, followed by the name of the file, and press `Enter`.

```sh
touch example.txt
```

This will create a new file called `example.txt` in the current directory.

## Using Git locally

Git is software without a GUI (Graphical User Interface). There is no icon that can be clicked to start the program. Git is a command line interface, which means that it can be used from the command line/terminal. Start by opening a terminal.

Create a new folder called `git-example` in the home directory and change the location to this folder.

```sh
cd ~
```

This will change the location to the home directory.

```sh
mkdir git-example && cd git-example
```

By using `&&` it is possible to execute multiple commands in one command. This will create a new folder called `git-example` and change the location to this folder.

### Making a folder into a Git repo

The word `repo` refers to `repository`. A repository is a folder that contains all the files that are part of a project.

Currently the folder `git-sexample/` is not a Git repo, it is an ordinary folder.

Use `ls -a` to see the files and folders in the current directory, it will not show anything, since the directory is empty.

```sh
ls -a # The output is: ./ ../
```

Create a Git repo by using `git init`.

```sh
git init #  This will output "Initialized empty Git repository in [this part differs per user]/git-example/.git"
```

Use `ls -a`, it will now show the folder `.git`.

```sh
ls -a # Dit heeft als output: ./ ../ .git/
```

Look at the folder `.git/` and see what is inside.

```sh
cd .git/ && ls -a
```

This will have the output: `./ ../ HEAD config description hooks/ info/ objects/ refs/`. It is not necessary to change anything here, it is enough to know that this is the location where Git keeps track of all the changes.

Go up one directory.

```sh
cd ..
```

Changes are recorded in `commits`. A commit is a snapshot of the current state of the repository. To know which changes already happened, `git log` can be used.

```sh
git log # The output: fatal: your current branch 'master' does not have any commits yet
```

Because there are not commits yet, an error message is shown. There is a reference to `branch` in the error message, currently there will only be worked with one branch, named `master`.

### Adding a commit to the Git repo

With `git status` the current status of the Git repo can be checked.

```sh
git status

###
# The output:
# On branch master
#
# No commits yet
#
# nothing to commit (create/copy files and use "git add" to track)
###
```

Executing `git status` tells us that there are no changes yet, there is nothing to `commit` yet.

Create a new file called `README.md`. A `.md` file is a Markdown file. Dit is a text file like a .txt file, but with an added benefit that `Markdown` syntax can be used. In this example the Markdown syntax will not be used, but still there is a reason that this file is called `README.md`. This is the file that will automatically be loaded when visiting a Git repo on the internet.

```sh
touch README.md

```

With `ls` it can be checked that the file now exists in the current directory.

Execute `git status` again. It will still say that we currently are on the master branch `On branch master`. It will still say there is nothing committed yet `No commits yet`. What is different now, is the `untracked files`. This means that there are new files that are not tracked by Git.

```sh
git status

###
# On branch master

# No commits yet

# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#         README.md
#
# nothing added to commit but untracked files present (use "git add" to track)
###
```

To track this file with Git from this point onwards, it is necessary to add it to the Git repo. This is done by using `git add`.

```sh
git add README.md

###
# On branch master
#
# No commits yet
#
# Changes to be committed:
#  (use "git rm --cached <file>..." to unstage)
#        new file:   README.md
###
```

Execute `git status` again. // HIER

Voer opnieuw `git status` uit. Er staat nog altijd dat we op de master branch zitten `On branch master`. Er staat nog altijd dat er niks gecommit is `No commits yet`. Dit is correct want er is nog altijd niks gecommit, maar er wordt wel een nieuw bestand opgevolgd door Git.

Om het aanmaken van dit bestand permanent als wijziging bij te houden in Git, kan er een `git commit` gedaan worden.

<details>
  <summary>Doe dit eenmalig vóór de eerste commit</summary>

```sh
git config user.name "Bart Duisters"
```

Wijzig "Bart Duisters" naar de naam die getoond moet worden bij het toevoegen van wijzigingen.

```sh
git config user.email "bartduisters@bartduisters.com"
```

Wijzig "bartduisters@bartduisters.com" door een eigen e-mailadres.

Tip: Indien er al een GitHub-account aangemaakt is, gebruik hetzelfde e-mailadres. Indien er nog geen GitHub-account aangemaakt is, gebruik het e-mailadres dat later gebruikt gaat worden om een GitHub-account aan te maken.

Dit stelt de gebruikersnaam en het e-mailadres in voor specifiek deze Git repo. Indien altijd dezelfde naam gebruikt moet worden, kan dit één keer globaal gezet worden. Dan zal het op de computer bijgehouden worden en niet specifiek in deze Git repo.

```sh
git config --global user.name "Bart Duisters"
```

```sh
git config --global user.email "bartduisters@bartduisters.com"
```

</details>

```sh
git commit -m "Add README.md"

###
# [master (root-commit) c0aba7a] Add README.md
#  1 file changed, 0 insertions(+), 0 deletions(-)
#  create mode 100644 README.md
###
```

Opmerking: De `c0aba7a` zal verschillend zijn. Dit is het begin van de de `commit hash`, dit is een unieke identificatiecode.

De `-m` (Engels: message, Nederlands: bericht) is een parameter om een bericht mee te geven aan de commit. Er wordt vaak gekozen voor een Engels bericht omdat de meeste Git repos in het Engels zijn, dit is de taal die gebruikt wordt door developers. Het bericht tussen de dubbele quotes kan van alles zijn, het beschrijft best de wijzigingen die aanwezig zijn in de commit.

Er wordt ook een kort overzicht getoond van de wijzigingen `1 file changed, 0 insertions(+), 0 deletions(-)`. Er is één bestand gewijzigd (toegevoegd in dit geval). Er is geen tekst toegevoegd `0 insertions(+)` of verwijderd `0 deletions(-)`.

Voer opnieuw `git status` uit.

```sh
git status

###
# On branch master
# nothing to commit, working tree clean
###
```

Dit geeft aan dat er geen nieuwe wijzigingen meer zijn: `working tree clean`.

### Geschiedenis bekijken

Met `git log` kan de geschiedenis bekeken worden. Alle commits die doorheen de tijd gedaan zijn op deze Git repo zullen getoond worden.

```sh
git log

###
# commit c0aba7a9e06057c325719bac3357faff70ec9d2b (HEAD -> master)
# Author: Bart Duisters <bartduisters@bartduisters.com>
# Date:   Thu Jun 24 22:25:20 2021 +0200
#
#    Add README.md
###
```

Hierin staat de commit message `Add README.md`. De volledige commit hash (de unieke identificatiecode van deze commit) `c0aba7a9e06057c325719bac3357faff70ec9d2b`. De auteur `Bart Duisters <bartduisters@bartduisters.com>` (dit is de waarde die toegekend is aan `user.name` en `user.email` tijdens de `git config`-stappen). Het tijdstip wanneer de commit gedaan is `Thu Jun 24 22:25:20 2021 +0200`.

Open het bestand `README.md` in een teksteditor en voeg de tekst `bla bla bla` toe. Bij het bekijken van `git status` kan gezien worden dat Git de wijziging ziet (omdat `README.md` al opgevolgd wordt, het is een `tracked file`).

```sh
git status

###
# On branch master
# Changes not staged for commit:
#  (use "git add <file>..." to update what will be committed)
#  (use "git restore <file>..." to discard changes in working directory)
#        modified:   README.md
#
# no changes added to commit (use "git add" and/or "git commit -a")
###
```

Er worden hints gegeven wat er gedaan kan worden. Met `git add README.md` kan de wijziging klaargezet worden om uiteindelijk gecommit te worden. Met `git restore README.md` kan de wijziging ongedaan gemaakt worden.

Voeg de wijziging toe. Commit de wijziging en bekijk opnieuw de geschiedenis.

```sh
git add README.md
git commit -m "Add text to README.md"
###
# [master 81f48fc] Add text to README.md
#  1 file changed, 1 insertion(+)
###
```

<details>
  <summary>Windows-gebruikers, klik hier!</summary>

Bij het uitvoeren van een `git add` van een bestand waarin lijnen zijn gewijzigd. Zal er een extra bericht getoond worden.

```sh
###
# warning: LF will be replaced by CRLF in README.md.
# The file will have its original line endings in your working directory
###
```

Unix-besturingssystemen (zoals Linux en macOS) gebruik een LF (Line Feed, ook wel weergegeven als `\n`) om het einde van een lijn aan te geven. Op Windows is dit CRLF (Carriage Return en Line Feed, ook wel weergegeven als `\r\n`). Line Feed en Carriage Return zijn termen gebaseerd op de acties die gedaan moesten worden op een typmachine. Line Feed, een nieuwe regel werd bekomen door het blad fysiek één lijn hoger te positioneren. Carriage Return, het gedeelte dat het blad vastklemt fysiek terug naar het begin bewegen.

Om te zorgen dat de code uniform is wanneer het naar de server overgezet wordt, worden de CRLF naar LF gewijzigd op het moment dat een `git add` wordt uitgevoerd. Wanneer een repo lokaal wordt binnengehaald, zal elke LF omgezet worden in een CRLF.

</details>

Er wordt een nieuwe commit aangemaakt `[master 81f48fc] Add text to README.md` waarbij te zien is dat er één bestand is gewijzigd, waarin één lijn is toegevoegd `1 file changed, 1 insertion(+)`.

```sh
git log

###
# commit 81f48fc07e47f264d5912b56104c20852b819e1d (HEAD -> master)
# Author: Bart Duisters <bartduisters@bartduisters.com>
# Date:   Thu Jun 24 23:12:41 2021 +0200
#
#    Add text to README.md
#
# commit c0aba7a9e06057c325719bac3357faff70ec9d2b
# Author: Bart Duisters <bartduisters@bartduisters.com>
# Date:   Thu Jun 24 22:25:20 2021 +0200
#
#    Add README.md
###
```

De Git log toont alle commits die doorheen de tijd zijn gedaan.

### De kracht van git

Tijdreizen is niet mogelijk. Of toch? Via `git checkout` kan er naar een oudere versie van een bestand gekeken worden. Stel dat er 100 commits gedaan over een tijdspanne van één jaar, waarvan de bovenstaande commits de eerste twee commits zijn.

Om te kijken hoe het bestand eruitzag na de eerste commit, doe een `git checkout` op de Git hash van de eerste commit.

```sh
git checkout c0aba7a9e06057c325719bac3357faff70ec9d2b

###
# Note: switching to 'c0aba7a9e06057c325719bac3357faff70ec9d2b'.
#
# You are in 'detached HEAD' state. You can look around, make experimental
# changes and commit them, and you can discard any commits you make in this
# state without impacting any branches by switching back to a branch.
#
# If you want to create a new branch to retain commits you create, you may
# do so (now or later) by using -c with the switch command. Example:
#
#  git switch -c <new-branch-name>
#
# Or undo this operation with:
#
#  git switch -
#
# Turn off this advice by setting config variable advice.detachedHead to false
#
# HEAD is now at c0aba7a Add README.md
###
```

Dit geeft veel informatie, voorlopig wordt hier niet op ingegaan. Bekijk `README.md` in de teksteditor en merk op dat de `bla bla bla` niet meer aanwezig is. Dit klopt, aangezien na de eerste commit het bestand wel was aangemaakt, maar er was nog geen tekst aan toegevoegd.

Gebruik `git switch -` om niet langer de eerste commit te bekijken.

```sh
git switch -
```

Om te weten wat het verschil is tussen twee commits, kan `git diff <oude commit> <nieuwe commit>` gebruikt worden. Wijzig `<oude commit>` door de commit hash van de oudste commit van de twee, wijzig `<nieuwe commit>` door de commit hash van de nieuwste commit van de twee.

```sh
git diff c0aba7a9e06057c325719bac3357faff70ec9d2b 81f48fc07e47f264d5912b56104c20852b819e1d

###
# diff --git a/README.md b/README.md
# index e69de29..cd591db 100644
# --- a/README.md
# +++ b/README.md
# @@ -0,0 +1 @@
# +bla bla bla
###
```

Hieruit kan afgeleid worden dat `bla bla bla` is toegevoegd.

Hieruit kan ook afgeleid worden dat alles dat ooit is toegevoegd aan de geschiedenis van Git, altijd nog terug bekeken kan worden. Het is dus belangrijk dat er **nooit geheime informatie toegevoegd wordt** aan de bestanden die gecommit worden. Voor software-ontwikkelaars kan dit betekenen `wachtwoorden`, `API-sleutels`, `omgevingsvariabelen` ...

## GitHub - Git online gebruiken

Sinds 13 augustus 2021 is het niet langer mogelijk om met een gebruikersnaam en wachtwoord een repo te pullen en pushen. De makkelijkste manier is om `ssh` te gebruiken. Er is een artikel op dit blog dat gevolgd kan worden om SSH te gebruiken met GitHub.

### Remote repo koppelen aan bestaande lokale repo

Tot nu toe worden de commits bijgehouden in de `.git/`-map en zolang de `.git/`-map op de computer staat, kan er altijd naar oudere versies van bestanden gekeken worden. Maar, stel, er zijn twee toestellen aanwezig (een vaste computer en een laptop) en soms wordt er op het ene toestel gewerkt en soms wordt er op het andere toestel gewerkt. Om te zorgen dat alle wijzigingen en alle oude wijzigingen aanwezig zijn, moet de `.git/`-map aanwezig zijn op beide systemen en deze moet continu in sync gehouden worden. Hetzelfde geldt als er met meerdere samengewerkt wordt op één repo, de `.git/`-map op alle toestellen moet continu in sync gehouden worden.

Top, dus de `.git/`-map in `Google Drive`, `OneDrive` of `Dropbox` steken en zo is de `.git/`-map altijd in sync? Nee! Dé cloudopslag om te gebruiken is [MEGAsync](https://mega.nz/pro/aff=xLXv1HsQvUE) (referral, 20 GB gratis). Dat gezegd zijnde, een Git repo wordt niet in sync gehouden door het in een map op cloudopslag te zetten.

Wat wordt er dan wél gebruikt om een Git repo in sync te houden op verschillende toestellen? Online platformen waar extra functionaliteit voorzien wordt, zoals het aanmaken van `releases`. Een van de meest gebruikte platformen is GitHub. Één van de mogelijke alternatieven is [BitBucket](https://bitbucket.org). Om dit artikel mee te volgen, maak een account op [GitHub](https://github.com/signup).

Eenmaal ingelogd, dan kan er een nieuwe repo aangemaakt worden door op [new](https://github.com/new) te klikken. Vul een `repository name` in, bijvoorbeeld `git-example-remote`. Standaard zal de repo publiek zijn, dit houdt in dat de inhoud van de repo door iedereen bekeken kan worden (ook door mensen zonder account). Indien de inhoud van de repo niet door anderen gezien mag worden, klik dan `private` aan.

Vervolgens zijn er drie checkboxes. Bij het aanmaken van een nieuwe repo die aan een bestaande lokale repo gekoppeld moet worden, is het handig om ze alledrie aan te vinken. Aangezien er lokaal al een repo is, laat ze **alledrie afgevinkt**:

- `Add a README file`, dit kan aangevinkt worden om automatisch een `README.md` aan te maken bij het aanmaken van een repo.

- `Add .gitignore`, dit zal in dit artikel handmatig toegevoegd worden.

- `Choose a license`, standaard is de code **niet** opensource. Dus ook al staat de repo publiek, als er geen LICENSE-bestand aanwezig is, is de code **niet** opensource. Als de code bedoelt is om door anderen gebruikt te worden, is het belangrijk om een opensourcelicentie toe te voegen.

Klik op `Create repository`. Zorg bij het volgen van dit artikel dat er **geen** checkbox is aangevinkt!

Wat dit achterliggend eigenlijk doet, is een `.git/`-map aanmaken op de server van GitHub. Maar nu is er dus een `.git/`-map op het lokale systeem en een aparte `.git/`-map op het systeem van GitHub (remote systeem). Om de lokale Git repo op de hoogte te brengen dat er ook een remote Git repo is, kan er een `remote origin` toegevoegd worden. Dit wordt gedaan vanuit de lokale map in de terminal.

```sh
git remote add origin https://github.com/bartduisters/git-example-remote.git
```

Opmerking: Indien tijdens het aanmaken van de repo dezelfde naam gebruikt is `git-example-remote`, dan moet enkel `bartduisters` uitgewisseld worden door de eigen GitHub-gebruikersnaam. Dit is ook de URL die teruggevonden kan worden in de adresbalk van de browser.

```sh
git branch -M master
```

Git werkt met `branches` (Nederlands: takken), in dit artikel wordt er met één `branch` gewerkt. Deze stap is momenteel niet nodig, maar indien in de toekomst de hoofdtak anders genoemd wordt (`main` wordt tegenwoordig soms gebruikt). Dan wordt het hernoemd naar `master` zodat de andere stappen in dit artikel blijven werken.

Ondertussen weet de lokale Git af van het bestaan van de remote Git. Om te zorgen dat de lokale commits ook in de remote Git aanwezig zijn, kan er gebruik gemaakt worden van `git push`. Aangezien er nog niet eerder gepusht is, zal de eerste keer aangegeven moeten worden op welke remote branch er gepusht moet worden.

```sh
git push -u origin master

###
# Enumerating objects: 6, done.
# Counting objects: 100% (6/6), done.
# Delta compression using up to 12 threads
# Compressing objects: 100% (2/2), done.
# Writing objects: 100% (6/6), 451 bytes | 451.00 KiB/s, done.
# Total 6 (delta 0), reused 0 (delta 0), pack-reused 0
# To https://github.com/bartduisters/git-example-remote.git
# * [new branch]      master -> master
# Branch 'master' set up to track remote branch 'master' from 'origin'.
###
```

Alle toekomstige commits van deze repo kunnen gepusht worden door enkel `git push` uit te voeren.

Ga naar de remote repo op GitHub en ververs de pagina. Hier is de README.md te zien.

Het is ook mogelijk om snelle wijzigingen online te doen:

- Klik op README.md.
- Klik op het bewerkicoontje (rechtsboven 'Edit this file).
- Wijzig `bla bla bla` door `bla John Duck`.
- Scroll naar onder en klik op `Commit changes`.
  - Standaard wordt `Update README.md` gebruikt als commit-bericht.
  - Dit is hetzelfde als `git commit -m "Update README.md"`.

Ga terug naar de hoofdpagina van de repo en klik op `3 commits` (onder de groene knop met `Code`). Hier zijn de eerste twee commits te zien die lokaal gedaan zijn `Add README.md` en `Add text to README.md` en ook de commit die online gedaan is `Update README.md`.

In de lokale terminal, doe een `git log`.

```sh
git log

###
# commit 81f48fc07e47f264d5912b56104c20852b819e1d (HEAD -> master, origin/master)
# Author: Bart Duisters <bartduisters@bartduisters.com>
# Date:   Thu Jun 24 23:12:41 2021 +0200
#
#    Add text to README.md
#
# commit c0aba7a9e06057c325719bac3357faff70ec9d2b
# Author: Bart Duisters <bartduisters@bartduisters.com>
# Date:   Thu Jun 24 22:25:20 2021 +0200
#
#    Add README.md
###
```

Hier is de remote commit `Update README.md` niet te zien.

Open `README.md` in een teksteditor en wijzig `bla bla bla` naar `John Duck lala`. Maak ook een nieuw bestand aan.

```sh
touch info.md
```

Voeg beide bestanden toe aan de lijst om uiteindelijk te committen.

```sh
git add README.md info.md
```

Een alternatieve manier om alle bestanden toe te voegen met wijzigingen in de huidige directory is door gebruik te maken van `.`.

```sh
git add .
```

Commit deze change lokaal.

```sh
git commit -m "Change README.md, add info.md"
###
# [master 5246933] Change README.md, add info.md
#  2 files changed, 1 insertion(+), 1 deletion(-)
#  create mode 100644 info.md
###
```

Bekijk de Git log.

```sh
git log

###
# commit 5246933337d680ac44d6137af58eb5b9f6144176 (HEAD -> master)
# Author: Bart Duisters <bartduisters@bartduisters.com>
# Date:   Fri Jun 25 01:18:47 2021 +0200
#
#   Change README.md, add info.md
#
# commit 81f48fc07e47f264d5912b56104c20852b819e1d (origin/master)
# Author: Bart Duisters <bartduisters@bartduisters.com>
# Date:   Thu Jun 24 23:12:41 2021 +0200
#
#   Add text to README.md
#
# commit c0aba7a9e06057c325719bac3357faff70ec9d2b
# Author: Bart Duisters <bartduisters@bartduisters.com>
# Date:   Thu Jun 24 22:25:20 2021 +0200
#
#   Add README.md
###
```

Nu valt er iets op. Remote zijn er drie commits, waarvan de derde commit `Update README.md` is. Lokaal zijn er drie commits, waarvan de derde commit `Change README.md, add info.md` is.

Ook is er te zien dat de lokale Git de informatie heeft van de remote Git tot aan de tweede commit (`(origin/master)` achter de tweede commit hash).

Push de commits.

```sh
git push

###
# To https://github.com/bartduisters/git-example-remote.git
#  ! [rejected]        master -> master (fetch first)
# error: failed to push some refs to 'https://github.com/bartduisters/git-example-remote.git'
# hint: Updates were rejected because the remote contains work that you do
# hint: not have locally. This is usually caused by another repository pushing
# hint: to the same ref. You may want to first integrate the remote changes
# hint: (e.g., 'git pull ...') before pushing again.
# hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

Dit geeft een **dikke vette error**. Er wordt gezegd dat de push gefaald is, dat er werk is op de remote repo die niet aanwezig is in de lokale repo. En dit is correct, `Change README.md, add info.md` is wél aanwezig op de remote repo en niet op de lokale repo. Ook wordt er gezegd dat er een `git pull` uitgevoerd kan worden om de remote commits lokaal te krijgen.

```sh
git pull

###
# remote: Enumerating objects: 5, done.
# remote: Counting objects: 100% (5/5), done.
# remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
# Unpacking objects: 100% (3/3), 639 bytes | 63.00 KiB/s, done.
# From https://github.com/bartduisters/git-example-remote
#    81f48fc..db5955a  master     -> origin/master
# Auto-merging README.md
# CONFLICT (content): Merge conflict in README.md
# Automatic merge failed; fix conflicts and then commit the result.
###
```

Godverd... Een merge conflict! Git kan veel wijzigingen zelf samenvoegen. Een merge conflict kan voorkomen als er remote wijzigingen gedaan zijn in een bestand op eenzelfde lijn waar lokaal ook wijzigingen zijn gedaan. Er staat `Merge conflict in README.md`.

Bij het bekijken van `git status` is meer informatie terug te vinden.

```sh
git status

###
# On branch master
# Your branch and 'origin/master' have diverged,
# and have 1 and 1 different commits each, respectively.
#  (use "git pull" to merge the remote branch into yours)
#
# You have unmerged paths.
#  (fix conflicts and run "git commit")
#  (use "git merge --abort" to abort the merge)
#
# Unmerged paths:
#  (use "git add <file>..." to mark resolution)
#        both modified:   README.md
#
# no changes added to commit (use "git add" and/or "git commit -a")
###
```

Aangezien er nog meer commits gaan komen in de toekomst is `git merge --abort` geen oplossing. Maar `fix conflicts and run "git commit"` is wel een optie.

Open het bestand `README.md` waarin het conflict zich bevindt. Hier is plots totale chaos terug te vinden.

```text
<<<<<< HEAD
John Duck lala
=======
bla John Duck
>>>>>> db5955a1a7bb936c7fee06a365203177e721d254
```

Opmerking: Normaal staat er één `<` en één `>` extra.

Alles tussen `<<<<<<< HEAD` en `=======` is de lokale verandering.
Alles tussen `=======` en `>>>>>>> [git hash]` is de remote verandering.

Het is aan de developer om te beslissen wat moet blijven. Indien `John Duck lala` het enige is dat moet blijven, dan moet `<<<<<<< HEAD`, `=======`, `bla John Duck` en `>>>>>>> [git hash]` verwijderd worden.

Resultaat na het oplossen van het merge conflict.

```
John Duck lala
```

Aangezien er een verandering is gedaan, moet deze verandering worden toegevoegd aan de lijst van gewijzigde bestanden om op een later moment te kunnen committen.

```sh
git add README.md
```

Normaal moet er een commit-bericht toegevoegd worden aan een `git commit`-commando. Maar doordat de repo door een moeilijke periode in zijn leven gaat (merge conflict), is het voldoende om enkel `git commit` te typen. Dit zal automatisch de standaard teksteditor openen. Het voegt standaard een commit-bericht toe. Het is voldoende om het bestand te sluiten in de teksteditor en de commit zal toegevoegd worden.

Hint: Indien er plots een teksteditor geopend wordt in de terminal, dan zal dit waarschijnlijk `Nano` of `Vim` zijn.

<details>
  <summary>Nano: Indien er onderaan `^X` en `^R` te zien zijn ... Klik hier!</summary>
  De terminal teksteditor Nano heeft zich geopend. `^` staat voor `CTRL` en `X` staat voor de toets `x`. Klik CTRL + X om Nano af te sluiten. Indien er nog een vraag gesteld wordt, typ `y` en klik op enter.
</details>

<details>
  <summary>Vim: Indien er onderaan géén `^X` en `^R` te zien zijn ... Klik hier!</summary>
  De terminal teksteditor Vim heeft zich geopend. Dit is een teksteditor met verschillende modi. Indien er per ongeluk van modus is gewijzigd, klik op `Escape`. Om uit Vim te geraken typ `:w!` en klik op enter.
</details>

Bekijk opnieuw de Git log.

```sh
git log

###
# commit 9d204398f076c28ddd997c56f4b70ec6556bc3c1 (HEAD -> master)
# Merge: 5246933 db5955a
# Author: Bart Duisters <bartduisters@bartduisters.com>
# Date:   Fri Jun 25 01:46:41 2021 +0200
#
#    Merge branch 'master' of https://github.com/bartduisters/git-example-remote
#
# commit 5246933337d680ac44d6137af58eb5b9f6144176
# Author: Bart Duisters <bartduisters@bartduisters.com>
# Date:   Fri Jun 25 01:18:47 2021 +0200
#
#    Change README.md, add info.md
#
# commit db5955a1a7bb936c7fee06a365203177e721d254 (origin/master)
# Author: Bart Duisters <43420049+bartduisters@users.noreply.github.com>
# Date:   Fri Jun 25 01:07:05 2021 +0200
#
#    Update README.md
#
# commit 81f48fc07e47f264d5912b56104c20852b819e1d
# Author: Bart Duisters <bartduisters@bartduisters.com>
# Date:   Thu Jun 24 23:12:41 2021 +0200
#
#    Add text to README.md
#
# commit c0aba7a9e06057c325719bac3357faff70ec9d2b
# Author: Bart Duisters <bartduisters@bartduisters.com>
# Date:   Thu Jun 24 22:25:20 2021 +0200
#
#    Add README.md
###
```

Er zijn **vijf** commits te zien. De twee eerste, de derde is de remote commit, de vierde is de lokale commit en tenslotte de vijde is de merge commit. Ook is te zien dat `(origin/master)` niet langer achter de tweede commit staat, maar wel achter de derde. De gewone `master` staat op de vijfde commit. Hieruit is af te leiden dat de lokale repo twee commits voorligt op de remote repo.

Het uitvoeren van `git status` bevestigt dit.

```sh
git status
# On branch master
# Your branch is ahead of 'origin/master' by 2 commits.
#  (use "git push" to publish your local commits)
#
# nothing to commit, working tree clean
```

Er is te lezen `Your branch is ahead of 'origin/master' by 2 commits.`. Het is al gekend wat er gedaan kan worden om commits van een lokale repo naar een remote repo te krijgen, namelijk `git push`.

```sh
git push
```
