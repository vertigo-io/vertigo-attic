# mars / chatbot

Fonctionnalité chatbot archivée depuis `vertigo-mars` (commit `34e6553b`, develop,
retirée le 2026-06-12).

Contenu :
- `src/main/java/io/mars/command/` — module complet : `CommandFeatures`,
  `CommandWebServices` (exécution de commandes pour le composant `v-commands`),
  `BotCommandWebServices` et `services/bot/generation/` (génération du training
  Rasa à partir des `@Command` : templates Freemarker domain/nlu/stories,
  `trainingConfiguration.json`).
- `src/main/resources/io/mars/webapp/static/chatbot/` — widget front (chatbot.js/css,
  index.html, shepherd.js pour le tour guidé, avatars).

Le bot s'appuyait sur un runner Rasa externe (`chatbotUrl`).

Config retirée du repo d'origine, à restaurer en cas de résurrection :

`mars.yaml` :

```yaml
  io.mars.command.CommandFeatures:
```

`mmcParentLayout.html` (head) :

```html
<script th:src="@{/static/chatbot/shepherd.min.js}"></script>
<script th:src="@{/static/chatbot/chatbot.js}"></script>
<link  th:href="@{/static/chatbot/chatbot.css}" rel="stylesheet"/>
<link  th:href="@{/static/chatbot/shepherd.css}" rel="stylesheet"/>
```

`GlobalParamsControllerAdvice.java` :

```java
viewContext.publishRef(() -> "chatbotUrl", paramManager.getParam("chatbotUrl").getValueAsString());
```

`context.xml` :

```xml
<Parameter name="chatbotUrl" value="https://chatbot-runner-rec.dev.klee.lan.net/runnerDemo" override="false"/>
```

`src/dev/env-dev.properties` :

```properties
chatbotUrl=https://chatbot-runner-rec.dev.klee.lan.net/runnerDemo
```

NB : la feature `- command:` de `CommonsFeatures` et les classes `HrCommands`/
`MaintenanceCommands` restent dans mars (le système de commandes sert au-delà du bot) ;
la ligne `v-commands` (déjà commentée) a été retirée du layout en même temps.
