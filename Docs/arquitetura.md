### Documento de arquitetura do projeto de Mentoria 2020

# Projeto MTGDeckster - Marcelo Henrique Pillonetto

## **Índice**

1. Requisitos  
    1.1. Requisitos BDD
2. Escopo
3. Entregáveis
4. M/B/R
5. Diagramas UML*
6. Versionamento
7. Defesa de tecnologia

*encontram-se em anexo


## Requisitos

### Requisitos funcionais
* O App deve permitir que o usuário consiga buscar por qualquer carta de qualquer formato, aplicando variados filtros, como tipo de carta, custo de mana, texto presente na carta, etc.

* O App deverá conter um construtor de deck completo, disponibilizando para o usuário várias ferramentas para tal, como filtro de cartas por formato, cor e afins. Também deverá conter os templates básicos para criar decks para todos os formatos do MTG.

* A aplicação deverá conter um sistema de autenticação e uma tela de “meu perfil” com informações básicas sobre o usuário

* Na aplicação deverá existir uma área “minha coleção” contendo:
    1. Uma coleção de todas as cartas marcadas pelo usuário como “obtidas” (have)
    2. Uma coleção de todos os decks construídos pelo usuário
    3. Uma coleção de todas as cartas na lista de desejos do usuário (want)
    4. As listas Have e Want de troca para os usuarios

* A aplicação deverá conter uma funcionalidade de trocas de cartas entre os usuários, baseando-se nas listas de cada um


### Requisitos não-funcionais
#### Requisitos do Produto
* O sistema deve ser compatível com uso mobile, podendo funcionar com acesso a internet intermitente
* Persistência dos dados

#### Requisitos Externos
* O sistema deve fornecer endpoints em padrão REST para que uma possível aplicação web possa consumir

#### Requisitos Organizacionais
* Não há requisitos desta natureza para o projeto em questão

### Requisitos no padrão BDD
#### Contexto:
Dado que eu não esteja logado:

**Cenário: Logar no App** 
    Quando eu insiro no aplicatio minha senha, 
    Devo ser autenticado no sistema 
    Então redirecionado para a tela <Buscar Card>

**Cenário: Cadastrar-me no App**
    Quando insiro meus dados no aplicativo
    E concordo com os termos
    Devo ser cadastrado, 
    Autenticado,
    Então sou redirecionado para a tela <Buscar Card>

#### Contexto: 
Dado que eu esteja logado:

**Cenário: Editar perfil**

    Quando estou na tela <Meu Perfil>
    E seleciono editar
    E edito os campos aos quais sou apresentado
    Então sou redirecionado a tela <Meu Perfil>

**Cenário: Adicionar amigos**

    Quando estou na tela <Meu Perfil>
    E seleciono a "opcao amigos"
    E clico em "adicionar amigos"
    E preencho o campo de busca
    E seleciono um dos resultados/busco novamente
    Então o amigo é adicionado e retorno a tela <Meu Perfil>

**Cenário: Busca simples de card**

    Quando eu estou na tela <Buscar Card>
    E digito qualquer palavra no input
    Então redirecionado a tela de <Resultados Busca Card>

**Cenário: Busca avançada de card**

    Quando estou na tela de <Busca Avançada>
    E preencho quantos campos quiser
    Então ou redirecionado a tela de <Resultados Busca Card>

**Cenário: Consultar/editar coleção**

    Quando estou na tela <Minha Coleção>
    E preencho os filtros (ou nao)
    Então consulto todas as cartas resultantes da busca

**Cenário: Criar Lista**

    Quando estou na tela <Criar Lista>
    E preencho os campos básicos de tipo de lista
    Então tenho que ser redirecionado para a tela <Detalhe Lista>

**Cenário: Editar Lista**

    Quando estou na tela <Detalhe Lista>
    E busco a carta na tela <Busca Avançada> 
    E seleciono cartas para adicionar/remover
    E sou redirecionado para <Detalhe Lista> 
    Ou clico em {+}/{-} ao lado da carta em <Detalhe Lista>
    Então o app deve re-renderizar a lista atualizada e redirecionar para a tela <Detalhe Lista>

**Cenário: Nova Troca (via busca)**

    Quando estou na tela <Nova Troca>
    E seleciono a opção buscar usuário ou trocar com amigo
    E sou redirecionado para a tela <Efetuar Troca>
    E ambos concordamos com a troca
    E sou redirecionado a tela <QR Code confirmacao> 
    E o usuario alvo concorda com a troca
    Então sou redirecionado a tela <Historico de Trocas>
    
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
Justificativa: por se tratar de uma parte central da segurança do app, é necessário que esta seja uma funcionalidade implementada tendo em vista princpipalmente a segurança. Para tal, utilizar-se-ão ferramentas privadas de desenvolvimento consolidadas third party de gestão e autenticação,com código e acesso protegidos fim-a-fim.

3. Listagem e armazenamento dos dados
Funcionalidade básica de interação do próprio app

Resolução: MAKE
Justificativa: Funcionalidade básica que é muito intrínseca a cada software diferente, por isso será feita integralmente pela equipe da Radix

4. Função de trocas
É a funcionalidade realmente inovadora do app, permitindo que usuários visualizem as listas um do outro via QR Code
 
Resolução: MAKE
Justificativa: Não foram encontrados no mercado de implementações semelhantes ao casos de uso deste projeto. Dado prazo e recursos disponíveis,
foi estipulado que a equipe desenvolverá esta nova funcionalidade em tempo de entrega.

## UML
Os diagramas encontram-se nomeados e em anexo.


## Versionamento
   Todo projeto de tecnologia deve aderir a um padrão de versionamento, visando permitir a colaboração
proveniente de múltiplas fontes no projeto. Sendo assim, devem ser escolhidos dois paradigmas:

* linguagem de versionamento
* __worflow__ de versionamento

    Em se tratando de linguagens de versionamento, a com maior disponibilidade, adesão de mercado e versatilidade é o [git](https://git-scm.com/). Por estas razões, esta será a linguagem de versionamento utilizada para intermediar todos os processos feitos nos repositórios remotos do projeto, que será hospedado no [github](https://github.com/mhpillonetto/MTGDS) deste.
    
    Escolhido o git como linguagem, basta escolher dentre os paradigmas de versionamento disponíveis o mais adequado para a melhor condução do projeto. Tendo em mente quesitos como facilidade de aderencia do mercado, suporte da comunidade, casos de sucesso e agilidade de implementaáão, a ferramenta escolhida foi o [github-flow](https://guides.github.com/introduction/flow/). Um dos pontos mais atraentes desta variante __lightweight__ do gitflow original é sua facilidade de implementação para projetos de escala reduzida, como é o caso deste.

## Relatório de defesa de tecnologia
__Encontra-se anexo o mapa de tecnologias utilizadas no projeto, que serve de __overview__ da aplicação como um todo__

A defesa será feita em cada instância do projeto, cobrindo:

1. Banco de dados
2. Uso de ORM
3. Paradigma de Renderização web
4. Linguagem/Framework Backend
5. Linguagem/Framework Frontend
6. DevOps & Cloud

### Defesa
#### 1 - Banco de dados
Primeiramente, a discussão é de paradigma geral: __Relacional x Não Relacional__
    A decisão por um banco de dados *SQL*(Structured Query Language) veio devido ao fato de que se trata de uma aplicação de pequeno porte, com base de usuário de fácil determinação e relacionamento trivial entre suas classes (como evidenciado pelo diagrama de classes - em anexo). Utilizando um banco de dados SQL, aproveita-se das propriedades ACID deste, bem como evitam-se alguns pntos negativos do NoSQL:
        - Falta de ferramentas de análise e teste de desempenho mobile
        - Falta de padronização de linguagem
    
   Escolhido o SQL, agora basta definir um banco específico para a implementação deste. Para isso foi escolhido o *SQLite*. Extremamente leve e ágil, atendendo com primor os requisitos de armazenamento e requisição do app, sem onerar a performance e ocupação em memória deste.

#### 2 - Uso de ORM


#### 3 - Paradigma de Renderização web
   Ao desenvolver uma aplicação, um dos paradigmas encontrados é envolvendo o processo de renderização das páginas/app, sendo os mais difundidos o *Server Rendering* e as *SPA's*.
    A escolha por uma SPA vem de ...

#### 4 - Linguagem/Framework Backend
A linguagem escolhida para o desenvolvimento backend foi o [Node.js](https://nodejs.org/en/). 
Para justificar a escolha da linguagem para esta função, elencam-se aos pontos abaixo:
* Universalidade de código (stack full JavaScript -- estado da arte)
* Fácil integração com banco de dados via ORM
* Ferramentas como o [webpack](https://webpack.js.org/) ajudam a reutilizar código fim-a-fim e mantem a coerência ao longo das diversas instâncias do sistema 
* Features específicas do JavaScript como funcoes assíncronas, que são essenciais e de extrema praticidade para o desenvolvimento web/mobile moderno

O Framwork de node.js escolhido para o desenvolvimento foi o [express](https://expressjs.com/pt-br/). Este foi escolhido por ter as seguintes caracteristicas:
* Rápido, agnóstico e minimalista, se tornou a ferramenta mais popular de desenvolvimento backend para JavaScript
* Com uma miríade de métodos utilitários HTTP e middleware a seu dispor, o express é ferramenta __go-to__ no desenvolvimento de API's seguindo o padrão REST

#### 5 - Linguagem/Framwework Frontend
A linguagem escolhida para o desenvolvimento frontend foi o JavaScript.
Para justificar a escolha da linguagem para esta função, elencam-se aos pontos abaixo:
*   Como se trata de uma SPA, a maioria do trabalho de rendereização será do frontend, o que pede para uma linguagem robusta e de capacidade de renderização rebuscada, que é o caso do JS.
*   É a linguagem mais utilizada atualmente para frontend, o que acarreta numa maior disponibilidade de frameworks especializados para dadas tarefas

O Framework de JavaScript escolhido para desenvolver o frontend foi o [ReactNative](https://facebook.github.io/react-native/). Este foi escolhido por ter as seguintes caracteristicas:
*   Um código, dois SOs: O react native compila o código JavaScript diretamente para o mobile do iOS(Objective C) e Android (Java)
*   Feito especialmente para rodar SPAs no mobile
*   Estado da arte em desenvolvimento mobile, com diversas bibliotecas e uma vasta comunidade fornecendo-as suporte
*   features como AsyncStorage, que permitem facilmente uso de cache e redução do consumo de dados
*   código leve e orientação a componentes garantem DRY 

#### 6 - DevOps & Cloud
Serão analizadas cada uma das tecnologias implementadas separadamente
como referência recomenda-se o site da AWS 
e o documento de mapeamento de serviços em anexo (DevOps_MTGDS.png)

**AWS Elasitc Beanstalk:**

- Fornece ambiente simples e de fácil implementação 
- Fornece provisionamento dinâmico facilmente configurado
- A aplicação tem dimensão suficinete para permanecer em sua __free-tier__
- Provê escalabilidade conforme o necessário

**AWS Elastic Computing Cloud (EC2):**

- Elimina a necessidade de investir em hardware inicialmente, permitindo desenvolver e implantar o aplicativo com mais rapidez.
- Servidor versáril que comporta diversas aplicacoes rodando em Docker, Apache, etc
- Várias configurações de capacidade de CPU, memória, armazenamento
- A aplicação tem dimensão suficinete para permanecer em sua __free-tier__

**AWS Relational Database Service (RDS):**

- Oferece capacidade econômica e redimensionável (free tier, novamente...)
- Automatiza tarefas demoradas de administração, como provisionamento de hardware, configuração de bancos de dados, aplicação de patches e backups
- Compatível com diversos mecanismos, inclusive SQLite
- Backup confiável de dados

**AWS Simple Storage Service (S3):**

- Serviço de storage simples e barato
- ideal para dados __cold__, de alta latencia e baixa frequencia de acesso
- manuseio de backups automatizado
    
