# Como acessar um API com PHP - Cotação do Dólar:


## Criando a chave para autenticação

Criar cadastro no site da HG Brasil: https://hgbrasil.com/
Crie uma chave, nomeie como preferir, selecione o tipo da chave como uso interno (no servidor ou aplicativo mobile) e clique em criar.
Vá em chaves para verificar se funcionou.


## Estruturando o diretório

Abra o Git Bash em Documentos
Crie a pasta do seu projeto com o comando: mkdir projeto
Acesse a pasta: cd projeto
Crie uma pasta chamada app: mkdir app
Acesse a pasta: cd app
Crie uma pasta chamada config: mkdir config
Crie uma pasta chamada modules: mkdir modules

###### 💬 Obs: Essa aplicação não utiliza framework.

Saia da pasta: cd ..

Digite: touch index.php app/config/config.php app/modules/hg-api.php
	- Vamos precisar do arquivo "index.php", 
	- Criando o "config.php" na pasta "app/config"
	- Criando o "hg-api.php" na pasta "app/modules"

Digite "code ." para abrir o Visual Studio Code


## Rodando o PHP com Docker

Vamos rodar o PHP usando o Docker, por isso você precisar criar uma conta no Docker Hub para ter acesso aos repositórios de imagem.
Realize o download do Docker desktop application.
Quando o container é usado, ele instância a partir de uma imagem que está dentro do repositório no Docker Hub, você pode criar a sua, mas existem muitas opções para escolher.
 
É necessário criar dois arquivos, por isso volte ao Git Bash e digite: touch Dockerfile docker-compose.yml

> Quando trabalhamos com o Docker é possível usar apenas o terminal, mas se no momento você não quiser aprender os comandos do Docker, você pode instalar a extensão do Docker no VS Code, assim você poderá utilizar várias opções o Docker pela própria interface do VS.


## Criando um Container - php

Abra no Visual Studio Code o arquivo do Dockerfile

	> O Dockerfile é o arquivo que você usa para criar as imagens, se trata de um arquivo de texto com vários comandos e a partir dele que é possível escrever e criar a imagem.
	
Digite: 
	'FROM php: 7.2-apache'
	
	> Aqui informamos que vamos pegar a imagem "php" da versão 7.2 usando o "apache", buscando no repositório php do Docker Hub é possível verificar que essa imagem existe, logo é possível baixar a partir dela.
	
Caso você precise utilizar outras coisas que não venham instaladas nessa imagem, saiba que é possível rodar vários comandos e reconfigurar a imagem se precisar.

Nesse caso precisaremos dos seguintes comandos:
	> docker-php-ext-install mysqli: A extensão do MySQL para php
	> a2enmod rewrite: A extensão para utiliza o enmod rewrite para usar o htaccess.
	
O seu código deverá ficar assim:

```
FROM php: 7.2-apache
RUN docker-php-ext-install mysqli
RUN a2enmod rewrite
```

Abra no Visual Studio Code o arquivo docker-compose.yml

	> Se trata de "como" você vai utilizar a imagem. Ele é o arquivo do orquestrador, você pode rodar o Dockerfile u você pode rodar ele através do docker-compose, pois ele organiza todos os containers, no sentido de orquestrar.
	> "yml" significa "YAML Ain't Markup Language", ou seja "YAML não é linguagem de marcação", mas é bem parecido, pois ele tem um padrão certo, então cada espaço precisa estar no lugar específico. A ideia é que ele seja de fácil leitura humana. 

Nesse caso precisaremos:
	> Cria um container chamado "php"
	> Ele será gerado a partir da minha pasta atual, a partir do meu ponto,  "build: .", a partir daqui ele lerá o "Dockerfile" e rodará os comandos.
	> Usa as portas do projeto, "ports:"
	> Usa a porta da máquina, e lerá a porta 80 do container, "80:80"
	> O mesmo para a porta HTTPS, "443:443"
	> Cria um volume dentro do container, "volumes:"
	> A pasta no container que é a "var/www/html" que é a do apache, será lida da pasta local  "./www" que é a do projeto onde se encontra a  "index", e a partir disso é possível rodar o container e tudo o que for feito na área de desenvolvimento será refletido dentro do container.

O seu código deverá ficar assim:

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

Ainda no arquivo docker-compose.yml

Nesse caso precisaremos:
	> Cria um novo container chamado "db"
	> Usando a imagem do Docker Hub MySQL da versão 5.7
	> Cria um volume dentro do container, "volumes:"
	> Serão usadas algumas variáveis de ambiente
	> Elas já serão setadas, como a senha do Root
	> Cria um database chamado "mydb"

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

O seu código deverá ficar assim:

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

## Extensão

Agora para utilizar as funcionalidades da extensão do Docker instalada no VS Code basta clicar em cima do código no arquivo docker-compose com o botão direito do mouse e selecionar a opção "Compose Up", logo será aberto um Shell, rodará os comandos, irá baixar as imagens disponíveis no Docker Hub.
Quando damos o "Compose Up" são baixadas as imagens e fica tudo local, isso significa que quando tentarmos recriar a imagem, ele só fará o download uma vez e baixará novamente no caso de alguma imagem ter sido atualizada.
Para visualizar isso clique no ícone da extensão do docker localizada na barra lateral do seu Visual Studio Code.






## Erros

> O erro abaixo costuma acontecer quando é o utilizado TAB no arquivo yaml, pois isso não é permitido, substitua por espaços e rode novamente.

```
ERROR: yaml.scanner.ScannerError: while scanning for the next token
found character '\t' that cannot start any token
  in ".\docker-compose.yml", line 3, column 1
```

---

> O erro abaixo costuma acontecer quando temos alguma versão do compose muito antiga, mas também pode ocorrer se a indentação não estiver correta, isso faz diferença no arquivo yaml, um exemplo seria verificar se os serviços estão no mesmo nível de indentação.

```
ERROR: The Compose file '.\docker-compose.yml' is invalid because:
Unsupported config option for texto: 'texto'
```

---------------------------------------------------------------------

PEQUISAR SOBRE COMO INSTALAR E USAR O DOCKER

Fontes:
Código Fonte TV - Ambientes Back-End com Docker + VS Code // Mão no Código #3: https://youtu.be/97jWpWp4Pnc
	10:36

Código Fonte TV - Aprenda a Usar APIs // Mão no Código #7: https://youtu.be/lc0VOosnlAc
	7:48
![image](https://user-images.githubusercontent.com/31097110/129889702-22c82982-0747-44e3-88f9-816d90ff6646.png)
