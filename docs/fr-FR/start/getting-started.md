---
summary: "Installez OpenClaw et exécutez votre premier chat en quelques minutes."
read_when:
  - Configuration initiale depuis zéro
  - Vous voulez le chemin le plus rapide vers un chat fonctionnel
title: "Démarrage"
---

# Démarrage

Objectif : passer de zéro à un premier chat fonctionnel avec une configuration minimale.

<Info>
Chat le plus rapide : ouvrez l'UI de Contrôle (pas besoin de configuration de canal). Exécutez `openclaw dashboard`
et chattez dans le navigateur, ou ouvrez `http://127.0.0.1:18789/` sur l'
<Tooltip headline="Hôte de la passerelle" tip="La machine exécutant le service de passerelle OpenClaw.">hôte de la passerelle</Tooltip>.
Documentation : [Tableau de bord](/web/dashboard) et [UI de Contrôle](/web/control-ui).
</Info>

## Prérequis

- Node 22 ou plus récent

<Tip>
Vérifiez votre version de Node avec `node --version` si vous n'êtes pas sûr.
</Tip>

## Configuration rapide (CLI)

<Steps>
  <Step title="Installer OpenClaw (recommandé)">
    <Tabs>
      <Tab title="macOS/Linux">
        ```bash
        curl -fsSL https://openclaw.ai/install.sh | bash
        ```
        <img
  src="/assets/install-script.svg"
  alt="Processus du script d'installation"
  className="rounded-lg"
/>
      </Tab>
      <Tab title="Windows (PowerShell)">
        ```powershell
        iwr -useb https://openclaw.ai/install.ps1 | iex
        ```
      </Tab>
    </Tabs>

    <Note>
    Autres méthodes d'installation et prérequis : [Installation](/install).
    </Note>

  </Step>
  <Step title="Exécuter l'assistant d'intégration">
    ```bash
    openclaw onboard --install-daemon
    ```

    L'assistant configure l'authentification, les paramètres de la passerelle et les canaux optionnels.
    Voir [Assistant d'intégration](/start/wizard) pour plus de détails.

  </Step>
  <Step title="Vérifier la Passerelle">
    Si vous avez installé le service, il devrait déjà être en cours d'exécution :

    ```bash
    openclaw gateway status
    ```

  </Step>
  <Step title="Ouvrir l'UI de Contrôle">
    ```bash
    openclaw dashboard
    ```
  </Step>
</Steps>

<Check>
Si l'UI de Contrôle se charge, votre Passerelle est prête à l'emploi.
</Check>

## Vérifications et extras optionnels

<AccordionGroup>
  <Accordion title="Exécuter la Passerelle au premier plan">
    Utile pour des tests rapides ou le dépannage.

    ```bash
    openclaw gateway --port 18789
    ```

  </Accordion>
  <Accordion title="Envoyer un message de test">
    Nécessite un canal configuré.

    ```bash
    openclaw message send --target +15555550123 --message "Bonjour depuis OpenClaw"
    ```

  </Accordion>
</AccordionGroup>

## Variables d'environnement utiles

Si vous exécutez OpenClaw en tant que compte de service ou souhaitez des emplacements de configuration/état personnalisés :

- `OPENCLAW_HOME` définit le répertoire personnel utilisé pour la résolution des chemins internes.
- `OPENCLAW_STATE_DIR` remplace le répertoire d'état.
- `OPENCLAW_CONFIG_PATH` remplace le chemin du fichier de configuration.

Référence complète des variables d'environnement : [Variables d'environnement](/help/environment).

## Aller plus loin

<Columns>
  <Card title="Assistant d'intégration (détails)" href="/start/wizard">
    Référence complète de l'assistant CLI et options avancées.
  </Card>
  <Card title="Intégration de l'application macOS" href="/start/onboarding">
    Flux de première exécution pour l'application macOS.
  </Card>
</Columns>

## Ce que vous aurez

- Une Passerelle en cours d'exécution
- Authentification configurée
- Accès à l'UI de Contrôle ou un canal connecté

## Prochaines étapes

- Sécurité des DM et approbations : [Appairage](/channels/pairing)
- Connecter plus de canaux : [Canaux](/channels)
- Flux de travail avancés et depuis les sources : [Configuration](/start/setup)
