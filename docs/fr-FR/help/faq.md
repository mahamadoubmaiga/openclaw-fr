---
summary: "Questions fréquemment posées sur la configuration, la configuration et l'utilisation d'OpenClaw"
title: "FAQ"
---

# FAQ

Réponses rapides et dépannage approfondi pour les configurations réelles (développement local, VPS, multi-agents, OAuth/clés API, basculement de modèle). Pour les diagnostics d'exécution, voir [Dépannage](/gateway/troubleshooting). Pour la référence de configuration complète, voir [Configuration](/gateway/configuration).

## Démarrage rapide et première configuration

### Quelle est la méthode recommandée pour installer et configurer OpenClaw ?

Utilisez le script d'installation du site web :

```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

Il installe Node si nécessaire, installe OpenClaw globalement et lance l'assistant d'intégration.

Voir [Installation](/install) pour d'autres méthodes.

### Comment ouvrir le tableau de bord après l'intégration ?

```bash
openclaw dashboard
```

### Quel runtime ai-je besoin ?

**Node 22 ou plus récent**. Vérifiez avec `node -v`.

### Je suis bloqué - quel est le moyen le plus rapide de me débloquer ?

1. Exécutez `openclaw doctor` — cela identifie les problèmes de configuration courants
2. Vérifiez les journaux : `openclaw logs`
3. Rejoignez Discord : https://discord.gg/clawd (#setup-help)

### Où puis-je voir les nouveautés de la dernière version ?

Voir le [CHANGELOG](https://github.com/openclaw/openclaw/blob/main/CHANGELOG.md) sur GitHub.

### Quelle est la différence entre stable et beta ?

- **stable** : versions étiquetées (`vYYYY.M.D`), npm dist-tag `latest` — recommandé pour la production
- **beta** : versions de pré-sortie (`vYYYY.M.D-beta.N`), npm dist-tag `beta` — nouvelles fonctionnalités, tests
- **dev** : tête mobile de `main` — derniers changements, peut être instable

Voir [Canaux de développement](/install/development-channels).

### Combien de temps prend généralement l'installation et l'intégration ?

- Installation : 2-5 minutes
- Intégration : 5-15 minutes (selon les canaux configurés)

### Ai-je besoin d'un abonnement Claude ou OpenAI pour exécuter ceci ?

**Recommandé mais non requis**. OpenClaw fonctionne avec :

- Abonnements OAuth : Claude Pro/Max, OpenAI ChatGPT
- Clés API : Anthropic, OpenAI, Google, et d'autres
- Modèles locaux : via Ollama ou llama.cpp

Pour de meilleurs résultats, utilisez **Anthropic Pro/Max + Opus 4.6**.

## Qu'est-ce qu'OpenClaw ?

### Qu'est-ce qu'OpenClaw, en un paragraphe ?

OpenClaw est un assistant IA personnel que vous exécutez sur vos propres appareils. Il se connecte aux canaux de messagerie que vous utilisez déjà (WhatsApp, Telegram, Slack, Discord, Signal, iMessage, etc.), fournit des outils de première classe (navigateur, canvas, cron, compétences) et fonctionne hors ligne d'abord avec votre choix de modèles (Claude, GPT, locaux). C'est une passerelle locale qui reste dans votre contrôle.

### Quelle est la proposition de valeur ?

- **Local d'abord** : vos données restent sur vos appareils
- **Multi-canaux** : utilisez les applications de messagerie que vous connaissez déjà
- **Flexibilité du modèle** : utilisez n'importe quel modèle (abonnements, API, local)
- **Outils puissants** : contrôle du navigateur, canvas, compétences, automatisation
- **Toujours actif** : fonctionne en arrière-plan sur votre machine ou VPS

### Je viens de le configurer - que devrais-je faire en premier ?

1. Ouvrez le tableau de bord : `openclaw dashboard`
2. Envoyez votre premier message : `openclaw agent --message "Bonjour !"`
3. Explorez les compétences : `openclaw skills list`
4. Configurez des canaux : voir [Canaux](/channels)
5. Lisez la [Vitrine](/start/showcase) pour des exemples

## Compétences et automatisation

### Comment puis-je personnaliser les compétences sans garder le dépôt sale ?

Utilisez le dossier d'espace de travail : `~/.openclaw/workspace/skills/`

```bash
openclaw skills create my-skill
```

Voir [Compétences](/tools/skills).

### Puis-je charger des compétences depuis un dossier personnalisé ?

Oui. Définissez `workspace.skillsDir` dans votre config :

```json
{
  "workspace": {
    "skillsDir": "/path/to/custom/skills"
  }
}
```

### Le bot se fige pendant un travail lourd. Comment puis-je décharger cela ?

Utilisez le mode sandbox ou exécutez des tâches dans des agents séparés. Voir [Bac à sable](/gateway/sandboxing).

### Les cron ou rappels ne se déclenchent pas. Que devrais-je vérifier ?

1. Vérifiez que la passerelle est en cours d'exécution : `openclaw status`
2. Vérifiez les jobs cron : `openclaw cron list`
3. Vérifiez les journaux : `openclaw logs`

Voir [Travaux Cron](/automation/cron-jobs).

## Bac à sable et mémoire

### Comment fonctionne la mémoire ?

OpenClaw utilise un système de mémoire sémantique qui :
- Stocke les interactions de manière persistante
- Rappelle le contexte pertinent automatiquement
- Prend en charge la recherche et la récupération

Voir les docs de mémoire complètes (référez-vous à la documentation originale en anglais pour plus de détails).

### La mémoire continue d'oublier des choses. Comment puis-je la faire rester ?

Soyez explicite dans vos instructions : "Souviens-toi que..." ou "Enregistre cela dans la mémoire..."

## Où les choses vivent sur le disque

### Où OpenClaw stocke-t-il ses données ?

- Configuration : `~/.openclaw/openclaw.json`
- Identifiants : `~/.openclaw/credentials/`
- Espace de travail : `~/.openclaw/workspace/`
- Sessions : `~/.openclaw/sessions/`
- Journaux : `~/.openclaw/logs/`

### Quelle est la stratégie de sauvegarde recommandée ?

Sauvegardez ces dossiers :
- `~/.openclaw/openclaw.json`
- `~/.openclaw/credentials/`
- `~/.openclaw/workspace/`

### Comment puis-je désinstaller complètement OpenClaw ?

```bash
npm uninstall -g openclaw
rm -rf ~/.openclaw
```

Voir [Désinstallation](/install/uninstall).

## Configuration de base

### Quel est le format de la config ? Où est-elle ?

Format JSON à `~/.openclaw/openclaw.json`

Voir [Configuration](/gateway/configuration).

### Dois-je redémarrer après avoir changé la configuration ?

Oui, redémarrez la passerelle :

```bash
openclaw gateway restart
```

## Canaux

### Telegram : que va dans `allowFrom` ?

Vos identifiants utilisateur Telegram. Obtenez-le de @userinfobot.

```json
{
  "channels": {
    "telegram": {
      "allowFrom": ["123456789"]
    }
  }
}
```

### Plusieurs personnes peuvent-elles utiliser un numéro WhatsApp avec différentes instances OpenClaw ?

Non. Un numéro WhatsApp ne peut être connecté qu'à une seule instance à la fois.

## VPS et déploiement

### Dois-je exécuter la Passerelle sur mon ordinateur portable ou un VPS ?

**VPS recommandé** pour :
- Disponibilité 24/7
- Accès distant
- Canaux WhatsApp/Telegram stables

**Ordinateur portable OK** pour :
- Développement local
- Tests
- Usage occasionnel

### Quelles sont les exigences minimales VPS et l'OS recommandé ?

Minimum :
- 1 CPU
- 2 Go RAM
- 20 Go disque
- Ubuntu 22.04 LTS ou Debian 11+

Recommandé :
- 2 CPU
- 4 Go RAM
- 40 Go disque

Voir les guides VPS : [Hetzner](/install/hetzner), [Fly](/install/fly), [Render](/install/render).

## Plus de questions ?

- **Documentation complète** : [docs.openclaw.ai](https://docs.openclaw.ai)
- **Discord** : [discord.gg/clawd](https://discord.gg/clawd)
- **GitHub Issues** : [github.com/openclaw/openclaw/issues](https://github.com/openclaw/openclaw/issues)
- **FAQ complète en anglais** : [FAQ original](/help/faq)
