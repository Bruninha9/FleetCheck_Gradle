# FleetCheck – Build Systems Lab

This project is intentionally incomplete. Follow the worksheet in the order given.

Expected final application output:

```
FleetCheck 1.0
Vehicles loaded: 4
Vehicles requiring service: 2
Average mileage: 37000 km
```

Do not copy the solution POM. The objective is to observe how each build change alters the result.

## Respostas às Evidências (Gradle)

### Evidence 8.1 & 8.2: Dependências e Árvore de Runtime
- **Dependência em Falta:** Jackson Databind (`com.fasterxml.jackson.core:jackson-databind`).
- **Dependências Transitivas:** Ao executar `gradle dependencies --configuration runtimeClasspath`, é possível observar que `jackson-core` e `jackson-annotations` surgem como dependências transitivas trazidas pelo `jackson-databind`.
- **Comparação:** Alterar o sistema de build de Maven para Gradle **não alterou** as dependências da aplicação, pois o grafo final de bibliotecas de runtime necessárias continua exatamente o mesmo.

### Evidence 8.3: Executável FAT JAR no Gradle
- **Transformação:** Ao configurar a tarefa `jar` com `configurations.runtimeClasspath`, o Gradle descompacta e inclui todos os ficheiros `.class` das dependências diretamente dentro do JAR final. Isso transforma o artefacto num FAT JAR autónomo e executável.

### Evidence 8.4: Suposição de Ambiente Eliminada pelo Wrapper
- **Suposição Eliminada:** Elimina a necessidade de ter uma instalação prévia do Gradle na máquina do programador ou no servidor de CI/CD, garantindo compilações determinísticas com a versão exata pinned pelo projeto.

### Evidence 8.5: Execução no GitHub Actions
- **URL do Execução com Sucesso:** https://github.com/Bruninha9/FleetCheck_Gradle/actions

### Evidence 8.6: SBOM Gerado pelo Gradle
- **Inclusão Transitiva:** O CycloneDX em Gradle mapeia recursivamente todo o grafo de dependências do projeto (diretas, transitivas e componentes de build), adicionando-os ao `bom.json` final.

### Section 8.7: Tabela Comparativa Maven vs Gradle

| Tarefa | Maven | Gradle |
| :--- | :--- | :--- |
| **Configuração do Build** | `pom.xml` | `build.gradle` |
| **Clean Build** | `mvnw.cmd clean verify` | `gradlew.bat clean build` |
| **Adicionar Dependência** | `<dependency>...</dependency>` | `implementation 'group:artifact:version'` |
| **Inspecionar Dependências** | `mvn dependency:tree` | `gradle dependencies` |
| **Wrapper** | `mvnw.cmd` | `gradlew.bat` |
| **Pasta de Saída (Build Output)** | `target/` | `build/` |
| **Localização do JAR** | `target/` | `build/libs/` |
| **Gerador de SBOM** | CycloneDX Maven plugin | CycloneDX Gradle plugin |

**Pergunta Final:** O software ou o processo de construção? O que mudou?
- **Resposta:** Mudou apenas o **processo de construção** (a ferramenta de automação, a sintaxe de configuração DSL e a gestão das tarefas de lifecycle). O código Java, os testes unitários e o comportamento da aplicação em tempo de execução mantêm-se rigorosamente idênticos.