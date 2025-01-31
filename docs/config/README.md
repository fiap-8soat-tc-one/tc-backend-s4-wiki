# Setup do ambiente de desenvolvimento local :rocket:

## Pré-requisitos :warning:

- [Intellij IDEA (Community ou superior)](https://www.jetbrains.com/pt-br/idea/)
- [DBeaver](https://dbeaver.io/)
- [Docker](https://www.docker.com/)
- [GIT](https://git-scm.com/)
- [K6](https://grafana.com/docs/k6/latest/)

## PostgreSQL :floppy_disk:

PostgreSQL é um sistema de gerenciamento de banco de dados relacional e objeto-relacional (ORDBMS) de código aberto. Ele
é conhecido por sua robustez, extensibilidade e conformidade com os padrões SQL. Foi desenvolvido para ser altamente
escalável, suportando grandes volumes de dados e usuários simultâneos.

### Vantagens do PostgreSQL :heavy_check_mark:

1. **Conformidade com SQL:** PostgreSQL é altamente compatível com o padrão SQL, garantindo portabilidade e facilidade
   de uso.
2. **Extensibilidade:** Permite a criação de tipos de dados personalizados, funções e operadores, oferecendo grande
   flexibilidade.
3. **Desempenho e Escalabilidade:** Suporta consultas complexas e grandes volumes de dados com eficiência.
4. **Segurança:** Oferece recursos avançados de segurança, como autenticação baseada em roles, SSL, e controle de acesso
   granular.
5. **Transações ACID:** Garantia de integridade e consistência dos dados através de transações completas com
   Atomicidade, Consistência, Isolamento e Durabilidade.
6. **Comunidade Ativa:** Um grande número de desenvolvedores e documentação disponível, facilitando a resolução de
   problemas e o aprendizado.

### Sinergia com Java :heavy_check_mark:

1. **JDBC (Java Database Connectivity):** O PostgreSQL possui drivers JDBC robustos, permitindo uma integração suave com
   aplicações Java.
2. **ORMs (Object-Relational Mappers):** Ferramentas como Hibernate e JPA (Java Persistence API) funcionam muito bem com
   PostgreSQL, facilitando o mapeamento objeto-relacional e reduzindo a complexidade do código.
3. **Multiplataforma:** Tanto PostgreSQL quanto Java são multiplataforma, permitindo a criação de aplicações portáveis e
   independentes de sistema operacional.
4. **Desempenho:** A combinação de PostgreSQL com Java permite a criação de aplicações de alto desempenho, aproveitando
   o melhor das capacidades de gerenciamento de dados de PostgreSQL e a eficiência da JVM (Java Virtual Machine).
5. **Ferramentas de Desenvolvimento:** Existem várias ferramentas de desenvolvimento e bibliotecas em Java que oferecem
   suporte direto para PostgreSQL, melhorando a produtividade do desenvolvedor.
6. **Escalabilidade e Manutenção:** Aplicações em Java com PostgreSQL são fáceis de escalar e manter, graças à robustez
   e confiabilidade de ambos.

### Instalação/Hospedagem do PostgreSQL :heavy_check_mark:

O modelo escolhido para o tech challanger foi a utilização de contêineres Docker.
Optamos por uma imagem do tipo alpine, levando em consideração a facilidade de implantação em cenários de
desenvolvimento onde as mudanças são frequentes e precisamos trazer celeridade nos processos de desenvolvimento além da
eficiência de recursos, considerando o ambiente "produtivo" como as maquinas dos avaliadores do tech challanger, não
possuimos dados sobre os recursos de CPU/Memória então optar por um ambiente menor nos garante mais estabilidade para os
possíveis testes que possam acontecer.

**Script para execução do contêiner PostgreSQL**

```sh
docker pull postgres:15.3-alpine
docker run --name tech-challenge -p 5432:5432 -e POSTGRES_PASSWORD=123456 -d postgres:15.3-alpine
```

---

Após a execução dos comandos acima, validar a conectividade através do CLI ou interface visual da sua preferência, aqui
iremos utilizar o [DBeaver](https://dbeaver.io/).

![image](https://github.com/fiap-8soat-tc-one/tc-backend-s2/blob/main/assets/setup-postgres.png)

## Intellij e JAVA 17 :factory:

Java 17 é uma versão de Long-Term Support (LTS) do Java lançada em setembro de 2021 pela Oracle. Esta versão terá
suporte estendido até 2029.

Optamos pela escolha de JAVA como principal linguagem nesse primeiro tech challanger, devido ao know-how do time, onde
dos 5 membros temos:

- 2 Back-ends JAVA
- 1 Back-end C#/.NET
- 1 DevOps
- 1 Front-end

Considerando a familiaridade entre as linguagens back-end que o time domina, JAVA foi a escolha pela maior amplitude de
colaboração.

Para ambiente de desenvolvimento optamos pela IDE da jetbrains, uma vez que ela traz facilidades no setup do ambiente,
sem necessidade de configurações avançadas, além disso em seu kit de ferramentas e funcionalidades temos:

1. **Refatoração Poderosa:**
    - **Automação de Refatorações:** Facilita a modificação segura e automática do código, mantendo a integridade do
      projeto.
    - **Sugestões Inteligentes:** IntelliJ sugere melhorias de código em tempo real, ajudando a manter um código limpo e
      eficiente.

2. **Depuração Avançada:**
    - **Breakpoints Condicionais:** Permite definir breakpoints que só param a execução quando certas condições são
      atendidas.
    - **Análise de Threads:** Ferramentas integradas para inspecionar e gerenciar múltiplas threads durante a depuração.

3. **Integração com Ferramentas de Build:**
    - **Maven e Gradle:** Suporte nativo e integração completa com sistemas de build populares.

4. **Assistente de Código (Code Assistance):**
    - **Autocompletar Inteligente:** Sugestões contextuais para completar código mais rapidamente.
    - **Navegação Fácil:** Ferramentas para navegar rapidamente entre classes, métodos e arquivos.

5. **Ferramentas de Teste Integradas:**
    - **JUnit e TestNG:** Suporte integrado para frameworks de teste populares, permitindo a execução e análise de
      testes dentro do próprio IDE.
    - **Cobertura de Código:** Ferramentas para medir a cobertura de testes, garantindo que o código seja bem testado.

6. **Plugins e Extensibilidade:**
    - **Ecossistema de Plugins:** Ampla variedade de plugins disponíveis para estender a funcionalidade do IDE.
    - **Customização:** Permite personalizar o ambiente de desenvolvimento conforme as necessidades específicas do
      desenvolvedor.

7. **Desempenho:**
    - **Rápido e Responsivo:** Desempenho eficiente, mesmo em projetos grandes e complexos.
    - **Índices de Código:** IntelliJ cria índices de código inteligentes para busca e navegação rápidas.

### Setup da IDE :rotating_light:

1. **Clone do Repositório**
    - Clone o repositório a partir do link: `https://github.com/fiap-8soat-tc-one/tc-backend.git`

2. **Configuração da JDK**
    - Abra o projeto através da IDE e acesse o caminho `File -> Project Structure`.
    - NA sessão `Plataform Settings -> SDKs` instale o open-jdk 17, segue abaixo a imagem:

   ![image](https://github.com/fiap-8soat-tc-one/tc-backend-s2/blob/main/assets/setup-intellij-jdk.png)

3. **Configuração do Maven**
    - Com projeto aberto através da IDE acessar o caminho `Run -> Edit Configuration`
    - Adicionar o seguinte comando maven `mvn clean package`, segue abaixo a imagem:

   ![image](https://github.com/fiap-8soat-tc-one/tc-backend-s2/blob/main/assets/setup-intellij-maven.png)

    - Executar o comando deve apresentar o seguinte resultado

   ![image](https://github.com/fiap-8soat-tc-one/tc-backend-s2/blob/main/assets/setup-intellij-maven-result.png)

4. **Execução da aplicação localmente**
    - Com projeto aberto através da IDE acessar o caminho `Run -> Edit Configuration`
    - Adicionaras seguintes configurações:

   ![image](https://github.com/fiap-8soat-tc-one/tc-backend-s2/blob/main/assets/setup-intellij-app.png)

    - Executar a aplicação e o resultado experado deve ser:

   ![image](https://github.com/fiap-8soat-tc-one/tc-backend-s2/blob/main/assets/setup-intellij-maven-result.png)

    - **A aplicação está configurada para o flyway gerar as tabelas no PostgreSQL, abra o DBeaver ou a ferramenta da sua
      escolha e confira se as tabelas do sistema foram criadas.**

    - Ao executar a aplicação, acessar a url do swagger/open-api `http://localhost:8080/swagger-ui/index.html`

### Grafana K6

O **Grafana k6** é uma ferramenta open-source para testes de carga e desempenho, projetada para simular como uma aplicação se comporta sob alto tráfego. Originalmente chamada apenas de **k6**, após ser adquirida pela Grafana Labs, passou a integrar o ecossistema da Grafana, facilitando a criação, execução e análise de testes de performance. Ele é especialmente utilizado para testar APIs, microsserviços e aplicações web, permitindo que equipes validem a capacidade de suas infraestruturas sob carga.

### Principais vantagens do Grafana k6 em comparação com outras ferramentas:

1. **Integração com o Ecossistema Grafana**: A integração com Grafana permite uma visualização robusta e em tempo real dos resultados de desempenho, possibilitando uma análise mais profunda e rica.

2. **Script em JavaScript**: Os scripts de teste são escritos em JavaScript, uma linguagem amplamente conhecida, o que facilita o aprendizado e a criação de testes personalizados em comparação com outras ferramentas que usam linguagens ou sintaxes mais complexas.

3. **Escalabilidade**: k6 é altamente escalável e pode simular milhares de usuários virtuais sem comprometer o desempenho da máquina de teste, tornando-o uma escolha adequada para testes em larga escala.

4. **Fácil Automação e Integração**: Ele pode ser facilmente integrado em pipelines de CI/CD e automatizado, oferecendo compatibilidade com várias ferramentas de automação.

5. **Baixo Overhead**: Diferente de outras ferramentas que podem consumir muitos recursos da máquina de teste, o k6 tem um overhead baixo, garantindo que a máquina utilizada para o teste não afete os resultados de desempenho.

6. **Modo de Execução Local e Distribuído**: Ele suporta tanto testes locais quanto a execução distribuída na nuvem, o que aumenta a flexibilidade para testar grandes cargas.

Essas características tornam o k6 uma solução poderosa, especialmente para equipes que já utilizam o Grafana para monitoramento e querem uma integração nativa de performance testing na sua stack de observabilidade.