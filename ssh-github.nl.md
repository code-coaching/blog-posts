---
postUuid: 067a4533-cc1a-41ae-b3e0-b5e9097f9f17
title: SSH gebruiken om te werken met GitHub
slug: ssh-gebruiken-om-te-werken-met-github
tags:
  - SSH
  - GitHub
categories:
youtubeIds:
  - kMyiY70nzRg
---

Wanneer er verwezen wordt naar `terminal` in onderstaande tekst en er wordt met Windows gewerkt, dan moet het uitgevoerd worden in `Git Bash`. Indien CTRL+V niet werkt om gekopieerde inhoud te plakken in de terminal, probeer SHIFT+INSERT.

## Wat is SSH?

Het SSH-protocol is een manier om een toestel te authenticeren bij een server of service. **S**ecure **SH**ell is een netwerkprotocol om op een veilige manier met een ander systeem te connecteren over een onbeveiligde verbinding.

## Waarom SSH gebruiken met GitHub?

Sinds 13 augustus 2021 is het niet meer mogelijk om te authenticeren met gebruikersnaam en wachtwoord. Er zijn twee opties om te authenticeren: een persoonlijke token of SSH.

Om met SSH te werken moet er een sleutelpaar gegenereerd worden:

- een privésleutel, deze mag **nooit** gedeeld worden
- een publieke sleutel, deze mag wel gedeeld worden

## Genereren van SSH-sleutels

```sh
ssh-keygen -t ed25519 -C "john@duck.com"
```

Opmerking: Wijzig `"john@duck.com"` door het e-mailadres dat gebruikt is om de GitHub-account aan te maken!

Er zal een melding verschijnen om een locatie te kiezen om de sleutels op te slaan, klik hier simpelweg op `Enter` om de standaardlocatie te gebruiken.
Er zal een melding komen om een `passphrase` (Nederlands: wachtwoord) in te geven. Geef het wachtwoord in (de ingetypte letters verschijnen niet op het scherm, maar worden wel ingevoerd).
Geef het wachtwoord nogmaals in (de ingetypt letters worden opnieuw niet getoond, maar worden wel ingevoerd).

## Toevoegen van de publieke sleutel aan GitHub

Indien de standaarlocatie is gekozen, kan onderstaand commando gekopieerd en geplakt worden om de inhoud van het bestand met de publieke sleutel uit te printen.

```sh
cat ~/.ssh/id_ed25519.pub
```

Kopieer de uitgeprinte regel `ssh-ed25519 [hier staan allerlei letters en cijfers] [hier staat het e-mailadres]`.

Ga naar de [GitHub-pagina om SSH-sleutels](https://github.com/settings/keys) toe te voegen (klik op account rechtsboven, vervolgens op `Settings`, vervolgens op `SSH and GPG keys`).

Klik op `New SSH key` (groene knop). Vul een titel naar keuze in en plak de gekopieerde sleutel in het veldje genaamd `Key`.

Klik vervolgens op `Add SSH key`.

## Clonen van een repo met SSH

Bij een repository klik je op de groene knop `Code`. In de popup die verschijnt zijn er drie tabbladen:

- `HTTPS`: dit was de manier om met gebruikersnaam en wachtwoord te clonen en tegenwoordig met de persoonlijke tokens.
- `SSH`: dit is wat nodig is om te clonen met ssh, kopieer de `git@github.com:hier-nog-wat-tekst.git` om vervolgens `git clone git@github.com:hier-nog-wat-tekst.git` uit te voeren in de terminal.
- `GitHub CLI`: dit is een manier om met repos te werken op GitHub.

Er wordt gekozen voor SSH omdat deze manier voor alle platformen werkt (GitHub, BitBucket, Gitlab etc.).
