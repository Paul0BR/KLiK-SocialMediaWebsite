<p align="center">
  <img src="_git%20assets/cover.png" width="600" align="center"/>
</p>

> **`Projeto Abandonado`**. Este projeto foi abandonado e pode estar desatualizado em relação aos padrões atuais e em termos de segurança e recursos, etc. Posso reviver este projeto no futuro, mas não há planos de desenvolvimento adicional no momento.

> KLiK é um sistema de Pool de Informações (ou simplesmente um site de mídia social) baseado em PHP, consistindo em um sistema de login/registro completo, sistema de perfil de usuário, sala de bate-papo, sistema de fórum e sistema de gerenciamento de blog/pesquisas/eventos.


# Sumário

# Tabela de Conteúdo

* [Instalação](#installation)
  * [Requisitos](#requirements)
  * [Passos de Instalação](#installation-steps)
  * [Primeiros Passos](#getting-started)
* [Funcionalidades](#Features)
* [Componentes](#Components)
  * [Linguagens](#Languages)
  * [Ambiente de Desenvolvimento](#Development-Environment)
  * [Banco de Dados](#database)
  * [SGBD](#DBMS)
  * [API](#api)
  * [Frameworks e Bibliotecas](#Frameworks-and-Libraries)
  * [Técnicas](#techniques)
  * [Plugins Externos](#external-plugins)
* [Detalhes](#details)
* [Arquivos da Aplicação](#application-files)
* [Melhorias Futuras](#future-improvements)
* [A Equipe](#the-team)


## Instalação

#### Requisitos
* PHP
* Servidor Apache
* Banco de Dados MySQL
* SQL
* phpMyAdmin

> Todos esses requisitos podem ser atendidos de uma vez simplesmente instalando uma pilha de servidor como `Wamp` ou `Xampp` etc.

#### Passos de Instalação
1. Importe o arquivo `klik_database.sql` na pasta `includes` para o phpMyAdmin. Não há necessidade de nenhuma alteração no arquivo .sql. Isso criará o banco de dados necessário para o funcionamento da aplicação.

2. Edite o arquivo `dbh.inc.php` na pasta `includes` para criar a conexão com o banco de dados. Altere a senha e o nome de usuário para os que estão sendo usados na instalação atual do `phpMyAdmin`. Não há necessidade de alterar mais nada.

```php
$serverName = "localhost";
$dBUsername = "root";
$dBPassword = "examplePassword";
$dBName = "klik_database";

$conn = mysqli_connect($serverName, $dBUsername, $dBPassword, $dBName, 3307);

if (!$conn)
{
    die("Connection failed: ". mysqli_connect_error());
}
```

> O número da porta não precisa ser alterado em circunstâncias normais, mas se você estiver enfrentando um problema ou a pilha do servidor estiver instalada em outra porta, sinta-se à vontade para alterá-la, mas faça-o com cuidado.

3. Edite o arquivo `email-server.php` na pasta `includes` e altere as variáveis de acordo:

  * `$SMTPuser` : endereço de e-mail no `gmail`
  * `$SMTPpwd` : senha do endereço de e-mail
  * `SMTPtitle` : nome hipotético da empresa
  * `Domain` : Domínio do site, como localhost em um servidor local ou se em um domínio ativo, algo como www.hypotheticalwebsite.com

```php
$SMTPuser = 'klik.official.website@gmail.com';
$SMTPpwd = 'some-example-password';
$SMTPtitle = "KLiK inc.";
$Domain = 'localhost';
```
> Este passo é principalmente para configurar uma conta de e-mail para habilitar o sistema de `contato` e `redefinição de senha`, os quais exigem envio de e-mails.

> Na fase atual da aplicação, apenas contas do `Gmail` são suportadas.


#### Primeiros Passos
O arquivo do banco de dados já contém muitos dados de exemplo e usuários. A maioria dos usuários no banco de dados tem a mesma senha que seus nomes de usuário, exceto por alguns. Não é possível se registrar como administrador através da aplicação, pois decidimos que era uma fraqueza explorável. Portanto, você terá que criar uma conta e ir manualmente para a tabela `users` no banco de dados para alterar o userLevel dessa conta de `0` para `1`.
> Nível 0 significa um usuário normal e Nível 1 significa administrador

Uma maneira simples de acessar todas as contas de exemplo sem excluí-las e, portanto, perder todos os dados de exemplo é alterar manualmente o `email` delas dentro do phpMyAdmin para um endereço de e-mail válido. Em seguida, tente fazer login com essa conta usando uma senha incorreta e use o link `esqueceu a senha?` fornecido para redefinir a senha da conta. O e-mail da conta pode ser alterado novamente com segurança para algo trivial mais tarde.


## Funcionalidades

* [Painel da Aplicação](#application-Dashboard)
* [Login/Registro e Autenticação de Usuário](#Login-Registration-and-User-Authentication)
* [Sistema de Perfil de Usuário](#user-profile-system)
* [Caixa de Entrada/Sala de Chat](#chatroom-inbox)
* [Sistema de Fórum](#management-systems)
* [Sistema de Gerenciamento de Blog](#management-systems)
* [Sistema de Gerenciamento de Enquetes/Votação](#management-systems)
* [Sistema de Gerenciamento de Eventos](#management-systems)
* [Segurança](#security)


## Componentes

#### Linguagens
```
PHP 5.6.40
SQL 14.0
JavaScript ES 6
HTML5
CSS3
```

####  Ambiente de Desenvolvimento
```
WampServer Stack 3.0.6
Windows 10
```

#### Banco de Dados
```
MySQL Database 8.0.13
```

#### Sistema de Gerenciamento de Banco de Dados (SGBD=DBMS)
```
phpMyAdmin 4.8.3
```

#### API
```
MySQLi APIs
```

#### Frameworks e Bibliotecas
```
JQuery v3.3.1
BootStrap v4.2.1
```

#### Técnicas
```
AJAX
```

#### Plugins Externos
```
[PHPMailer 6.0.6](https://github.com/PHPMailer/PHPMailer)
```
> Isso foi usado para criar um `servidor de e-mail` no `localhost do Windows`, já que, ao contrário do Linux, não há um já instalado no Windows. Este plugin foi usado para o envio e recebimento de e-mails no localhost e não é necessário em um domínio ativo.

## Detalhes

> Detalhes das Funcionalidades importantes da Aplicação

### Painel da Aplicação

<p align='center'>
  <img src="_git%20assets/dashboard.png" width="500" align="center"/>
</p>

O Painel fornece uma interface central para a maioria das funcionalidades da aplicação. O `cartão de perfil do usuário` no canto superior esquerdo da tela fornece um resumo do perfil, bem como um link para o perfil e para a página de edição de perfil. O botão de criador no canto superior direito fornece um link proeminente para a página da Equipe, que mostra os `Criadores do KLiK`.

A interface de 4 abas no centro fornece acesso aos `últimos`, ou mais recentemente criados `Fóruns`, `Blogs`, `Enquetes` e `Eventos`. Os componentes mostram as características individuais dos respectivos elementos, como total de `upvotes` para um fórum, `curtidas` para um blog, `votos` para uma enquete e `dias restantes` para eventos. Existem mais 2 botões, que levam aos `Fóruns KLiK` (a interface central para os Fóruns) e ao `Hub KLiK` (a interface central para o Sistema de Gerenciamento de Blog, Enquetes e Eventos).


### Sistemas de Gerenciamento

<p align="center">
  <img src="_git%20assets/management.png" width="600" align="center"/>
</p>

* `Sistema de Fórum`:
  * Criação de fórum
  * Responder / postar mensagens em um fórum
  * Categorias de fórum
  * Capacidade do administrador de criar categorias de fórum
  * Sistema de upvote/downvote em respostas de fórum (não é possível repetir a votação etc.)
  * Capacidade de excluir suas próprias respostas de fórum (o administrador pode excluir qualquer uma)

* `Sistema de Gerenciamento de Blog`:
  * Criação de blog
  * Escolha opcional de imagem de capa do blog (há uma imagem padrão)
  * Sistema de `Curtir` em blogs (usuários podem curtir o blog ou remover sua curtida)
  * Excluir próprios blogs (o administrador pode remover qualquer um)

* `Sistema de Gerenciamento de Eventos`:
  * Sistema de Criação de Eventos
  * Escolha opcional de imagem de capa do evento (há uma imagem padrão)
  * Título do Evento
  * Informações do Evento (opcional)
  * Definição da Data do Evento
  * Contagem regressiva em tempo real na página do Evento
  * Marcação automática como concluído ao passar a Data do Evento

* `Sistema de Gerenciamento de Enquetes`:
  * Criação de Enquete
  * Votação na enquete
  * Alterar / visualizar voto atual
  * Enquetes Bloqueadas / Abertas (o voto não pode ser alterado em enquetes bloqueadas)
  * Visualização de Resultados [total de usuários votando, nº de usuários votando em cada opção]
  * Página separada para visualizar cada opção de enquete juntamente com cada usuário que votou nela



### Login/Registro e Autenticação de Usuário

<p>
  <img src="_git%20assets/login.png" width="400" align="right"/>
</p>

KLiK suporta um sistema completo de login/registro e Perfil de Usuário. Ao iniciar, a aplicação mostra opções para fazer login, se inscrever ou entrar em contato com o administrador do site via e-mail. Cada usuário pode criar um nome de usuário único que não pode ser alterado posteriormente. As `senhas` dos usuários são `hashed` antes de serem armazenadas no banco de dados, para que nem mesmo os administradores tenham acesso às senhas originais. Informações adicionais do usuário incluem `Nome Completo`, `e-mail`, `Imagem de Perfil`, `Título do Perfil`, `Gênero` e `Bio`.

Existe também um `Sistema Seguro de Recuperação de Senha` que permite ao usuário redefinir suas senhas de forma segura. O aplicativo gera links de token temporários criptografados com um certo tempo de expiração que, quando usados pelo usuário, solicitam a alteração da senha. Como isso também exige a senha atual, o processo é seguro e tem menores chances de exploração.

O aplicativo usa vários métodos de autenticação para se inscrever e fazer login. Ele verifica `campos vazios`, `nome de usuário incorreto`, `senha incorreta`, `erros de SQL`, `erros de servidor` e, no caso de inscrição, `imagem corrompida` ou `erros de tipo de imagem`.


### Sistema de Perfil de Usuário

<p>
  <img src="_git%20assets/profile.png" width="350" align="left"/>
</p>

KLiK possui um `sistema completo de Perfil de Usuário`. Cada usuário recebe um perfil ao se inscrever, com o qual pode criar Fóruns, Blogs, Eventos etc. e interagir com as funcionalidades do aplicativo. O nome completo do usuário, título e bio, bem como a imagem de perfil, são opcionais, o que significa que qualquer pessoa pode se inscrever sem defini-los. Nesse caso, o usuário receberá uma imagem de usuário padrão e o título, bio e nome completo ficarão vazios.

O `perfil do usuário` pode ser acessado através da opção no menu de configurações na barra de navegação, ou mais simplesmente, clicando na imagem do usuário no cartão de perfil do usuário, que está presente no canto superior esquerdo da tela do aplicativo na maioria das páginas. A página de perfil mostra as informações básicas do usuário, como nome de usuário, nome completo, gênero, título e bio. Além disso, mostra os diferentes `Fóruns` e `Blogs` que o usuário criou, juntamente com as `Enquetes` em que participou. Se por acaso o usuário não tiver feito nada disso ou for novo, a página mostra um gatinho bongo fofo com a legenda 'tão vazio' para lembrá-lo de que você precisa ser mais ativo :)

Existe também um `Sistema de Edição de Perfil` que permite ao usuário editar suas informações de perfil. Ele pode ser acessado através da respectiva opção no menu de configurações na barra de navegação ou simplesmente clicando no ícone de lápis ao lado da imagem de perfil do usuário no cartão de perfil. O sistema permite que o usuário altere a maioria de suas informações, exceto o nome de usuário, que não pode ser alterado. Todos os campos já contêm as informações atuais, para que o usuário não precise digitar tudo novamente se ele só deseja editar ligeiramente as informações atuais. A senha também pode ser alterada, no entanto, apenas fornecendo a senha atual para reter uma interface mais segura.


### Sala de Chat / Caixa de Entrada

<p align="center">
  <img src="_git%20assets/inbox.png" width="600" align="center"/>
</p>

KLiK também possui uma caixa de chat, que usa `PHP` e `AJAX` para chat em tempo real com outros usuários. A seção à esquerda é uma lista de todos os usuários atualmente no site, enquanto a tela de chat à direita é para exibir as mensagens de entrada e saída. Um usuário pode acessar um chat com um determinado usuário clicando nele na lista de usuários, o que recuperará todas as mensagens de chat do banco de dados. As mensagens de entrada e saída são estilizadas de forma diferente para manter a legibilidade. O chat é feito em tempo real, sem a necessidade de atualizar a página continuamente.

**Possíveis Melhorias**:
* `otimização`: Todas as mensagens de um chat são recuperadas de uma vez, e isso pode causar atrasos se o chat for grande. Isso pode ser corrigido implementando o carregamento incremental de mensagens para carregar apenas as mensagens sendo exibidas na tela.
* `busca de usuário`: Uma funcionalidade de busca pode ser implementada na lista de usuários para buscar diretamente por um usuário específico, economizando tempo.

### Segurança

* `Hashing de senha` antes de armazenar no banco de dados.
* Redefinição de senha feita através de `tokens criptografados` criados individualmente e enviados por e-mail como um link. Os tokens têm uma data de expiração após a qual não podem ser usados.
* Filtragem de informações obtidas dos métodos `$_GET` e `$_POST` para prevenir `header injection`.
* Implementação de `MySQLi Prepared Statements` para segurança **avançada** do banco de dados.

  **Exemplo:**
```php
$sql = "select uidUsers from users where uidUsers=?;";
        $stmt = mysqli_stmt_init($conn);
        if (!mysqli_stmt_prepare($stmt, $sql))
        {
            header("Location: ../signup.php?error=sqlerror");
            exit();
        }
        else
        {
            mysqli_stmt_bind_param($stmt, "s", $userName);
            mysqli_stmt_execute($stmt);
            mysqli_stmt_store_result($stmt);
       }

```

## Arquivos da Aplicação

> Uma lista de todas as principais Funcionalidades da Aplicação e seus respectivos arquivos de front-end e back-end.

|&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; Funcionalidade &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;|Arquivos Front-end| Arquivos Back-end |
| ----------- | -- | -- |
|Painel| `index.php (Painel Principal)`, `Forum.php`, `Hub.php`| N/A|
|Sistema de Fórum| `categories.php`, `create-category`, `topics.php`, `create-topic.php`, `posts.php` |`create-category.inc.php`, `create-topic.inc.php`, `delete-category.php`, `delete-forum.php`, `delete-post.php`|
|Sistema de Blog| `blog-page.php`, `blogs.php`, `create-blog`| `blog-vote.inc.php`, `create-blog.inc.php`|
|Sistema de Eventos| `event-page.php`, `events.php`, `create-event.php`| `create-event.inc.php`|
|Sistema de Enquetes| `poll.php`, `polls.php`, `poll-voters.php`|`create-poll.inc.php`, `delete-poll.inc.php`, `poll.class.php`, `post-vote.inc.php`|
|Sala de Chat| `message.php`|`post_message_ajax.php`, `get_message_ajax.php`, `script.js`|
|Cadastro/ Login| `signup.php`, `login.php`| `signup.inc.php`, `login.inc.php`, `logout.inc.php`|
|Sistema de Perfil| `profile.php`, `edit-profile.php` | `profileUpdate.inc.php`|
|Redefinição de Senha| `reset-pwd.php`, `create-new-pwd.php`|`reset-request.inc.php`|`reset-password.inc.php`|
|Upload de Imagem| N/A| `upload.inc.php`|
|Showcase de Criadores| `team.php`, `KLiK_anas-imran.php`, `KLiK_anas-kamal.php`, `KLiK_saad.php`, `KLiK_ubaid.php`| N/A|
|Encontrando Usuários| `users-view.php`|N/A|

> **Nota:** Os arquivos GUI estão no `diretório raiz`, e os `arquivos de backend` estão presentes na pasta `includes`. Da mesma forma, todos os arquivos CSS e JS estão presentes em seus respectivos diretórios `css` e `js`. Apenas os arquivos dos Criadores na pasta `_KLiK Creators` têm seus próprios arquivos css. Os principais arquivos de estruturação HTML são o `HTML-head.php` e o `HTML-footer.php`, que também residem na pasta includes.


## Melhorias Futuras
* Otimização (em componentes como sala de chat)
* Integração de frameworks avançados como `Laravel`
* Implementação de `Vue.js` para sala de chat.
* Correções contínuas de bugs e melhorias

> Se você gostou do meu trabalho, por favor, mostre seu apoio marcando o repositório com uma estrela! Significa muito para mim, e é tudo o que peço.

## A Equipe

Um enorme agradecimento à maravilhosa equipe sem a qual todo este projeto não seria possível. Confira nossos perfis e marque nossos repositórios com uma estrela! :)

<img src="_git%20assets/me.png" width="150"/> | <img src="_git%20assets/kamal.png" width="150"/> | <img src="_git%20assets/ubaid.png" width="150"/> | <img src="_git%20assets/ait.png" width="150"/>
---|---|---|---
[msaad1999](https://github.com/msaad1999) | [skamal16](https://github.com/skamal16) | [UbaidAsim](https://github.com/aitasadduq) | [aitasadduq](https://github.com/aitasadduq)


---

### Tradução

Tradução para o português brasileiro por:

[![Paul0BR](https://github.com/Paul0BR.png?size=100)](https://github.com/Paul0BR)  
[Paul0BR](https://github.com/Paul0BR)

Caso encontre algum erro ou deseje sugerir melhorias, sinta-se à vontade para abrir uma *issue* ou um *pull request*.

