# mars / ai

Fonctionnalité IA/LLM archivée depuis `vertigo-mars` (commit `34e6553b`, develop,
retirée le 2026-06-12). Les notebooks `src/ia/` proviennent du commit `598b26c0`
(supprimés du repo d'origine juste avant l'archivage).

Contenu : 5 écrans IA (analyse de fichiers, énergie, transport, santé, documents/RAG)
sur `vertigo-ai-langchain4j` (OpenAI + embeddings + stockage pgvector), un
`FakeLlmPlugin` pour le mode `home`, les DTOs studio (ai.kpr/ksp + javagen),
et deux notebooks Python de reconnaissance faciale (hors build).

Les fichiers sont rangés selon leur chemin d'origine. Config retirée du repo
d'origine, à restaurer en cas de résurrection :

`pom.xml` (dependencyManagement) :

```xml
<dependency>
	<groupId>io.vertigo</groupId>
	<artifactId>vertigo-ai-langchain4j</artifactId>
	<version>${vertigo.version}</version>
	<type>pom</type>
	<scope>import</scope>
</dependency>
```

`pom.xml` (dependencies) :

```xml
<dependency>
	<groupId>io.vertigo</groupId>
	<artifactId>vertigo-ai-langchain4j</artifactId>
	<version>${vertigo.version}</version>
</dependency>
<dependency>
	<groupId>org.apache.tika</groupId>
	<artifactId>tika-core</artifactId>
	<version>2.9.1</version> <!-- force version for langchain4j (revert vertigo dependency-management) -->
</dependency>
<dependency>
    <groupId>dev.langchain4j</groupId>
    <artifactId>langchain4j-open-ai</artifactId>
</dependency>
<dependency>
    <groupId>dev.langchain4j</groupId>
    <artifactId>langchain4j-pgvector</artifactId>
</dependency>
```

`mars.yaml` :

```yaml
  io.vertigo.ai.AiFeatures:
    __flags__: ["klee"]
    features:
      - llm:
    plugins:
#      - io.vertigo.ai.llm.plugin.lc4j.rag.embedding.Lc4jClasspathEmbeddingPlugin:
      - io.vertigo.ai.llm.plugin.lc4j.Lc4jPlugin:
          apiKey: ${openAiApiKey}
      - io.vertigo.ai.llm.plugin.lc4j.rag.embedding.Lc4jOpenAiEmbeddingPlugin:
          apiKey: ${openAiApiKey}
      - io.vertigo.ai.llm.plugin.lc4j.rag.storage.Lc4jPgVectorStoragePlugin:
          __flags__: ["pgvector"]
          dataSpace: pgVector
          createTable: true
#          dropTableFirst: true
```

```yaml
  io.mars.ai.AiFeatures:
    features:
      - fakeLlm:
          __flags__: ["home"]
```

Datasources `pgVector` dans `DatabaseFeatures.featuresConfig` (flag `pgvector`) :

```yaml
      - sql.datasource:
          __flags__: ["home && pgvector && tomcat"]
          name: pgVector
          classname: io.vertigo.database.impl.sql.vendor.h2.H2DataBase
          source: java:/comp/env/jdbc/DataSourceHomePg
      - sql.c3p0:
          __flags__: ["home && pgvector && !tomcat"]
          name: pgVector
          dataBaseClass: io.vertigo.database.impl.sql.vendor.postgresql.PostgreSqlDataBase
          jdbcDriver: org.postgresql.Driver
          jdbcUrl: jdbc:postgresql://localhost:5432/mars?user=mars&password=mars
```

`application.kpr` : ligne `ai/ai.kpr`.

`README.md` (template de propriétés) : `openAiApiKey=demo`.

`mmcParentLayout.html` : section de menu « AI center » (5 entrées vers
`/ai/extract/`, `/ai/energy/`, `/ai/transport/`, `/ai/health/`, `/ai/documents/`).
