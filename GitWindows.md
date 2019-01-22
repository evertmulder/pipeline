# Git client op windows

### Git client
Installeer Git for windows (http://git-scm.com/) Dit installeert ook een git-bash client die we gaan gebruiken om een ssh key te genereren


## SSH Key toevoegen (per user)

De volgende commando's uitvoeren in de **git bash** client.

```
git config --global user.name "Voornaam Achternaam"
git config --global user.email "voornaam@local"
ssh-keygen -t rsa -C "voornaam@local"
clip < ~/.ssh/id_rsa.pub
```
Accepteer de default locatie van de gegenereerde public key. Vul geen passphrase in.

Het clip commando kopieert je public key in je paste buffer. Ga naar gitlab, log hier in, ga naar Settings (rechtsboven bij je profiel), SSH Keys en plak hier je public key.

Je bent nu klaar om een repo te clone op de client:
Vanuit een command prompt:
```
mkdir git
cd git
git clone ssh://git@<repourl>.git
```
Beantwoord de vraag over de 'authenticity of the host' met 'yes'.
