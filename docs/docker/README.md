# Docker e Docker Compose :rocket:

## Docker :whale:

Para o tech challenger, o ambiente de hospedagem escolhido será via contêiner utilizando Docker como plataforma.

Ao adota essa plataforma garantimos as seguintes vantagens:

1. Consistência e Portabilidade: Permite criar ambientes de desenvolvimento, teste e produção idênticos, garantindo que
   o software funcione da mesma forma em qualquer lugar.

2. Isolamento: Cada contêiner roda de maneira isolada, evitando conflitos de dependências e facilitando a manutenção e
   atualização.

3. Escalabilidade: Facilita o escalonamento de aplicações, permitindo a adição ou remoção de contêineres conforme
   necessário.

4. Eficiência de Recursos: Contêineres são mais leves e utilizam menos recursos comparados a máquinas virtuais
   tradicionais.

5. Velocidade: Permite um desenvolvimento mais rápido e ágil, com tempos de inicialização de contêineres
   significativamente menores.

6. DevOps e Integração Contínua: Integra-se bem com práticas de DevOps, automatizando processos de build, teste e
   deployment.

As imagens base escolhidas para estruturação da aplicação em formato de contêiner foram `slim`, são imagens menores
facilitando a subida da aplicação e manutenção da mesma.

### Slim x Alpine :warning:

- Slim: Possuem um bom equilíbrio entre o tamanho reduzido e compatibilidade com pacotes, mantendo a mesma versão da
  distro original a qual foi baseada.
- Alpine: Extremamente economia de espaço, porém pode exigir um conhecimento maior para configuração e gerenciamento de
  pacotes da distribuição.

Para o tech challenger, foi escolhido imagens alpine para "produtos" prontos como redis, postgresql por não haver a
necessidade de manutenção e para a aplicação visando uma possibilidade de *troubleshooting* imagens do tipo slim.

### Estrutura do Dockerfile Criada  :rotating_light:

```dockerfile
# Etapa de construção
FROM maven:3.8.4-openjdk-17-slim AS build

# Cria o diretório de trabalho
WORKDIR /app

# Copia o arquivo pom.xml e resolve as dependências do projeto (usando o cache do Docker)
COPY pom.xml /app/pom.xml
RUN mvn dependency:go-offline

# Copia todos os arquivos de código fonte e compila o projeto
COPY src /app/src
RUN mvn clean package -DskipTests

# Etapa de implantação
FROM openjdk:17-jdk-slim AS deploy

# Define o diretório de trabalho
WORKDIR /app

# Copia o jar do build para o diretório de trabalho na etapa de deploy
COPY --from=build /app/target/*.jar /app/tc-backend-api.jar

# Define o comando de entrada
ENTRYPOINT ["java", "-jar", "tc-backend-api.jar"]

```

## Docker Compose :whale:

Docker Compose é uma ferramenta para definir e gerenciar multi-contêineres Docker de maneira simplificada, dentro do
ambiente do tech challenger, precisamos iniciar tanto o PostgreSQL quanto a própria aplicação e para isso iremos
utilizar do Docker Compose.

### Execução :round_pushpin:

1. Escolha o terminal de sua preferencia e acesse a raiz do diretório da aplicação onde ela foi clonada.
2. Verifique que é possível visualizar o arquivo docker-compose.yml através dos comandos `dir` (Se seu SO for
   Windows/PowerShell) ou `ls` (Linux)
3. **É pré-requisito que o Docker e Docker-Compose estejam devidamente configurados em sua maquina.**
4. Execute o comando `docker-compose up` para inicializar a subida do ambiente (Se deseja executar em modo *detached* ou
   seja em segundo plano adicione a flag -d).
5. Em seu terminal através do comando `docker ps` ou `docker container ls` ou `docker-compose ps`
6. Execute o comando `docker-compose down` para desligar o ambiente.
