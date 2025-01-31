# Clean Architecture: Vantagens e Estrutura Detalhada

Clean Architecture, proposta por Robert C. Martin (Uncle Bob), é uma abordagem de design de software que visa criar sistemas altamente flexíveis, escaláveis e de fácil manutenção. Ela organiza o código em camadas de responsabilidade bem definidas, mantendo as regras de negócio isoladas dos detalhes de implementação técnica, como frameworks, bancos de dados ou interfaces externas.

## Estrutura do Clean Architecture

A arquitetura é organizada em torno de camadas concêntricas, onde a dependência entre as camadas segue a regra da dependência: **camadas externas podem depender de camadas internas, mas não o contrário**. A camada mais interna contém a lógica de negócios, enquanto a camada mais externa lida com frameworks, UI, e outras interfaces externas. As principais camadas são:

**Segue abaixo como foi estrutura a aplicação**

![image](https://github.com/fiap-8soat-tc-one/tc-backend-s2/blob/main/assets/clean-arch.png)

### 1. Entidades (Entities)

- São objetos de negócios que encapsulam as regras e políticas mais gerais do sistema.
- Representam o núcleo da aplicação e são independentes de frameworks e bibliotecas externas.
- **Exemplo**: No domínio de uma aplicação bancária, uma "Conta" é uma entidade que possui regras como o cálculo de saldo.

### 2. Casos de Uso (Use Cases)

- Esta camada contém a lógica específica de aplicação, como fluxos de operações que o sistema realiza (Ex.: criar uma conta, transferir dinheiro).
- É responsável por coordenar a interação entre as entidades, sem se preocupar com como as informações são exibidas ou armazenadas.
- **Exemplo**: Um caso de uso poderia ser a "Transferência de Fundos", que precisa orquestrar interações entre múltiplas entidades.

### 3. Adaptadores de Interface (Interface Adapters)

- Esta camada converte dados entre o formato que as camadas internas esperam e o formato das camadas externas (ex.: API, UI, banco de dados).
- Contém controladores, presenters e gateways que servem de ponte entre a lógica de negócios e os sistemas externos.
- **Exemplo**: Um adaptador de interface converteria uma requisição HTTP em uma chamada a um caso de uso.

### 4. Frameworks e Drivers (Infrastructure)

- A camada mais externa contém detalhes específicos da implementação, como frameworks, banco de dados, bibliotecas externas, APIs, etc.
- A ideia é que esses componentes sejam tratados como detalhes que podem ser facilmente substituídos.
- **Exemplo**: Um ORM como o Entity Framework estaria nesta camada, fornecendo a persistência de dados, mas sem impactar o núcleo da aplicação.

## Vantagens de Aplicar o Clean Architecture

### 1. Independência de Frameworks

- Clean Architecture coloca os frameworks e bibliotecas externas como "detalhes" e não como parte central da aplicação. Isso facilita a substituição de tecnologias sem grandes impactos na lógica do negócio.
- **Exemplo**: Se você está usando o Entity Framework para persistência de dados e decide trocar por outro ORM, isso não afetará as camadas de negócio.

### 2. Fácil Manutenção e Evolução

- Como as regras de negócios estão separadas dos detalhes de infraestrutura, o código torna-se mais fácil de manter. Mudanças na UI ou no banco de dados podem ser feitas sem impactar a lógica central da aplicação.
- **Exemplo**: Se precisar alterar a interface de usuário, não será necessário mudar os casos de uso ou as entidades.

### 3. Alta Testabilidade

- A separação entre camadas permite que cada uma seja testada de forma isolada. A camada de regras de negócio (entidades) pode ser testada independentemente da camada de banco de dados ou interface, permitindo a criação de testes unitários robustos.
- **Exemplo**: Pode-se testar as regras de transferência bancária sem precisar configurar um banco de dados real ou uma interface gráfica.

### 4. Facilidade para Mudanças e Escalabilidade

- Como a arquitetura é modular, é mais fácil escalar e introduzir novas funcionalidades. Você pode adicionar novos casos de uso sem afetar as outras partes da aplicação.
- **Exemplo**: Se o sistema bancário precisa de um novo tipo de operação, pode-se simplesmente criar um novo caso de uso sem modificar as entidades ou os adaptadores existentes.

### 5. Resiliência a Mudanças de Requisitos

- Os sistemas projetados com Clean Architecture são menos suscetíveis a mudanças, porque isolam as partes mais voláteis do sistema (UI, frameworks, etc.) das partes mais estáveis (regras de negócios). Isso protege o sistema contra grandes mudanças quando novos requisitos aparecem.
- **Exemplo**: Se a interface de entrada mudar de uma API para uma interface gráfica, o impacto será mínimo, pois as regras de negócio e os casos de uso já estão encapsulados e prontos para lidar com o novo adaptador.

### 6. Independência de Interface de Usuário

- A lógica de negócio não depende da interface com o usuário, o que permite substituir a UI sem impactar os casos de uso ou as regras de negócio. Isso é útil em cenários onde é necessário criar diferentes interfaces para web, mobile, e desktop.
- **Exemplo**: Um sistema bancário pode ter uma interface web e um aplicativo mobile, ambos interagindo com os mesmos casos de uso sem duplicar código.

### 7. Flexibilidade no Uso de Bancos de Dados

- Como o banco de dados está abstraído na camada de infraestrutura, você pode trocar de um banco de dados relacional para um banco NoSQL, por exemplo, sem impactar as camadas de negócio.
- **Exemplo**: Se você começar com um banco de dados MySQL e decidir migrar para MongoDB, a mudança afetará apenas a camada de infraestrutura, sem modificar a lógica de negócio.

### 8. Código mais Limpo e Organizado

- A separação clara entre responsabilidades e camadas de código leva a uma estrutura mais limpa e organizada. Isso também torna o código mais fácil de ler e entender por outros desenvolvedores.
- **Exemplo**: Um novo desenvolvedor pode rapidamente entender onde as regras de negócios estão localizadas, quais são os casos de uso e como o sistema interage com as partes externas.

## Desafios ao Implementar Clean Architecture

Embora as vantagens sejam muitas, a implementação de Clean Architecture pode trazer alguns desafios:

- **Complexidade Inicial**: Para sistemas pequenos, a implementação inicial pode parecer mais complexa do que necessário. A criação de múltiplas camadas e abstrações pode ser um overhead em projetos simples.
- **Curva de Aprendizado**: Entender completamente os princípios e aplicá-los corretamente demanda experiência e conhecimento das boas práticas de design de software.

No entanto, em sistemas que demandam manutenção contínua e que podem crescer com o tempo, os benefícios superam os desafios.

## Conclusão

A Clean Architecture oferece um modelo poderoso e flexível para projetar sistemas de software. Ao seguir os princípios dessa arquitetura, você cria aplicações que são independentes de frameworks, altamente testáveis, fáceis de manter e escaláveis. Para projetos complexos e de longo prazo, a aplicação de Clean Architecture garante uma base sólida que resiste a mudanças e facilita a evolução contínua do software.

## Referências

[The Clean Architecture - Ungle Bobo](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)