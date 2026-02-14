# 🦞 OpenClaw — Assistant IA Personnel

<p align="center">
    <picture>
        <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/openclaw/openclaw/main/docs/assets/openclaw-logo-text-dark.png">
        <img src="https://raw.githubusercontent.com/openclaw/openclaw/main/docs/assets/openclaw-logo-text.png" alt="OpenClaw" width="500">
    </picture>
</p>

<p align="center">
  <strong>EXFOLIATE! EXFOLIATE!</strong>
</p>

<p align="center">
  <a href="https://github.com/openclaw/openclaw/actions/workflows/ci.yml?branch=main"><img src="https://img.shields.io/github/actions/workflow/status/openclaw/openclaw/ci.yml?branch=main&style=for-the-badge" alt="CI status"></a>
  <a href="https://github.com/openclaw/openclaw/releases"><img src="https://img.shields.io/github/v/release/openclaw/openclaw?include_prereleases&style=for-the-badge" alt="GitHub release"></a>
  <a href="https://discord.gg/clawd"><img src="https://img.shields.io/discord/1456350064065904867?label=Discord&logo=discord&logoColor=white&color=5865F2&style=for-the-badge" alt="Discord"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-blue.svg?style=for-the-badge" alt="MIT License"></a>
</p>

**OpenClaw** est un _assistant IA personnel_ que vous exécutez sur vos propres appareils.
Il vous répond sur les canaux que vous utilisez déjà (WhatsApp, Telegram, Slack, Discord, Google Chat, Signal, iMessage, Microsoft Teams, WebChat), ainsi que sur des canaux d'extension comme BlueBubbles, Matrix, Zalo et Zalo Personal. Il peut parler et écouter sur macOS/iOS/Android, et peut afficher un Canvas en direct que vous contrôlez. La Passerelle n'est que le plan de contrôle — le produit, c'est l'assistant.

Si vous voulez un assistant personnel, mono-utilisateur qui se sent local, rapide et toujours disponible, c'est lui.

[Site Web](https://openclaw.ai) · [Documentation](https://docs.openclaw.ai) · [DeepWiki](https://deepwiki.com/openclaw/openclaw) · [Démarrage](https://docs.openclaw.ai/start/getting-started) · [Mise à jour](https://docs.openclaw.ai/install/updating) · [Vitrine](https://docs.openclaw.ai/start/showcase) · [FAQ](https://docs.openclaw.ai/start/faq) · [Assistant](https://docs.openclaw.ai/start/wizard) · [Nix](https://github.com/openclaw/nix-openclaw) · [Docker](https://docs.openclaw.ai/install/docker) · [Discord](https://discord.gg/clawd)

Configuration préférée : exécutez l'assistant d'intégration (`openclaw onboard`) dans votre terminal.
L'assistant vous guide étape par étape dans la configuration de la passerelle, de l'espace de travail, des canaux et des compétences. L'assistant CLI est le chemin recommandé et fonctionne sur **macOS, Linux et Windows (via WSL2 ; fortement recommandé)**.
Fonctionne avec npm, pnpm ou bun.
Nouvelle installation ? Commencez ici : [Démarrage](https://docs.openclaw.ai/start/getting-started)

**Abonnements (OAuth) :**

- **[Anthropic](https://www.anthropic.com/)** (Claude Pro/Max)
- **[OpenAI](https://openai.com/)** (ChatGPT/Codex)

Note sur les modèles : bien que n'importe quel modèle soit pris en charge, je recommande fortement **Anthropic Pro/Max (100/200) + Opus 4.6** pour sa force en contexte long et une meilleure résistance à l'injection de prompts. Voir [Intégration](https://docs.openclaw.ai/start/onboarding).

## Modèles (sélection + authentification)

- Configuration des modèles + CLI : [Modèles](https://docs.openclaw.ai/concepts/models)
- Rotation des profils d'authentification (OAuth vs clés API) + basculements : [Basculement de modèle](https://docs.openclaw.ai/concepts/model-failover)

## Installation (recommandée)

Runtime : **Node ≥22**.

```bash
npm install -g openclaw@latest
# ou : pnpm add -g openclaw@latest

openclaw onboard --install-daemon
```

L'assistant installe le daemon de la Passerelle (service utilisateur launchd/systemd) pour qu'il reste en cours d'exécution.

## Démarrage rapide (TL;DR)

Runtime : **Node ≥22**.

Guide complet pour débutants (auth, appairage, canaux) : [Démarrage](https://docs.openclaw.ai/start/getting-started)

```bash
openclaw onboard --install-daemon

openclaw gateway --port 18789 --verbose

# Envoyer un message
openclaw message send --to +1234567890 --message "Bonjour depuis OpenClaw"

# Parler à l'assistant (optionnellement livrer à n'importe quel canal connecté : WhatsApp/Telegram/Slack/Discord/Google Chat/Signal/iMessage/BlueBubbles/Microsoft Teams/Matrix/Zalo/Zalo Personal/WebChat)
openclaw agent --message "Liste de contrôle d'expédition" --thinking high
```

Mise à niveau ? [Guide de mise à jour](https://docs.openclaw.ai/install/updating) (et exécutez `openclaw doctor`).

## Canaux de développement

- **stable** : versions étiquetées (`vYYYY.M.D` ou `vYYYY.M.D-<patch>`), npm dist-tag `latest`.
- **beta** : tags de pré-version (`vYYYY.M.D-beta.N`), npm dist-tag `beta` (l'application macOS peut être manquante).
- **dev** : tête mobile de `main`, npm dist-tag `dev` (quand publié).

Changer de canal (git + npm) : `openclaw update --channel stable|beta|dev`.
Détails : [Canaux de développement](https://docs.openclaw.ai/install/development-channels).

## Depuis les sources (développement)

Préférez `pnpm` pour les builds depuis les sources. Bun est optionnel pour exécuter TypeScript directement.

```bash
git clone https://github.com/openclaw/openclaw.git
cd openclaw

pnpm install
pnpm ui:build # installe automatiquement les dépendances UI au premier lancement
pnpm build

pnpm openclaw onboard --install-daemon

# Boucle de développement (rechargement automatique sur les changements TS)
pnpm gateway:watch
```

Note : `pnpm openclaw ...` exécute TypeScript directement (via `tsx`). `pnpm build` produit `dist/` pour l'exécution via Node / le binaire `openclaw` packagé.

## Paramètres de sécurité par défaut (accès DM)

OpenClaw se connecte à de vraies surfaces de messagerie. Traitez les DM entrants comme **entrée non fiable**.

Guide de sécurité complet : [Sécurité](https://docs.openclaw.ai/gateway/security)

Comportement par défaut sur Telegram/WhatsApp/Signal/iMessage/Microsoft Teams/Discord/Google Chat/Slack :

- **Appairage DM** (`dmPolicy="pairing"` / `channels.discord.dm.policy="pairing"` / `channels.slack.dm.policy="pairing"`) : les expéditeurs inconnus reçoivent un court code d'appairage et le bot ne traite pas leur message.
- Approuver avec : `openclaw pairing approve <channel> <code>` (puis l'expéditeur est ajouté à un magasin de liste d'autorisation local).
- Les DM entrants publics nécessitent une activation explicite : définissez `dmPolicy="open"` et incluez `"*"` dans la liste d'autorisation du canal (`allowFrom` / `channels.discord.dm.allowFrom` / `channels.slack.dm.allowFrom`).

Exécutez `openclaw doctor` pour faire apparaître les politiques DM risquées/mal configurées.

## Points forts

- **[Passerelle locale d'abord](https://docs.openclaw.ai/gateway)** — plan de contrôle unique pour les sessions, canaux, outils et événements.
- **[Boîte de réception multi-canaux](https://docs.openclaw.ai/channels)** — WhatsApp, Telegram, Slack, Discord, Google Chat, Signal, BlueBubbles (iMessage), iMessage (legacy), Microsoft Teams, Matrix, Zalo, Zalo Personal, WebChat, macOS, iOS/Android.
- **[Routage multi-agents](https://docs.openclaw.ai/gateway/configuration)** — acheminez les canaux/comptes/pairs entrants vers des agents isolés (espaces de travail + sessions par agent).
- **[Voice Wake](https://docs.openclaw.ai/nodes/voicewake) + [Mode Conversation](https://docs.openclaw.ai/nodes/talk)** — reconnaissance vocale toujours active pour macOS/iOS/Android avec ElevenLabs.
- **[Canvas en direct](https://docs.openclaw.ai/platforms/mac/canvas)** — espace de travail visuel piloté par l'agent avec [A2UI](https://docs.openclaw.ai/platforms/mac/canvas#canvas-a2ui).
- **[Outils de première classe](https://docs.openclaw.ai/tools)** — navigateur, canvas, nodes, cron, sessions et actions Discord/Slack.
- **[Applications compagnes](https://docs.openclaw.ai/platforms/macos)** — application de barre de menu macOS + [nodes](https://docs.openclaw.ai/nodes) iOS/Android.
- **[Intégration](https://docs.openclaw.ai/start/wizard) + [compétences](https://docs.openclaw.ai/tools/skills)** — configuration guidée par assistant avec compétences groupées/gérées/d'espace de travail.

## Historique des étoiles

[![Star History Chart](https://api.star-history.com/svg?repos=openclaw/openclaw&type=date&legend=top-left)](https://www.star-history.com/#openclaw/openclaw&type=date&legend=top-left)

## Tout ce que nous avons construit jusqu'à présent

### Plateforme principale

- [Plan de contrôle WS de la passerelle](https://docs.openclaw.ai/gateway) avec sessions, présence, config, cron, webhooks, [UI de contrôle](https://docs.openclaw.ai/web), et [hôte Canvas](https://docs.openclaw.ai/platforms/mac/canvas#canvas-a2ui).
- [Surface CLI](https://docs.openclaw.ai/tools/agent-send) : gateway, agent, send, [assistant](https://docs.openclaw.ai/start/wizard), et [doctor](https://docs.openclaw.ai/gateway/doctor).
- [Runtime d'agent Pi](https://docs.openclaw.ai/concepts/agent) en mode RPC avec streaming d'outils et streaming par blocs.
- [Modèle de session](https://docs.openclaw.ai/concepts/session) : `main` pour les discussions directes, isolation de groupe, modes d'activation, modes de file d'attente, réponse. Règles de groupe : [Groupes](https://docs.openclaw.ai/concepts/groups).
- [Pipeline média](https://docs.openclaw.ai/nodes/images) : images/audio/vidéo, hooks de transcription, limites de taille, cycle de vie des fichiers temporaires. Détails audio : [Audio](https://docs.openclaw.ai/nodes/audio).

### Canaux

- [Canaux](https://docs.openclaw.ai/channels) : [WhatsApp](https://docs.openclaw.ai/channels/whatsapp) (Baileys), [Telegram](https://docs.openclaw.ai/channels/telegram) (grammY), [Slack](https://docs.openclaw.ai/channels/slack) (Bolt), [Discord](https://docs.openclaw.ai/channels/discord) (discord.js), [Google Chat](https://docs.openclaw.ai/channels/googlechat) (Chat API), [Signal](https://docs.openclaw.ai/channels/signal) (signal-cli), [BlueBubbles](https://docs.openclaw.ai/channels/bluebubbles) (iMessage, recommandé), [iMessage](https://docs.openclaw.ai/channels/imessage) (legacy imsg), [Microsoft Teams](https://docs.openclaw.ai/channels/msteams) (extension), [Matrix](https://docs.openclaw.ai/channels/matrix) (extension), [Zalo](https://docs.openclaw.ai/channels/zalo) (extension), [Zalo Personal](https://docs.openclaw.ai/channels/zalouser) (extension), [WebChat](https://docs.openclaw.ai/web/webchat).
- [Routage de groupe](https://docs.openclaw.ai/concepts/group-messages) : contrôle de mention, tags de réponse, chunking et routage par canal. Règles de canal : [Canaux](https://docs.openclaw.ai/channels).

### Applications + nodes

- [Application macOS](https://docs.openclaw.ai/platforms/macos) : plan de contrôle de la barre de menu, [Voice Wake](https://docs.openclaw.ai/nodes/voicewake)/PTT, overlay [Mode Conversation](https://docs.openclaw.ai/nodes/talk), [WebChat](https://docs.openclaw.ai/web/webchat), outils de débogage, contrôle de [passerelle distante](https://docs.openclaw.ai/gateway/remote).
- [Node iOS](https://docs.openclaw.ai/platforms/ios) : [Canvas](https://docs.openclaw.ai/platforms/mac/canvas), [Voice Wake](https://docs.openclaw.ai/nodes/voicewake), [Mode Conversation](https://docs.openclaw.ai/nodes/talk), caméra, enregistrement d'écran, appairage Bonjour.
- [Node Android](https://docs.openclaw.ai/platforms/android) : [Canvas](https://docs.openclaw.ai/platforms/mac/canvas), [Mode Conversation](https://docs.openclaw.ai/nodes/talk), caméra, enregistrement d'écran, SMS optionnel.
- [Mode node macOS](https://docs.openclaw.ai/nodes) : system.run/notify + exposition canvas/caméra.

### Outils + automatisation

- [Contrôle du navigateur](https://docs.openclaw.ai/tools/browser) : Chrome/Chromium openclaw dédié, instantanés, actions, téléchargements, profils.
- [Canvas](https://docs.openclaw.ai/platforms/mac/canvas) : [A2UI](https://docs.openclaw.ai/platforms/mac/canvas#canvas-a2ui) push/reset, eval, snapshot.
- [Nodes](https://docs.openclaw.ai/nodes) : caméra snap/clip, enregistrement d'écran, [location.get](https://docs.openclaw.ai/nodes/location-command), notifications.
- [Cron + réveils](https://docs.openclaw.ai/automation/cron-jobs) ; [webhooks](https://docs.openclaw.ai/automation/webhook) ; [Gmail Pub/Sub](https://docs.openclaw.ai/automation/gmail-pubsub).
- [Plateforme de compétences](https://docs.openclaw.ai/tools/skills) : compétences groupées, gérées et d'espace de travail avec contrôle d'installation + UI.

### Runtime + sécurité

- [Routage de canal](https://docs.openclaw.ai/concepts/channel-routing), [politique de nouvelle tentative](https://docs.openclaw.ai/concepts/retry), et [streaming/chunking](https://docs.openclaw.ai/concepts/streaming).
- [Présence](https://docs.openclaw.ai/concepts/presence), [indicateurs de saisie](https://docs.openclaw.ai/concepts/typing-indicators), et [suivi d'utilisation](https://docs.openclaw.ai/concepts/usage-tracking).
- [Modèles](https://docs.openclaw.ai/concepts/models), [basculement de modèle](https://docs.openclaw.ai/concepts/model-failover), et [élagage de session](https://docs.openclaw.ai/concepts/session-pruning).
- [Sécurité](https://docs.openclaw.ai/gateway/security) et [dépannage](https://docs.openclaw.ai/channels/troubleshooting).

### Ops + packaging

- [UI de contrôle](https://docs.openclaw.ai/web) + [WebChat](https://docs.openclaw.ai/web/webchat) servis directement depuis la Passerelle.
- [Tailscale Serve/Funnel](https://docs.openclaw.ai/gateway/tailscale) ou [tunnels SSH](https://docs.openclaw.ai/gateway/remote) avec authentification token/mot de passe.
- [Mode Nix](https://docs.openclaw.ai/install/nix) pour une configuration déclarative ; installations basées sur [Docker](https://docs.openclaw.ai/install/docker).
- Migrations [Doctor](https://docs.openclaw.ai/gateway/doctor), [journalisation](https://docs.openclaw.ai/logging).

## Comment ça marche (court)

```
WhatsApp / Telegram / Slack / Discord / Google Chat / Signal / iMessage / BlueBubbles / Microsoft Teams / Matrix / Zalo / Zalo Personal / WebChat
                │
                ▼
┌───────────────────────────────┐
│         Passerelle            │
│       (plan de contrôle)      │
│     ws://127.0.0.1:18789      │
└──────────────┬────────────────┘
                │
                ├─ Agent Pi (RPC)
                ├─ CLI (openclaw …)
                ├─ UI WebChat
                ├─ Application macOS
                └─ Nodes iOS / Android
```

## Sous-systèmes clés

**Voir le [README.md original](README.md) pour la documentation complète en anglais.**
