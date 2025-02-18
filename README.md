# Tech Challenge Backend API

## O Desafio :triangular_flag_on_post:

Uma lanchonete de bairro está em expansão devido ao seu grande sucesso. Entretanto, com essa expansão e a ausência de um sistema de controle de pedidos, o atendimento aos clientes pode tornar-se caótico e confuso. Por exemplo, imagine que um cliente faça um pedido complexo, como um hambúrguer personalizado com ingredientes específicos, acompanhado de batatas fritas e uma bebida. O atendente pode anotar o pedido em um papel e entregá-lo à cozinha, mas não há garantia de que o pedido será preparado corretamente.

Sem um sistema de controle de pedidos, pode haver confusão entre os atendentes e a cozinha, resultando em atrasos na preparação e entrega dos pedidos. Pedidos podem ser perdidos, mal interpretados ou esquecidos, levando à insatisfação dos clientes e à perda de negócios.

Em resumo, um sistema de controle de pedidos é essencial para garantir que a lanchonete possa atender os clientes de maneira eficiente, gerenciando seus pedidos e estoques de forma adequada. Sem ele, a expansão da lanchonete pode não ser bem-sucedida, resultando em clientes insatisfeitos e impactando negativamente os negócios.

Para solucionar o problema, a lanchonete irá investir em um sistema de autoatendimento de fast food, composto por uma série de dispositivos e interfaces que permitem aos clientes selecionar e fazer pedidos sem precisar interagir com um atendente, com as seguintes funcionalidades:

1. **Pedido**
    - Os clientes são apresentados a uma interface de seleção na qual podem optar por se identificarem via CPF, se cadastrarem com nome e e-mail, ou não se identificar. A montagem do combo segue a sequência a seguir, sendo todas as etapas opcionais:
        - Lanche
        - Acompanhamento
        - Bebida
        - Sobremesa

**Em cada etapa, são exibidos o nome, descrição e preço de cada produto.**

2. **Pagamento**
    - O sistema deverá possuir uma opção de pagamento integrada para o MVP, sendo a forma de pagamento oferecida via QRCode do Mercado Pago.
    - Nesse MVP, será realizado um `fake checkout` para o fluxo de pagamento, sem integração direta com o Mercado Pago.

3. **Acompanhamento**
    - Uma vez que o pedido é confirmado e pago, ele é enviado para a cozinha para ser preparado. Simultaneamente, deve aparecer em um monitor para o cliente acompanhar o progresso do seu pedido com as seguintes etapas:
        - Recebido
        - Em preparação
        - Pronto
        - Finalizado

4. **Entrega**
    - Quando o pedido estiver pronto, o sistema deverá notificar o cliente que ele está disponível para retirada. Ao ser retirado, o pedido deve ser atualizado para o status finalizado.

**Além das etapas do cliente, o estabelecimento precisa de um acesso administrativo:**

1. **Gerenciar clientes**
    - Com a identificação dos clientes, o estabelecimento pode trabalhar em campanhas promocionais.

2. **Gerenciar produtos e categorias**
    - Os produtos dispostos para escolha do cliente serão gerenciados pelo estabelecimento, definindo nome, categoria, preço, descrição e imagens. Para esse sistema, teremos categorias fixas:
        - Lanche
        - Acompanhamento
        - Bebida
        - Sobremesa

3. **Acompanhamento de pedidos**
    - Deve ser possível acompanhar os pedidos em andamento e o tempo de espera de cada pedido.

As informações dispostas no sistema de pedidos precisarão ser gerenciadas pelo estabelecimento através de um painel administrativo.

## Equipe :construction_worker:

- Myller Lobo
- Jean Carlos
- Caio Isikawa
- Vanderly
- Thiago

## Pré-Requisitos :exclamation:

- Maven 3
- Java 17 (Open JDK 17)
- Postgres 15
- Docker Desktop
- IntelliJ IDEA
- DBeaver SQL Client
- Postman
- k6

---

## Clean Architecture

[Clique aqui para ser redirecionado a documentação sobre clean-architecture aplicada nesse projeto](https://github.com/fiap-8soat-tc-one/tc-backend-s2/blob/main/docs/clean-arch/README.md)

---

## Configuração de Ambiente de Desenvolvimento Local  :heavy_check_mark:

[Clique aqui para ser redirecionado para a wiki de configuração do ambiente de desenvolvimento local](https://github.com/fiap-8soat-tc-one/tc-backend-s2/blob/main/docs/config/README.md)

## Configuração do Ambiente Docker/Docker Compose :heavy_check_mark:

- **A aplicação está configurada para o Flyway gerar as tabelas no PostgreSQL. Abra o DBeaver ou a ferramenta de sua escolha e verifique se as tabelas do sistema foram criadas.**

[Clique aqui para ser redirecionado para a wiki de configuração do ambiente Docker](https://github.com/fiap-8soat-tc-one/tc-backend-s2/blob/main/docs/docker/README.md)

## Documentação do banco de dados :heavy_check_mark:

[Clique aqui para ser redirecionado para a wiki de documentação do banco de dados](https://github.com/fiap-8soat-tc-one/tc-backend-s3-rds-iac/blob/main/README.md)

## Detalhamento sobre Stress Testing e Smoke Testing :heavy_check_mark:

- **Dentro da pasta scripts/tests contém todos os scripts k6 para efetuar a execução os cenários de smoke-test e stress-test que foram realizados para configurar de maneira efetiva o os requests/limits da aplicação juntamente com o HPA**

[Clique aqui para ser redirecionado para a wiki de testes](https://github.com/fiap-8soat-tc-one/tc-backend-s2/blob/main/tests/README.md)

## Manual/Documentação de Funcionalidades (Swagger/Open API) :heavy_check_mark:

- **Para todos os endpoints privados, é necessário gerar o token via endpoint login(POST /oauth/token)**

- **É possível acessar o Swagger/Open API da aplicação pela seguinte URL: `http://localhost:8080/swagger-ui/index.html`**



### Workflow de Execução das APIs

**Segue abaixo o descritivo simplificado da jornada das APIs dentro do sistema, esses diagramas servem apenas para materializar a jornada do ClienteXTerminalxSistemaXCozinha, mas em nenhum momento substitui o detalhamento/especificação realizados no Domain Storytelling e Event Storming criados, favor utiliza-los como fonte da verdade**

1 - **Criação do Pedido a partir de um cliente identificado**

```mermaid
sequenceDiagram
    Cliente->>+Terminal de Autoatendimento: 1 - Informa CPF para identificação.
    Terminal de Autoatendimento->>+Sistema: 2 - [GET] http://localhost:8080/api/public/v1/customers/{cpf}
    Sistema-->>-Terminal de Autoatendimento: Retorna dados do cliente identificado.
    Terminal de Autoatendimento-->>-Cliente: Exibir dados do cliente.
    Cliente->>+Terminal de Autoatendimento: 3 - Buscar produtos para montar o pedido.
    Terminal de Autoatendimento->>+Sistema: 4 - [GET] http://localhost:8080/api/public/v1/products/categories/{categoryId}
    Sistema-->>-Terminal de Autoatendimento: Retorna dados dos produtos.
   Terminal de Autoatendimento-->>-Cliente: Exibir dados do produto.

    Cliente->>+Terminal de Autoatendimento: 5 - Escolhe produtos e realiza pedido.
    Terminal de Autoatendimento->>+Sistema: 6 - [POST] http://localhost:8080/api/public/v1/orders
    Sistema-->>-Terminal de Autoatendimento: Retorna dados do pedido cadastrado/recebido.
    Terminal de Autoatendimento-->>-Cliente: Exibir tela de pagamento.
```

**Observação:**

- **Os fluxos de 1 a 2 são opicionais.**
- **Não é necessário informar campo id_customer no payload do POST v1/orders uma vez que esse campo é opcional com base na escolha do usuário se identificar ou não.**

---

3 - **Pagamento do Pedido**

```mermaid
sequenceDiagram
    Fake Pagamento->>+Sistema: 1 - [POST] http://localhost:8080/api/public/v1/hook/orders/payments
    Sistema-->>-Fake Pagamento: Return Status Code 200 and result SUCCESS/ERROR
```

---

4 - **Acompanhamento e Preparação de Pedido na Cozinha**

```mermaid
sequenceDiagram
    Cozinha->>+Terminal interno: 1 - Buscar pedidos com status "Confirmado" para iniciar os preparos.
    Terminal interno->>+Sistema: 2 - [GET] http://localhost:8080/api/private/v1/orders
    Sistema-->>-Terminal interno: Retorna pedidos com status "Confirmado", "Pendente para Preparo" ou "Pronto para Retirada".
    Terminal interno-->>-Cozinha: Exibir dados para cozinha iniciar os preparos.

    Cozinha->>+Terminal interno: 3 - Seleciona pedido e altera status para "Em Preparação" (PREPARING)
    Terminal interno->>+Sistema: 4 - [PUT] http://localhost:8080/api/private/v1/orders/status
    Sistema-->>-Terminal interno: Retorna status do pedido alterado com sucesso.
    Terminal interno-->>-Cozinha: Exibir mudança de status do pedido.
    Cozinha->>+Terminal interno: 5 - Seleciona pedido e altera status para "Pronto para Retirada" (READY)
    Terminal interno->>+Sistema: 6 - [PUT] http://localhost:8080/api/private/v1/orders/status
    Sistema-->>-Terminal interno: Retorna status do pedido alterado com sucesso.
    Terminal interno-->>-Cozinha: Exibir mudança de status do pedido.
```

---

5 - **Finalização do pedido**

```mermaid
sequenceDiagram
    Atendente->>+Terminal interno: 1 - Seleciona pedido e altera status para "Finalizado" após retirada do cliente (FINISHED)
    Terminal interno->>+Sistema: 2 - [PUT] http://localhost:8080/api/private/v1/orders/status
    Sistema-->>-Terminal interno: Retorna status do pedido alterado com sucesso.
    Terminal interno-->>-Atendente: Exibir mudança de status do pedido.
```

[Clique aqui para ser redirecionado para a documentação das APIs e suas funcionalidades](https://documenter.getpostman.com/view/37556926/2sA3s1oXsw)

## Domain Storytelling :heavy_check_mark:

[Clique aqui para ser redirecionado para a documentação do domain storytelling](https://miro.com/app/board/uXjVKuUez2Q=/)

## Dicionário de Linguagem Onipresente/Ubíqua

| Palavra                         | Descrição                                                                                                                                                                                                     |
|---------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Lanchonete                      | Estabelecimento onde a solução/sistema será aplicado.                                                                                                                                                         |
| Cliente                         | Pessoa que realiza pedidos na lanchonete.                                                                                                                                                                     |
| Cozinha                         | Setor da lanchonete responsável por preparar todos os produtos do combo.                                                                                                                                      |
| Administrador/Usuário Sistêmico | Pessoa que cadastra produtos no sistema.                                                                                                                                                                      |
| Sistema de Controle de Pedidos  | Sistema que soluciona o problema da lanchonete, automatizando a coleta de pedidos, pagamento e comunicação com a cozinha.                                                                                     |
| Monitor/Terminal                | No Contexto da Cozinha: Display onde são exibidos os pedidos na cozinha pendentes de preparo. No Contexto do Cliente: Display onde o cliente consegue acompanhar o status dos seus pedidos.                   |
| Promoção                        | Oferta de produtos com desconto customizada por cliente.                                                                                                                                                      |
| Pagamento                       | Ação realizada pelo cliente ao fazer a leitura do QR code do Mercado Pago para realizar o pagamento do pedido.                                                                                                |
| Pedido                          | Pedido de combo realizado pelo cliente.                                                                                                                                                                       |
| RECEIVED/Pedido Recebido        | Pedido aguardando pagamento pelo cliente)                                                                                                                                                                     |
| PENDING/Pedido Pendente         | Status do pedido após uma falha no fluxo de pagamento.                                                                                                                                                        |
| PREPARING/Pedido Em Preparação  | Status do pedido após a após a conclusão do pagamento e encaminhamento para a cozinha iniciar o preparo.                                                                                                      |
| READY/Pedido Pronto             | Status do pedido após a cozinha terminar o preparo e disponibilizar para retirada pelo cliente.                                                                                                               |
| FINISHED/Pedido Finalizado      | Status do pedido após ser retirado pelo cliente.                                                                                                                                                              |
| CANCELED/Pedido Cancelado       | Status do pedido após ser cancelado pelo cliente ou pela cozinha.                                                                                                                                             |
| Acompanhamento                  | No Contexto de Itens do Pedido: Item que acompanha o hambúrguer, como, por exemplo, batata frita. No Contexto do Pedido: Funcionalidade que permite ao cliente acompanhar o status do seu pedido no monitor.  |

## Event Storming :heavy_check_mark:

[Clique aqui para ser redirecionado para a documentação do event storming](https://miro.com/app/board/uXjVK1Bf4Q4=/)

## Domain Mapping :heavy_check_mark:

![image](https://github.com/fiap-8soat-tc-one/tc-backend-s2/blob/main/assets/domain-mapping.drawio.png)


## Draft proposto para o desafio da fase 4 :heavy_check_mark:

[Clique aqui para ser redirecionado para o drasft](https://excalidraw.com/#room=788c047a84a1804ccff8,z7B6WGLVRFSn0qvuE8mKnw)

## Desenho de Arquitetura/Infraestrutura  proposto  :heavy_check_mark:

[Clique aqui para ser redirecionado para o desenho de arquitetura e infraestrutura](https://viewer.diagrams.net/?tags=%7B%7D&lightbox=1&highlight=0000ff&edit=_blank&layers=1&nav=1&title=arq-fiap-v4.2.drawio&dark=0#R%3Cmxfile%20scale%3D%221%22%20border%3D%220%22%3E%3Cdiagram%20name%3D%22Draft%22%20id%3D%22jiXIkiBsy346j2COqR-b%22%3E7V1rd5s4E%2F41OSf9YI4kEJePiVNvu9u%2Bm23a7e6nHAzYocHGCziX%2FvpXwoBBEgbHyCYpyTZrZC5CmhnNPHPRmTpePP0W2au7z6HrBWcIuE9n6tUZQibUMfkfbXnetKgIq5uWeeS7mzawbbjxf3qbRpi3rn3Xi7O2TVMShkHir6qNTrhcek5SabOjKHysnjYLA7fSsLLnXqUbtOHGsQOPO%2B277yZ32YshY9v%2BwfPnd%2FmToW5tvlnY%2BcnZjeM72w0fS03q%2BzN1HIVhsvm0eBp7AR296rhMar4tOhZ5y6TNBd7F5w9%2F3Vxa06f1pw%2FLsfVzCX6McHabBztYZ2%2Bc9TZ5zocgCtdL16N3AWfq5eOdn3g3K9uh3z6SWSdtd8kiIEeQfIyTKLz3xmEQRqRlGS7JaZd8V%2FPHelHiPZWasq7%2F5oULL4meySnZtyOsZT3LKGmkatkcPG7nBRmqvmm8K00KgkbWamfUMC%2Fuvx0w8iEbsz3GL%2B9Du%2FGjA2RHTkblkA6na8d3xdjO%2FCDIh%2B4MqRNMf4tBzelPJS3zwI7j%2FKpwmZSu2vxwU0G%2B0dMfenVku763varDaVIBqEwThKJZ0gE%2FSyq0oKRZsrg58VzC5dlhGCV34Txc2sH7betlleq353wKw1U2lT%2B8JHnOJtNeJ6GIE8qTVju%2BcbiOHG%2FHC2RSlPZ65yxEXmAn%2FkNVdomGM7v0OvRJV4rZgyqs8hhpUXD1Lokdzb0ku5CZl6InL58q3LE8Ylhqhunvi5im4NQ9J3cf5uFkHMA890DEMw80VUm8g5onhKxsK%2FrRX6SLaXn46cv7ZDW9CPz5krQllHmK1k%2F21Auuw9hP%2FJB%2BOw2TJFyQEwL6xaXt3M%2FTyS5PYPpDTkkfdhGvNos%2BpQQ7P5j5T3SiLrP%2BXN0lCdUWLuhAoInjLjXFJ%2FrCzCdkFCkOeSKauHZik%2F%2FR9piOk78eTQPSgVEcOr4d0GGYUKk1mfvJ3Xo6gshUVsu5PFLABkMKiCcFTbDYabKWOpUjhN%2F85MN6StouHDqBMUcY5GWTKj3YGR04ZGi8SEAgC991U%2Fkr4usq51MWzqQvKo6zh0vkUagzMtJAPI8CTbDAAVlKyP4s%2BlIOdL2ZvQ6SWs4W82CFTcVc%2BUBOCKPY8%2B4zhnxcjQgvJnSm0GS9CkLbpYyJACKEOAFkSZ9s6G%2BUUd%2FoE1mgR3%2Bn91Hih%2FlILn%2BqmCUDXtGBEApENZSl5ugcHYw%2FkhuNySdw7a%2B8wCdq3i%2FBo%2Bw6CrHZbh2VxqMt7IQV1anS5%2BJL8h9QKImNQfoPkxPGaaNl0gaFGm6l9vRIMSBzsk4eO2YaN2daWNCqF43kvxZCIr73Eucum%2BrGVZ%2BTDRyhpSPgRe8fvM1ApBSSSa7F05xCDIrjx04ILWUdp5cw%2Bh0AGBi8HpcZOFKozYIWQ2y6xksCDQskgYYlUVuOnpTI7Ys398n7EyUHfKQSmMhYsAzJnzFhSZtIhoh8zk56PoWUkC4VLGaekM4v3NASLNyaLKEAReiLni6vRK3S50n65psGOkaVOdH%2FW4f5F6M4lbFkLQVklJ42l2Xf5ze6Cp37dJI%2FfCtuSnq9uW%2F1WaS59PzXru2DRm0%2F0%2FEXnuvbxOalytsEacUXt%2BkXt5WDgCgbt246pLJtAaiaDOGaOk%2B4yBQImLyxe8oVmYUdEeq3eH02Vs8uYOSHMVVd6NHlxTQg0xU3UK7rPxz07M5e4vy7R1ho8jVMqKSdfA6nPrkTMZVWq3fN3Je%2BRg3%2FvQFRbOoqs2RavHqWG8Blgs5X1u7p2eDG%2BQgQ4SwIHy%2BWRHBlElE84HFiR8kF9WWUNRnSNvHpS6YPr52UDVy3680zWSIbW9RA1WDSVaN6iw0IKg1YhCIruSNm5xbtj8t55MVUeH22l2RZcsUr6%2BsQZdzbfSK2OLmAyB176VCVYu93axBxFbuCNYsENhESGUqiNoNvhPxpvM20eQLbKGoz%2BEZYY4MhkQkmajMw32P2aii4GjJXp0ZduE4oDjAu%2FKUCFxL5nVDWYq2r91cGBICzrsqKWsWf1aj51RiK7HpWFplbQIHoINX1SqgWsvaj%2FRhrCmHOVNp8dGh%2FLsnh5lP1LCLwYtK9W4o93U5TavflqXmqaiiMpqdpkFf0RB7OorH7hRGfYmEkoxg9%2F0OvV3B%2B%2BG92u%2FTg6qly9Fw%2BuvYin7w8pZwrIJwt8bpbWmUd6l71HWahhbsmuuzD%2B%2Bd%2FEba%2F%2FDH%2BH3z%2B8X366Z%2F3%2F6rTHD9vXpC1rhfktgTwp47Wj%2FZVMP82mnr%2FWN%2FGH%2B9%2BjiDQT0EB1UnDNZP2Y71Y5R2Zp9psq%2Fk5qSIENbOqCUHMqkKSfay5VShQheKVvTxIWfji%2Fbf2Yz814IzUcMMe1YM%2BfP16fX5TNn02j%2BqRkcOAheW1b%2Bv2kCL6NZPxKUFVPbFBpLVxKg1q2qCm9UBNu49lMSY082syzjRVKw%2BIKbGmKXDzmR0syGLO5ENkLr7fkIb3f9C%2F42Ad0%2Bk4mZ9PE%2BtcXcxGEUyWz4bGzwW0IFAEzhZDmvddtJoyg19ht7KWw1O9qjhBuHZ5rp5gE6s7ovcqz5DkVzWrflWIeE8XsgRuVWhaska%2FJ7F9Iu305boobmkr5OGnsnXWkV71naGWGiuxp%2Bzn0mmZklD%2FHKRWNSGsl2%2FX2K%2Fq6eTDpgPdRijyvtXOtOcrbxWEdMA%2BLmeR7REyWydr6rMdtGaRONI1pWpKjURxHrL0ZqGVL%2FKLDfjJnvjJTpdjWSgKpwAeRyayTizIRODW%2BDMEjpHqoqpBaWiAeLi0fqyglfjqkyB0ApeZ8LzOgaKDJE6LDB6hMSnbBBTqtvbKv53bifdIV2VWw72yLgwN1mu4HawZKkaKWTUiYB4sWDYigKkggREBYQdWxC50WIZScXH9kXz9WzbqvVQlojDJOX1XHlS7pA3WHhKLG4k0hqBiVh05BRpQpjFdoJewenVn9MXHBvdYSgT2Yurat7P10tk8QWgLa1Jn0dKqUwhVU4D8FBZwVUrImkWJwP0ne7Ekg05OuViTtxvEhGwxYSGdsV6QcWopASUuQ38Tykjpi0x4HKeRfUQbIN1crWloXy3FlaJhqk0H9%2BgyCP9be361T%2BT5Wbci%2F4GsmQ396lWw3pviD6xVoSAoyPdWoYA7LFk62oljIYqD1JLfGvaH2fKNZlftVJ4gkqHGow5ytbyoMsGEw9TY4vvCk9yDMLAYqjoMcBSTnUh360gEju%2FshU3FXir7vKWbu3EvTiCPz3%2Fz0vyU379%2FfVesDqGdqiOThNDtck9hLLGrxWq2q7OTh%2FSsYRE53SKCqiCdJvAYanl%2BTzV1ooOEenFAVbMlVq0Y0lDhoDRfFmgx1uXKIaW57W7MoWoBxdRA8VN1oUMDqwrmMReNNBuWQNkFSLE6SJQTToYIrpcVNm2vVqPEGU1t5z6czXzHG8XaS%2BPCZYd4j%2B5W9gv61iDI%2BKiPmgTSCuqQ0n9cpf8XIhGMMEOmgd8jTmiVA2W4pFwA8CZXVmVCirJ4HzZdV9gqTOy1am5spFcrltUirgmZzLlpMBELstyvp160JGphnKbG0RHYNqVDms3DKsoCbig5yIrq0A1dQWgrLqoSGwGsiYAXcRg0GZUOIqGFoqLeNOa442B%2BvP7zqsh0m0Ycl3ItL8njHJixt8xYsN0qdKWxHSbrLWIwTtPqB6%2B1CD4dyHcgX5Z8DQP0gnxbFDEbyHcgX076IrUX5FuPN9XlpO60KPbQg8C0qAIxcjYkSL%2F3l4So7UD4IJFVNXK9VYP1Mpgpr5I3G8wUdxM%2FKWJaDjKTxcXYEqUIHJ%2BLRRWnDrRXIK6xV2LPibxEYLKQZpeML2HeeBS5TdU7BnvldXJlwX8bOuiEs4CmgNIPYkABBBVMOC3%2F4RkOIwUISuLgDpyDYlh3r8q1rxHX1XRVsdTtmFdD76FhEu0FczPWAuFFFi4Cprqfl3ptRhrES71AA7g7yOe%2ByOfTgbuGhhWDiYUEomDIo6tHUIJ%2BNOC5A%2F%2BdClHQEVLM%2BtXZQr1guha5sgMdD3S8Q8u0BGW2j0%2FH1kDHAx0fQsdmboielI5RfS5zf6HezLoaQN63yp79AnkbGFnPY41Py8iiqMIB7R3Y81WgvdiEPEqAYcEuJ4R1c8Xu7cK6lsHUMTB00EcgF9UH4EkDcsPI9aIByR1kb19k7wnDdBHWdkT1E3nNl945gR5UX%2BV9QHUHXnx1KAKEKlRY6MDqBacNQY4D8e5PvIYh2Bzt%2BMTbohzKQLwD8bKSF%2FXB%2BYDqPdf9BW1zS2pAbd8qU%2FYLtRWxr56nFp6WfeuLOA1Q7cCTPYdqIdiZ3E%2FWCrMPsK0qcmy%2BKdgWGjqzjYxhWAroH26r1rumpOG2K%2Ft54dFbDMjtIIr7IYpPh9wioOk7Rbah9yJhSRW52gbsduDG14ogWEhVzFLgCFPP0II94TuRY3Ug5YGU25OyYaF%2BkPJQOGQg5cNI2dRwP0hZ5F%2FrO8q7tbsGnPetMmnPcN4GdtZpongf2Lk%2BDXlAfQcO7Tvqa5poJ4SgGQroAeorSst6IWBQswhG8%2Bk5okWEx%2FTk0od3Qk68WNg%2FQ1pq%2BsvVzf6w495lqrvp9DlhwmQeeTd%2FfarfsLvVMl6RCXttaVxTErqeuVlxkN%2Bo1QY2RblsVtQUMmjXHjYi10F5A2RRlW0qA%2F3lPFsZhbsfr6LQXTtJtvlxwdTVHZJdeXsfY6IJI7O2BIsGBd7UfNvwyrpqSCsTCTiK683mr96Tn%2FxT%2BlzatIEcbfdsoAf5lg21s1beum5XUfHylgw7JaT0LRnIiqGoTEoHsjQlJ5B9d0mEhmlxN1QB4G7Y3W6JYpKDvwjJyaYQnRCDpVXnE%2BqFsr4vfYhup5o6ezvZ1MHDTW%2BTOhoFTb57mnxBYxG5wKijUNUUZL1Q0IhuqGKLu6FsUlLfFil1uiXxAXSJj0SXlgV5usQH0KXohqpxfLpsEfdfUftZq1tgLiORDS1qM%2FhGyJ8msqVFBraozeAbIX%2FaBgjAgkZRm4H5HrNXQ8HVkLk6tf%2F3sqLKRlFANfKRa0f35%2FQUhGaqtzEEUzPKcGazdzstLNlbgZZNJsSYTPSRPOIkspoib8NRmdlEDkV2kxs6a4pUu9PbRz%2B5u12Ey3lIDpxwsSJsP%2FUDP3nuDKHUlXL9gGpUqtiOyhm9ik%2FukEKH2VESwMkmTKJKjhmRpeT4YEfno9HcS7%2FMLsnpFCL6%2B64BbLnKZvdquhfCWYvDvKZBOP9MSZm%2BObi4%2FvjuBQPQgOlI3o2sVmLNbPpbDILn0t93HfHoBt%2BsMibObb0SYyLNVASsqcpCNLX6XI8B0RwQzQHR3KuSDzR3OTFKcMEJIc367JCB4QeGHxh%2BvyQwZCht1O0j83iHbsrGRIMbL3rwnWIH2m0swLlw55T4wSkzTHNewtHDAjaGZC2VD2EBzWEBD46Q4wJmpAtBISVwR4OGYO%2BUPmRoYlHy2Mn4My93OXDmwJlH4UzN1FlbGOqwD%2BXD8TFTCRsZsyhpMHDmwJlH4UzLUCubIkGGSQEuwiFOy6eoT3xaCkofOHXg1ONot4ZJLM8SqzI1Vvuxnur1iu7euA3Lp3%2BuvMg%2BG6tnFwb9e4m9mJzuEj4FXuxEfmKTLgeen6zJaQgsQ%2Bqw%2BH6T40xgRP5lIE5889enWhDnuN4JBgIpVYVgnReEByeY%2Fp5xCNHmh%2BPaDB9qqiwhRpU6oFkMTS6ECgnKtalYTXmcJVU2Eq87Mq2v%2FDuQ6S9HphAYZI3Btf50ZPLlMRE2ldyVViZZU5VFsvV1aE9IskK%2F%2BECnkujUxNYudxMSYF0IY6FoNWQV59FF%2FmWGBl53nSQKZjBh4xbCCuArOaI85baif0FdyXeX7X706519PczxjTzXjxuspyGn91UaTz3L6VV1RTW26zvLvybYhtGedov1evvp8JAxQcUiHuOobxlyet8Uhx6lNIahK2yWn1VqK%2FGabqZxySy36dKQihYFu%2F2FPfdYouVIukTFVdq5EkkzNiY5e8aVv5hTJduf0reJHZso35Ox7dx5t1%2FoKnl7vQl8UOKHeTdzQ9RcIhMZOaiZCub1mNx9WbHNkSwRWO%2BSOdjQ4WRpqoFQELdBC9lp0gxCrv9Crg7D7ULIaUhRGcgAA0NkEJxAoeCl2pD0dXjSl36kZEQEeGNTN7bG5r45XwgUtajy2xm6qmCmYzUZXxdRZD%2BXTsukyI7ec08rdX6fPpoWcxX5sOlNp6louQZylLXnIlrYP70lxdYu12TFJxd%2FJVxDn%2FL7969nNCdhcuPFcQrMqRRz%2Bxav0wMY%2BeEhC5ZcDK4L3UQzFKs2wQmaxjbQ64SpFKZEZxdHLX%2BTKXLtMm2QvzndVMnEpX%2FWvwqtQJAHB%2BTkoeY3KRcqB0emjWNqsQNt1OVnAIvombVyRNVxRczwIuXoZIMEZNMRet4L8BzqSGUC6FVAFGgBInB89Nysdwv3ET23p1M%2FWfw3FMZ8q9Zrz0B0TNEhhnVVrCCedY9v55r17nEpwPlQ6v%2FXZcvjIOeIqi514X4q0Hkd99gQullf5EECUpuvdgNY%2B%2Bb5SiZYi7FWwF0FvKTymv8JljCDI9YBqj0cqs2Veun1uXTIOAEswJBK68pcOjQUHTF3E9S67AinFT8v6%2F0%2BnVQBAgwfSEBpzfps5%2B51vUGr%2B9VWH7nxEKwB1QM9zjpm0NHATwM%2FdcdPukV6DesqiveDu1pAuvlw5gFHjcSYpp5dFpBhTmeuN7PXKdfUFEtk2SV94EXeCs628Uh3SbKKU16e0LgkolU%2B%2BveKQ56OJmReEzppaLJeBaFNt%2F2YpNfRD7mhZuoQKD9WHUUrQV3VmLkFgrlFhUZSiVUyZM3tKyipu1XT%2Fy1906iyU6lUVtqpFHmB3h4TFTUhOmD4SBoc6l7wnbx54gf5K9VSSGOd%2Bpybmu0A%2FUh2AAKC%2BtHaAXV6RTdU9dZ1eve1BiyG06AFrJ0dZC9A5jGsgDyx8PXwHmjLe3Thfg28l7tB%2B8N7gDFIoQWZRKfWXGcabHAs6%2FXtKkqqSMYqOo0aIqTYK47EcfXRUWJnZ73239qM0GvMiC%2Be403TBMUHQrZpZAM9IprJzI8WpWTGTdjDyp7bi%2BzE85u148Ux%2FRiuyZ%2BJHdzZ%2B5W1ZeSO3DiI3TmMM0x%2Fz%2FbMYexAI%2BOTaAQoqmaqovgJCHaw9mEa2c7C08el0ev1NPDTGJwqka4810%2FjcGahT2k28m16ON7MCD3fX%2FqOb0f0lGD9tL1wS8UDsR5GrLogV%2Fb4xFpfi3koQ%2FDrpXdrpqEgRunoRe0BS5RVXANhzALvKdM5iVbq8upnJTitrlB7WVZwUJqe%2Fuwa9EY9syG5KW872BS0iDLP%2BlX0F%2B8jpCHR3Y68I5Ul8r4MxNBiXylNLZDHbTrES3evQwZqvptkUoBgp%2BegH9rWYBIcTcuChsaIJ1OQpnF0Nau4cx%2FItMZwLWyCsjWwMQMGK0AefepmD8wAmLuthiV1vyUVAU3jNSL8Uv3KtHTR3Y6sX0EgykIYqKFZwTItg1eJ9BcqWFSEN99NOi30wMmRSmqBk%2BPHerHKOzJPF4FaCil7EryLzx%2F%2Burm0pk%2FrTx%2BWY%2BvnEvwYwSM5CDCuzijEbDin9BnFw4x26%2FLJN1UrplRr56fpbkp7ELj7pqbUzJlyy6XtXN0vmFLhm6r1BsPBwOwXbx7ZBRb7JUw8TpsXbSrVxU5Y3J5WX8J1QjdcIv1YB1584MZWA%2Fb78h0ymAIyRKZkLFBOScB5xEdZe7I6yEXeye5Hqbn%2FcUkdDU3FIIdS%2Bq88FjTd3a1FKf1oQ9%2BFAAm8WdINp1lGNZPOEiBUx8%2F9gVmI1ZF1iE5TdETqBz%2FT0oOSYKZgNwYlGW3N1%2F3Umb3DfyCoin5LRbstU%2BYCpB0j%2BAfmTz2llqu10HLpdprt6YyR2S72TFcTyWwTTdUDYZCDLR2I%2Bf3t2C3c2%2BIbmNmYgejYjPu4q%2BQyABmbW4O7CRxbh16AMyu7%2FctXL5DFQrkMOzELteWNBk6zI0fAQbZnzhwRB%2BmO6U1nJ%2BUgjTMs1ReGh2J2qyyMJeVmcn1Wm7gBgAMvwBmhtn%2F56gWy2CfDwdsXRyIUWt5vuGLeCeobVTnFPGMKIrWyJMuIeY29SLtxbSdEc1qmj0HA7Ea3rcYOmfm0Vmoq5RlRZdUWaZDBVzozIvPUADlIiutHnkOEU%2BHIL%2BInU2tyRBOVojAIiI5KG%2BmIF0XQ%2FijMFHJw%2Fv6Pm3r0Y8A5usI5DLMqagyDp1EMkDBDrwugQ%2Fs7uXL%2FHD8%2Bfn98fLz%2Fbzn%2Bro1GPVmVf2HF1mDXEzbVorUPV4McdNzOfbu%2FWst2WjN3Z23wfWOuOHjZFFK3aNGUVaQnrznp2m6YQtzp34%2BJt4xzsZsHurzhqpMQ6Coz0XncSXnTOygI4e2isOQnNUy%2BTL89Lx4%2F44Xz8%2B%2Frf1fjUQ%2B8js3JcsaLMlWPmy0n1QDqLttVErglNeMOM8UxpGXcWYCJsgdot4XEXYDNSkrswZJbyLPHLK%2B2LUZeEt6%2FhrjWVHNHsXGkQ37D0u4qjdOCCSGdmi3hUCfK59Cl9T7f%2Fx8%3D%3C%2Fdiagram%3E%3C%2Fmxfile%3E#%7B%22pageId%22%3A%22jiXIkiBsy346j2COqR-b%22%7D)

## Documentação do Kubernetes

[Clique Aqui para ser redirecionado](https://github.com/fiap-8soat-tc-one/tc-backend-s4-k8s-iac/blob/main/README.md)
