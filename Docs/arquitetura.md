### Documento de arquitetura do projeto de Mentoria 2020

# Projeto MTGDeckster - Marcelo Henrique Pillonetto

## **Índice**

1. Requisitos  
2. Escopo
3. Entregáveis
4. M/B/R
5. Diagramas UML
6. Versionamento
7. Defesa de tecnologia

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

