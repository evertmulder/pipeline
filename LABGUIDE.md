# Creating a CI-CD pipeline

## Register as a new user

Open de gitlab url, kies een gebruikersnaam en wachtwoord. Kies voor het mail adres <username>@local. Het mailadres wordt verder niet gebruikt.

Na het registreren ben je ingelog op gitlab.

## Create new project
Maak een nieuw project aan, geef het een naam.

Voeg wat code toe aan je project. In dit geval een stukje python en een unit test.
Dit kan via de web interface, maar kan ook via een git client.

Via de UI door upload file.

## Toevoegen van een pipeline
Ga naar de repository een kies de knop 'setup CI/CD'. Er wordt een bestand gemaakt `.gitlab-ci.yml`. Dit is de pipeline beschrijving.

Paste hierin de volgende configuratie
```yaml
stages:
  - test
  - deploy

test1:
  stage: test
  script: 
    - python3 Person.py
    
deploy1:
  stage: deploy
  script:
    - echo "Do your deploy here"
    - echo "Tell ansible to deploy this release to a server"

```

Vraag: Enig idee hoe de pipeline eruit ziet en wat het doet?

Controleer bij CI / CD --> Pipelines of er een pipe line gelopen heeft. Bekijk de output van elke stap.

Vraag: Waar is de code uit stap test1 uitgevoerd?

## parallelle uitvoer
We gaan nu een extra test stap toevoegen die parallel loopt met de bestaand test stap.

voeg de een extra stap toe aan de `.gitlab-ci.yml`

```yaml
test2:
  stage: test
  script: 
    - python3 PersonTest.Test
```
Controleer bij CI / CD --> Pipelines of er een pipe line gelopen heeft. Bekijk de output van elke stap.

## Artifact
Het opslaan van een release

voeg de een extra stap toe aan de `.gitlab-ci.yml`

```yaml
build1:
  stage: build
  script:
    - echo "Do your build here"
    - zip release.zip Person.py
  artifacts:
    paths:
    - release.zip
    expire_in: 1 week
```

pas ook de stages aan in de `.gitlab-ci.yml`

```yaml
stages:
  - test
  - build
  - deploy
```

Controleer bij CI / CD --> Pipelines of er een pipe line gelopen heeft. Bekijk de output van elke stap.
Kun je nu ook een release downloaden?


## Werken met Merge Request
We gaan het project zo inrichten dat er alleen via Merge Request code kan worden toegevoegd.

Ga naar Settings->Repository->Protected Branches. Zoek de protected branch master en pas de settings aan naar "merge": Maintainers en "push": No one.

Er worden nu alleen merges vanuit branches toegestaan. Je kunt dit test door te proberen de README.md aan te passen en als branch naar master te gebruiken. Dit gaat niet lukken. Alle changes moeten nu via een merge request.

pas nu de `.gitlab-ci.yml` aan zodat de deploy stap alleen gebeurd bij een merge naar master


```yaml
deploy1:
  stage: deploy
  script:
    - echo "Do your deploy here"
    - echo "Tell ansible to deploy this release to a server"
  only:
    - master
```

Commit en submit merge request. Is de deploy stap uitgevoerd?
Merge de merge request. Wordt de merge request nu wel uitgevoerd?

## Extra opdrachten
Als je een git client gebruikt moet je eerst een aantal stappen doorlopen.

1. SSH key pair maken en key aan profiel toevoegen
2. Een clone maken van de repository
3. Een branch aanmaken (via client of via gitlab)
4. Switch naar de branch
5. Maak aanpassingen
6. Commit de code
7. Push de code
8. Ga naar Gitlab. Maak een Merge request.
9. Controleer of de pipeline gelopen heeft
10. Merge
11. Controleer of de pipeline gelopen heeft

