![banner_Acesso-API-com-PHP](https://github.com/__________)
----

<img src="https://img.shields.io/static/v1?label=Status&message=complete&color=32CD32&style=for-the-badge"/>

<p align="center">
 <a href="#-welcome">Welcome</a> | 
 <a href="______">___________</a> | 
 <a href="#-features">Features</a> | 
 <a href="#criando-a-chave-para-autentica%C3%A7%C3%A3o">Criando a chave para autenticação</a> | 
 <a href="#estruturando-o-diret%C3%B3rio">Estruturando o diretório</a> | 
 <a href="#rodando-o-php-com-docker">Rodando o PHP com Docker</a> | 
 <a href="#criando-um-container---php">Criando um Container - php</a> | 
 <a href="#criando-um-container---db">Criando um Container - db</a> | 
 <a href="#conectando-containers">Conectando Containers</a> | 
 <a href="#usando-a-extens%C3%A3o-do-docker">Usando a Extensão do Docker</a> | 
 <a href="#-erros-poss%C3%ADveis">Erros Possíveis</a> | 
 <a href="#-d%C3%BAvidas">Dúvidas</a> | 
 <a href="#%EF%B8%8F-contatos">Contatos</a> | 
 <a href="#%EF%B8%8F-desenvolvedora">Desenvolvedora</a>
</p>

# 🤗 Welcome

Olá, seja muito bem vinda(o)! 

Tive a ideia de começar o desafio de #100DaysOfCode, além de compartilhar alguns projetos para quem tem interesse em aprender PHP#, por isso se trata de algo bem simples para quem gostaria de iniciar na programação ou precisa melhorar as suas habilidades.

<br>

## <img align="center" alt="Ana-Csharp" height="40" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/csharp/csharp-original.svg"> Acesso a API com PHP - Cotações do Dólar com PHP

📚 Aproveite o código desse exercício

👩‍💻 Refaça do seu jeito

😉 Se tiver qualquer dúvida, me contate

<br>

## ✅ Features

- [X] Chave para autenticação
- [X] Estrutura de diretório
- [X] PHP com Docker
- [X] Container php
- [X] Container db
- [X] Conexão entre Containers
- [X] Extensão do Docker
- [ ] Progress...

<br>

## Criando a chave para autenticação

- Criar cadastro no site da HG Brasil: https://hgbrasil.com/
- Crie uma chave, nomeie como preferir, selecione o tipo da chave como uso interno (no servidor ou aplicativo mobile) e clique em criar.
- Vá em chaves para verificar se funcionou.

<br>

## Estruturando o diretório

- Abra o Git Bash na sua pasta de Documentos
- Crie a pasta do seu projeto com o comando: mkdir projeto
- Acesse a pasta: cd projeto
- Crie uma pasta chamada app: mkdir app
- Acesse a pasta: cd app
- Crie uma pasta chamada config: mkdir config
- Crie uma pasta chamada modules: mkdir modules

###### 💬 Obs: Essa aplicação não utiliza framework.

- Saia da pasta: cd ..

- Digite: touch index.php app/config/config.php app/modules/hg-api.php
Explicação:
	- Vamos precisar do arquivo "index.php", 
	- Criando o "config.php" na pasta "app/config"
	- Criando o "hg-api.php" na pasta "app/modules"

- Utilize o atalho para abrir o Visual Studio Code digitando "code ." 

<br>

## Rodando o PHP com Docker

- Vamos rodar o PHP usando o Docker, por isso você precisar criar uma conta no Docker Hub para ter acesso aos repositórios de imagem.
- Realize o download do Docker desktop application.
- Quando o container é usado, ele instância a partir de uma imagem que está dentro do repositório no Docker Hub, você pode criar a sua, mas existem muitas opções já desenvolvidas para escolher.

- É necessário criar dois arquivos, por isso volte ao Git Bash e digite: touch Dockerfile docker-compose.yml

###### 💬 Obs: Quando trabalhamos com o Docker é possível usar apenas o terminal, mas se no momento você não quiser aprender os comandos do Docker, você pode instalar a extensão do Docker no VS Code, assim você poderá utilizar o Docker pela própria interface do VS.

<br>

## Criando um Container - php

- Abra no Visual Studio Code o arquivo do Dockerfile

###### 💬 Obs: O Dockerfile é o arquivo que você usa para criar as imagens, se trata de um arquivo de texto com vários comandos e a partir dele que é possível escrever e criar a imagem.
	
- Digite: 
   - 'FROM php: 7.2-apache'
	
###### 💬 Obs: Aqui informamos que vamos pegar a imagem "php" da versão 7.2 usando o "apache", buscando no repositório php do Docker Hub é possível verificar que essa imagem existe, logo é possível baixar a partir dela.
	
###### 💬 Obs: Caso você precise utilizar outras coisas que não venham instaladas nessa imagem, saiba que é possível rodar vários comandos e reconfigurar a imagem se precisar.

- Precisaremos dos seguintes comandos:
   - docker-php-ext-install mysqli: A extensão do MySQL para php
   - a2enmod rewrite: A extensão para utiliza o enmod rewrite para usar o htaccess.
	
- Adicionando as extensões o seu código deverá ficar assim:

```
FROM php: 7.2-apache
RUN docker-php-ext-install mysqli
RUN a2enmod rewrite
```

- Abra no Visual Studio Code o arquivo docker-compose.yml

###### 💬 Obs: O docker-compose.yml se trata de "como" você vai utilizar a imagem, ele é o arquivo do orquestrador, pois ele organiza todos os containers, no sentido real de orquestrar.

###### 💬 Obs: "yml" significa "YAML Ain't Markup Language", ou seja "YAML não é linguagem de marcação", mas é bem parecido, pois ele tem um padrão certo, então cada espaço precisa estar no lugar específico. A ideia é que ele seja de fácil leitura humana. 

-----------------------------------------------------


Próximos passos:
- Criar um container chamado "php"
- Ele será gerado a partir da pasta atual,  "build: .", a partir do ponto ele lerá o "Dockerfile" e rodará os comandos.
- Usar as portas do projeto, "ports:"
- Usar a porta da máquina que lerá a porta 80 do container, "80:80"
	- O mesmo para a porta HTTPS, "443:443"
- Cria um volume dentro do container, "volumes:"
- A pasta no container que "var/www/html" do apache, será lida da pasta local  "./www" que é a do projeto onde se encontra a "index", e a partir disso é possível rodar o container e tudo o que for feito na área de desenvolvimento será refletido dentro do container.

O código deverá ficar assim:

```
php:
	build: .
	ports:
	- "80:80"
	- "443:443"
	volumes:
	- ./www:/var/www/html
```

<br>

## Criando um Container - db

Realize os seguintes passos no arquivo docker-compose.yml:
- Criar um novo container chamado "db" usando a imagem do Docker Hub MySQL da versão 5.7
- Cria um volume dentro do container, "volumes:"
- Usar variáveis de ambiente que já serão setadas, como a senha do Root
- Criar um database chamado "mydb"

Essa parte do código deverá ficar assim:

```
db:
	image: mysql:5.7
	volumes:
	- /var/lib/mysql
	enviroment:
	- MYSQL_ROOT_PASSWORD=myrootpass
	- MYSQL_DATABASE=mydb
```

<br>

## Conectando Containers

Para conectar os dois containers e fazer eles se conversarem, precisa adicionar "links", assim eles terão um relacionamento na rede.

```
links:
	- db
```

O código deverá ficar assim:

```
php:
	build: .
	ports:
		    - "80:80"
		    - "443:443"
	volumes:
		    - ./www:/var/www/html
	  links:
		    - db

db:
	image: mysql:5.7
	volumes:
		- /var/lib/mysql
	enviroment:
		- MYSQL_ROOT_PASSWORD=myrootpass
		- MYSQL_DATABASE=mydb
```

<br>

## Usando a Extensão do Docker

Para utilizar as funcionalidades da extensão do Docker instalada no VS Code basta clicar em cima do código no arquivo docker-compose com o botão direito do mouse e selecionar a opção "Compose Up", logo será aberto um Shell, os comandos irão rodar, as imagens disponíveis no Docker Hub serão baixadas e ficará tudo local, isso significa que quando tentarmos recriar a imagem, ele só fará o download novamente no caso de alguma imagem ter sido atualizada, e para visualizar basta clicar no ícone da extensão do docker localizado na barra lateral do Visual Studio Code.

<br>

## ‼ Erros Possíveis

O erro abaixo costuma acontecer quando é o utilizado TAB no arquivo yaml, pois isso não é permitido, substitua os locais onde tiver tab por espaços normais e rode novamente.

```
ERROR: yaml.scanner.ScannerError: while scanning for the next token
found character '\t' that cannot start any token
  in ".\docker-compose.yml", line 3, column 1
```

<br>

O erro abaixo costuma acontecer quando temos alguma versão do compose muito antiga, mas também pode ocorrer se a indentação não estiver correta, isso faz diferença no arquivo yaml, você verificar se os serviços estão no mesmo nível de indentação.

```
ERROR: The Compose file '.\docker-compose.yml' is invalid because:
Unsupported config option for texto: 'texto'
```

<br>

## ❓ Dúvidas

🚧 Código em construção 🚧

<div>
  <img align="right" alt="Computador Carregando" width="200px" src="https://media.giphy.com/media/d8d7kW0JUCUDwHpDsk/giphy.gif"/>
</div>

Se tiver qualquer dúvida, interaja aqui:
  * Faça perguntas
  * Dê sugestões de melhoria para o projeto
  * Compartilhe suas ideias
  * E interaja sobre o assunto

😉 Lembre-se de que esta é uma comunidade que construímos juntos 💪.

<br>

## ✉️ Contatos

Se precisar de ajuda, entre em contato comigo 😉

[<img align="left" alt="Gmail" width="80px" src="https://img.shields.io/badge/Gmail-D14836?style=for-the-badge&logo=gmail&logoColor=white"/>](mailto:anabe.valentim@gmail.com)
[<img align="left" alt="LinkedIn" width="100px" src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white"/>](https://www.linkedin.com/in/ana-beatriz-valentim)
[<img align="left" alt="Beacons" width="80px" src="https://github.com/AnaProgramando/AnaProgramando/blob/31ac40741768033915a37ec0f949984bf6aad2d1/beacons_logo.png"/>](https://beacons.page/anaprogramando)

<br>
<br>

## 🙋‍♀️ Desenvolvedora

<div>
  <img align="left" alt="Ana Valentim" width="100px" src="https://avatars.githubusercontent.com/u/31097110?v=4"/>
</div>

<br>
✏️ Feito com ❤️ e C# por <a href="https://github.com/AnaProgramando">Ana Valentim</a>.

💙 Se você gostou desse projeto, dê uma ⭐ e compartilhe!

<br><br>
[<img align="right" alt="Voltar ao topo" width="100px" src="https://img.shields.io/static/v1?label&message=Voltar+ao+topo&color=blueviolet&style=flat&logo"/>](https://github.com/AnaProgramando/_________/blob/main/README.md#)

<br>

<div>
  <img align="center" alt="Pixel-Art" width="1000px" src="____________"/>
</div>