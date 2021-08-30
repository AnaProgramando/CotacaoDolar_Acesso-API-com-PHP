# <img align="center" alt="Ana-Csharp" height="40" src="https://github.com/devicons/devicon/blob/00f02ef57fb7601fd1ddcc2fe6fe670fef3ae3e4/icons/php/php-plain.svg"> Como acessar um API com PHP - Cotação do Dólar:


Olá, seja muito bem vinda(o)!

📚 Aproveite o código desse exercício
<br>
👩‍💻 Refaça ele do seu jeito
<br>
😉 Se tiver qualquer dúvida, me contate

## Criando a chave para autenticação

- Criar cadastro no site da HG Brasil: https://hgbrasil.com/
- Crie uma chave, nomeie como preferir, selecione o tipo da chave como uso interno (no servidor ou aplicativo mobile) e clique em criar.
- Vá em chaves para verificar se funcionou.

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


## Rodando o PHP com Docker

- Vamos rodar o PHP usando o Docker, por isso você precisar criar uma conta no Docker Hub para ter acesso aos repositórios de imagem.
- Realize o download do Docker desktop application.
- Quando o container é usado, ele instância a partir de uma imagem que está dentro do repositório no Docker Hub, você pode criar a sua, mas existem muitas opções já desenvolvidas para escolher.

- É necessário criar dois arquivos, por isso volte ao Git Bash e digite: touch Dockerfile docker-compose.yml

###### 💬 Obs: Quando trabalhamos com o Docker é possível usar apenas o terminal, mas se no momento você não quiser aprender os comandos do Docker, você pode instalar a extensão do Docker no VS Code, assim você poderá utilizar o Docker pela própria interface do VS.


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

## Usando a Extensão do Docker

Para utilizar as funcionalidades da extensão do Docker instalada no VS Code basta clicar em cima do código no arquivo docker-compose com o botão direito do mouse e selecionar a opção "Compose Up", logo será aberto um Shell, os comandos irão rodar, as imagens disponíveis no Docker Hub serão baixadas e ficará tudo local, isso significa que quando tentarmos recriar a imagem, ele só fará o download novamente no caso de alguma imagem ter sido atualizada, e para visualizar basta clicar no ícone da extensão do docker localizado na barra lateral do Visual Studio Code.

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


## 🚧 Código em construção 🚧

<div>
  <img align="right" alt="Computador Carregando" width="200px" src="https://media.giphy.com/media/d8d7kW0JUCUDwHpDsk/giphy.gif"/>
</div>

Se tiver qualquer dúvida, interaja aqui:
  * Faça perguntas
  * Dê sugestões de melhoria para o projeto
  * Compartilhe suas ideias
  * E interaja sobre o assunto

😉Lembre-se de que esta é uma comunidade que construímos juntos 💪.
