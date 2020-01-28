### Documento de arquitetura do projeto de Mentoria 2020

## Projeto MTGDeckster - Marcelo Henrique Pillonetto

## **Índice**

1. Requisitos  
2. Escopo
3. Entregáveis
4. M/B/R
5. Diagramas UML
6. Versionamento
7. Defesa de tecnologia

## Requisitos

## Requisitos funcionais
* O App deve permitir que o usuário consiga buscar por qualquer carta de qualquer formato, aplicando variados filtros, como tipo de carta, custo de mana, texto presente na carta, etc.

* O App deverá conter um construtor de deck completo, disponibilizando para o usuário várias ferramentas para tal, como filtro de cartas por formato, cor e afins. Também deverá conter os templates básicos para criar decks para todos os formatos do MTG.

* A aplicação deverá conter um sistema de autenticação e uma tela de “meu perfil” com informações básicas sobre o usuário

* Na aplicação deverá existir uma área “minha coleção” contendo:
    1. Uma coleção de todas as cartas marcadas pelo usuário como “obtidas” (have)
    2. Uma coleção de todos os decks construídos pelo usuário
    3. Uma coleção de todas as cartas na lista de desejos do usuário (want)
    4. As listas Have e Want de troca para os usuarios

* A aplicação deverá conter uma funcionalidade de trocas de cartas entre os usuários, baseando-se nas listas de cada um


## Requisitos não-funcionais
### Requisitos do Produto
* O sistema deve ser compatível com uso mobile, podendo funcionar com acesso a internet intermitente
* Persistência dos dados

### Requisitos Externos
* O sistema deve fornecer endpoints em padrão REST para que uma possível aplicação web possa consumir

### Requisitos Organizacionais
* Não há requisitos desta natureza para o projeto em questão


## Escopo

#### Declaração do escopo
Para a entrega deste projeto, será necessário desenvolver um aplicativo multiplataforma que atenda aos requisitos previamente especificados. Dentre os esforços da equipe de desenvolvimento empregada, estarão:
* desenvolvimento de um banco de dados
* desenvolvimento de uma api (backend)
* projeto de design (UX/UI)
* desenvolvimento de aplicativo (frontend responsivo integrado)

Para tal, empregaremos as seguintes equipes:
* time de Desgin (XXXX, YYYY): projeto de design (UX/UI)
* time de arquitetura (ZZZZ, AAAA, BBBB): desenvolvimento de um banco de dados, desenvolvimento de uma api (backend)
* time de frontend (CCCC): 


#### Fora-de-escopo
Não se encontram no escopo desse projeto:
* integração com serviços de compra (e-commerce)
* análise dos dados da aplicação
* pesquisa por cartas não incluídas no Gatherer


## Entregáveis
São os entregáveis deste projeto: 
1. Este documento de arquitetura, descrevendo escopoe requisitos do projeto, 
bem como defesa e escalrecimento das escolhas de tecnologias empregas para o desenvolvimento do projeteo
2. O protótipo de design que, ao ser aprovado, servirá de respaldo para o time de desenvolvimento de frontend
3. A plataforma web integrada, apresentando as diversas funcionalidades satisfazendo os requisitos do contrato de projeto


## Relatório M/B/R

É importante lembrar que certos frameworks, bibliotecas utilizados ao longo do projeto (com exceção dos serviços de autenticação) serão de código aberto (open source).
Para fins de esclarecimento do projeto, fica-se implícito a decisão ReUse por este tipo de tecnologia (não iremos reconstruir o react native, por exemplo).
Para uma mais clara concepção do projeto,serão listadas e analisadas abaixo suas principais funcionalidades, de forma abstrata:

1. Ferramenta de busca
Esta é uma das principais funcionalidades do projeto, o motor da aplicação toda. Estará presente em todas as telas, excluindo as
de autenticação.

Resolução: BUY
Justificativa: O serviço de backend consumirá a api MTGApi <Link>, que provê acesso gratuito e instantâneo ao banco de dados
do Gatherer, oficial da Wizards of the Coast.  

2. Autenticação
Funcionalidade indispensável em qualquer projeto que necessite armazenar dados de usuários. Trata do login, cadastro e gerenciamento dos dados
de cada usuário

Resolução: BUY
Justificativa: por se tratar de uma parte central da segurança do app, é necessário que esta seja uma funcionalidade implementada tendo em vista
especialmente a segurança. Para tal, utilizar-se-ão ferramentas privadas de desenvolvimento e gestão de autenticação, com código e acesso protegidos fim-a-fim

3. Listagem e armazenamento dos dados
Funcionalidade básica de interação do próprio app

Resolução: MAKE
Justificativa: Funcionalidade básica que é muito intrínseca a cada software diferente, por isso será feita integralmente pela equipe da Radix

4. Função de trocas
É a funcionalidade realmente inovadora do app, permitindo que usuários visualizem as listas um do outro via QR Code
 
Resolução: MAKE
Justificativa: Não foram encontrados no mercado de implementações semelhantes ao casos de uso deste projeto. Dado prazo e recursos disponíveis,
foi estipulado que a equipe desenvolverá esta nova funcionalidade em tempo de entrega.


