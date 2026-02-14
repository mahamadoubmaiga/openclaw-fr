---
summary: "Mise à jour sécurisée d'OpenClaw (installation globale ou sources), plus stratégie de retour en arrière"
read_when:
  - Mise à jour d'OpenClaw
  - Quelque chose se casse après une mise à jour
title: "Mise à jour"
---

# Mise à jour

OpenClaw évolue rapidement (avant "1.0"). Traitez les mises à jour comme une infrastructure de déploiement : mettre à jour → exécuter des vérifications → redémarrer (ou utiliser `openclaw update`, qui redémarre) → vérifier.

## Recommandé : réexécuter le programme d'installation du site web (mise à niveau sur place)

Le chemin de mise à jour **préféré** est de réexécuter le programme d'installation depuis le site web. Il
détecte les installations existantes, met à niveau sur place et exécute `openclaw doctor` quand
nécessaire.

```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

Notes :

- Ajoutez `--no-onboard` si vous ne voulez pas que l'assistant d'intégration s'exécute à nouveau.
- Pour les **installations depuis les sources**, utilisez :

  ```bash
  curl -fsSL https://openclaw.ai/install.sh | bash -s -- --install-method git --no-onboard
  ```

  Le programme d'installation exécutera `git pull --rebase` **uniquement** si le dépôt est propre.

- Pour les **installations globales**, le script utilise `npm install -g openclaw@latest` sous le capot.
- Note héritée : `clawdbot` reste disponible en tant que shim de compatibilité.

## Avant de mettre à jour

- Sachez comment vous avez installé : **global** (npm/pnpm) vs **depuis les sources** (git clone).
- Sachez comment votre Passerelle s'exécute : **terminal au premier plan** vs **service supervisé** (launchd/systemd).
- Sauvegardez votre personnalisation :
  - Config : `~/.openclaw/openclaw.json`
  - Identifiants : `~/.openclaw/credentials/`
  - Espace de travail : `~/.openclaw/workspace`

## Mise à jour (installation globale)

Installation globale (choisissez-en un) :

```bash
npm i -g openclaw@latest
```

```bash
pnpm add -g openclaw@latest
```

Nous **ne recommandons pas** Bun pour le runtime de la Passerelle (bugs WhatsApp/Telegram).

Pour changer de canal de mise à jour (installations git + npm) :

```bash
openclaw update --channel beta
openclaw update --channel dev
openclaw update --channel stable
```

Utilisez `--tag <dist-tag|version>` pour une installation ponctuelle tag/version.

Voir [Canaux de développement](/install/development-channels) pour la sémantique des canaux et les notes de version.

Note : sur les installations npm, la passerelle enregistre un conseil de mise à jour au démarrage (vérifie le tag du canal actuel). Désactivez via `update.checkOnStart: false`.

Ensuite :

```bash
openclaw doctor
openclaw gateway restart
openclaw health
```

Notes :

- Si votre Passerelle s'exécute en tant que service, `openclaw gateway restart` est préféré plutôt que de tuer les PIDs.
- Si vous êtes épinglé à une version spécifique, voir "Retour en arrière / épinglage" ci-dessous.

## Mise à jour (`openclaw update`)

Pour les **installations depuis les sources** (git checkout), préférez :

```bash
openclaw update
```

Il exécute un flux de mise à jour sûr :

- Nécessite un arbre de travail propre.
- Bascule vers le canal sélectionné (tag ou branche).
- Récupère + rebase contre l'upstream configuré (canal dev).
- Installe les dépendances, compile, compile l'UI de Contrôle et exécute `openclaw doctor`.
- Redémarre la passerelle (si elle s'exécute en tant que service).

Options :

```bash
openclaw update --channel stable    # passer au canal stable
openclaw update --channel beta      # passer au canal beta
openclaw update --channel dev       # passer au canal dev
openclaw update --skip-restart      # sauter le redémarrage automatique
```

## Retour en arrière / épinglage

Si une mise à jour introduit un problème, revenez à la dernière version connue fonctionnelle.

### Installation globale

Épinglez à une version spécifique :

```bash
npm i -g openclaw@2026.1.15
```

```bash
pnpm add -g openclaw@2026.1.15
```

### Installation depuis les sources

Vérifiez une version étiquetée spécifique :

```bash
cd <dépôt-openclaw>
git fetch --all --tags
git checkout v2026.1.15
pnpm install
pnpm build
openclaw gateway restart
```

## Après la mise à jour

1. Vérifiez la version :
   ```bash
   openclaw --version
   ```

2. Exécutez les diagnostics :
   ```bash
   openclaw doctor
   openclaw health
   ```

3. Vérifiez que la Passerelle fonctionne :
   ```bash
   openclaw status
   openclaw dashboard
   ```

## Problèmes courants

<AccordionGroup>
  <Accordion title="La passerelle ne démarre pas après la mise à jour">
    1. Vérifiez les journaux : `openclaw logs`
    2. Exécutez les diagnostics : `openclaw doctor`
    3. Vérifiez les conflits de configuration : `openclaw config validate`
    4. Si tout échoue, revenez en arrière à la version précédente
  </Accordion>

  <Accordion title="Erreurs de dépendances manquantes">
    Réinstallez les dépendances :
    
    ```bash
    # Installation globale
    npm i -g openclaw@latest --force
    
    # Installation depuis les sources
    cd <dépôt-openclaw>
    rm -rf node_modules
    pnpm install
    pnpm build
    ```
  </Accordion>

  <Accordion title="Erreurs de migration de configuration">
    `openclaw doctor` devrait gérer automatiquement les migrations. Si cela échoue :
    
    1. Sauvegardez votre config : `cp ~/.openclaw/openclaw.json ~/.openclaw/openclaw.json.backup`
    2. Réexécutez doctor : `openclaw doctor --force`
    3. Si cela échoue toujours, restaurez et signalez le problème
  </Accordion>
</AccordionGroup>

## Canaux de développement

OpenClaw a trois canaux de version :

- **stable** : versions étiquetées (`vYYYY.M.D`), npm dist-tag `latest` — recommandé pour la production
- **beta** : versions de pré-sortie (`vYYYY.M.D-beta.N`), npm dist-tag `beta` — nouvelles fonctionnalités, tests
- **dev** : tête mobile de `main`, npm dist-tag `dev` — derniers changements, peut être instable

Voir [Canaux de développement](/install/development-channels) pour plus de détails.

## Besoin d'aide ?

- Documentation complète : [docs.openclaw.ai](https://docs.openclaw.ai)
- Discord : [discord.gg/clawd](https://discord.gg/clawd)
- Problèmes GitHub : [github.com/openclaw/openclaw/issues](https://github.com/openclaw/openclaw/issues)
