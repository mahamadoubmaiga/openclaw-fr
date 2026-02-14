# Contribuer à OpenClaw

Bienvenue dans le réservoir à homards ! 🦞

## Liens rapides

- **GitHub :** https://github.com/openclaw/openclaw
- **Discord :** https://discord.gg/qkhbAGHRBT
- **X/Twitter :** [@steipete](https://x.com/steipete) / [@openclaw](https://x.com/openclaw)

## Mainteneurs

- **Peter Steinberger** - Dictateur Bienveillant
  - GitHub : [@steipete](https://github.com/steipete) · X : [@steipete](https://x.com/steipete)

- **Shadow** - Sous-système Discord + Slack
  - GitHub : [@thewilloftheshadow](https://github.com/thewilloftheshadow) · X : [@4shad0wed](https://x.com/4shad0wed)

- **Vignesh** - Mémoire (QMD), modélisation formelle, TUI et Lobster
  - GitHub : [@vignesh07](https://github.com/vignesh07) · X : [@\_vgnsh](https://x.com/_vgnsh)

- **Jos** - Telegram, API, mode Nix
  - GitHub : [@joshp123](https://github.com/joshp123) · X : [@jjpcodes](https://x.com/jjpcodes)

- **Christoph Nakazawa** - Infrastructure JS
  - GitHub : [@cpojer](https://github.com/cpojer) · X : [@cnakazawa](https://x.com/cnakazawa)

- **Gustavo Madeira Santana** - Multi-agents, CLI, interface web
  - GitHub : [@gumadeiras](https://github.com/gumadeiras) · X : [@gumadeiras](https://x.com/gumadeiras)

- **Maximilian Nussbaumer** - DevOps, CI, Sanité du Code
  - GitHub : [@quotentiroler](https://github.com/quotentiroler) · X : [@quotentiroler](https://x.com/quotentiroler)

## Comment contribuer

1. **Bugs et petites corrections** → Ouvrez une PR !
2. **Nouvelles fonctionnalités / architecture** → Commencez une [Discussion GitHub](https://github.com/openclaw/openclaw/discussions) ou demandez d'abord sur Discord
3. **Questions** → Discord #setup-help

## Avant votre PR

- Testez localement avec votre instance OpenClaw
- Exécutez les tests : `pnpm build && pnpm check && pnpm test`
- Assurez-vous que les vérifications CI passent
- Gardez les PR concentrées (une chose par PR)
- Décrivez le quoi et le pourquoi

## Décorateurs de l'UI de Contrôle

L'UI de Contrôle utilise Lit avec des décorateurs **legacy** (l'analyse Rollup actuelle ne prend pas en charge
les champs `accessor` requis pour les décorateurs standard). Lors de l'ajout de champs réactifs, conservez le
style legacy :

```ts
@state() foo = "bar";
@property({ type: Number }) count = 0;
```

Le `tsconfig.json` racine est configuré pour les décorateurs legacy (`experimentalDecorators: true`)
avec `useDefineForClassFields: false`. Évitez de changer cela à moins de mettre également à jour les
outils de build de l'UI pour prendre en charge les décorateurs standard.

## Les PR codées par IA/Vibe sont les bienvenues ! 🤖

Construit avec Codex, Claude ou d'autres outils IA ? **Génial - marquez-le simplement !**

Veuillez inclure dans votre PR :

- [ ] Marquer comme assisté par IA dans le titre ou la description de la PR
- [ ] Noter le degré de test (non testé / légèrement testé / entièrement testé)
- [ ] Inclure les prompts ou les journaux de session si possible (super utile !)
- [ ] Confirmer que vous comprenez ce que fait le code

Les PR IA sont des citoyens de première classe ici. Nous voulons juste de la transparence pour que les réviseurs sachent quoi chercher.

## Focus actuel et feuille de route 🗺

Nous priorisons actuellement :

- **Stabilité** : Correction des cas limites dans les connexions de canaux (WhatsApp/Telegram).
- **UX** : Amélioration de l'assistant d'intégration et des messages d'erreur.
- **Compétences** : Pour les contributions de compétences, rendez-vous sur [ClawHub](https://clawhub.ai/) — le hub communautaire pour les compétences OpenClaw.
- **Performance** : Optimisation de l'utilisation des tokens et de la logique de compactage.

Consultez les [Issues GitHub](https://github.com/openclaw/openclaw/issues) pour les labels "good first issue" !

## Signaler une vulnérabilité

Nous prenons les rapports de sécurité au sérieux. Signalez les vulnérabilités directement au dépôt où se trouve le problème :

- **CLI principal et passerelle** — [openclaw/openclaw](https://github.com/openclaw/openclaw)
- **Application de bureau macOS** — [openclaw/openclaw](https://github.com/openclaw/openclaw) (apps/macos)
- **Application iOS** — [openclaw/openclaw](https://github.com/openclaw/openclaw) (apps/ios)
- **Application Android** — [openclaw/openclaw](https://github.com/openclaw/openclaw) (apps/android)
- **ClawHub** — [openclaw/clawhub](https://github.com/openclaw/clawhub)
- **Modèle de confiance et de menace** — [openclaw/trust](https://github.com/openclaw/trust)

Pour les problèmes qui ne correspondent à aucun dépôt spécifique, ou si vous n'êtes pas sûr, envoyez un email à **security@openclaw.ai** et nous le redirigerons.

### Requis dans les rapports

1. **Titre**
2. **Évaluation de la gravité**
3. **Impact**
4. **Composant affecté**
5. **Reproduction technique**
6. **Impact démontré**
7. **Environnement**
8. **Conseils de remédiation**

Les rapports sans étapes de reproduction, impact démontré et conseils de remédiation seront déprioritisés. Étant donné le volume de résultats de scanner générés par IA, nous devons nous assurer que nous recevons des rapports vérifiés de chercheurs qui comprennent les problèmes.

---

**Merci de contribuer à OpenClaw !** 🦞✨
