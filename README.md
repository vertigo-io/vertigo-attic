# vertigo-attic

Archives de modules retirés des différents repos Vertigo.

Ce repo n'a **pas vocation à contenir du code qui fonctionne** : pas de build,
pas de tests, pas de CI. C'est un grenier — le code y est conservé tel qu'il
était au moment de son retrait, pour référence ou résurrection éventuelle.

## Structure

Un répertoire par repo d'origine :

```
connectors/   ← modules retirés de vertigo-connectors
mars/         ← fonctionnalités retirées de vertigo-mars
libs/         ← modules retirés de vertigo-libs
...
```

Chaque répertoire contient un `README.md` qui précise la provenance de chaque
module archivé (repo, commit, date de retrait).

## Contenu

| Module | Origine | Retiré le |
|---|---|---|
| [connectors/vertigo-ifttt-connector](connectors/vertigo-ifttt-connector) | vertigo-connectors | 2026-06-11 |
| [connectors/vertigo-twitter-connector](connectors/vertigo-twitter-connector) | vertigo-connectors | 2026-06-11 |
| [mars/iot](mars/iot) — IoT/Arduino (MQTT, capteurs, écrans environnement) | vertigo-mars | 2026-06-12 |
| [mars/ai](mars/ai) — IA/LLM (écrans IA, RAG, notebooks) | vertigo-mars | 2026-06-12 |
| [mars/chatbot](mars/chatbot) — chatbot Rasa (module command/bot, widget) | vertigo-mars | 2026-06-12 |
