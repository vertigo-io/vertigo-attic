# mars

Fonctionnalités archivées en provenance du repo `vertigo-mars` (l'app de démo Vertigo-UI).

| Fonctionnalité | Commit d'origine | Retiré le |
|---|---|---|
| [iot/](iot) — IoT / Arduino (MQTT, capteurs, environnement) | `598b26c0` (develop) | 2026-06-12 |
| [ai/](ai) — IA/LLM (5 écrans, RAG langchain4j/OpenAI, notebooks) | `34e6553b` (develop) | 2026-06-12 |
| [chatbot/](chatbot) — chatbot Rasa (module command/bot, widget front) | `34e6553b` (develop) | 2026-06-12 |

## iot/

Chaîne IoT complète : un shield Arduino publie des mesures (température…) sur
Mosquitto (MQTT) ; `MqttShield` les écoute et les republie sur l'event bus ;
`IotEquipmentServices` les persiste en séries temporelles (InfluxDB) ;
les écrans « Environment » (équipement et global) les affichent, et
`BaseManagementCommands` pilote les actionneurs (alertes, messages).

Le répertoire `src/iot/arduino/` contient les sketches Arduino (.ino) embarqués :
station météo, capteurs de température (Dallas, DHT22), détection de mouvement,
humidité, boutons NodeMCU — le `Mars_Project.ino` étant le sketch principal.

Les fichiers sont rangés selon leur chemin d'origine dans `vertigo-mars`.
Config retirée du repo d'origine, à restaurer en cas de résurrection :

`pom.xml` :

```xml
<dependency>
	<groupId>io.vertigo</groupId>
	<artifactId>vertigo-mqtt-connector</artifactId>
	<version>${vertigo.version}</version>
</dependency>
```

`mars.yaml` :

```yaml
  io.vertigo.connectors.mqtt.MqttFeatures:
    __flags__: ["klee"]
    features:
      - mosquitto:
          name: Subscriber
          host: ${mosquittoHost}
      - mosquitto:
          name: Publisher
          host: ${mosquittoHost}
```

et sous `io.mars.basemanagement.BasemanagementFeatures` :

```yaml
      - mqtt:
          __flags__: ["klee"]
```

`BasemanagementFeatures.java` :

```java
@Feature("mqtt")
public BasemanagementFeatures withMqtt(final Param... params) {
	getModuleConfigBuilder().addComponent(MqttShield.class, params);
	return this;
}
```

`context.xml` :

```xml
<Parameter name="mosquittoHost" value="tcp://mars.dev.klee.lan.net:1883" override="false"/>
```

Onglet des vues équipement (`equipmentInformation.html` et consorts) :

```html
<vu:button-link flat label="Environment" url="@{/basemanagement/equipment/environment/} + ${model.equipment.equipmentId}" />
```
