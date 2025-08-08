# Como Executar o Projeto

Link do meu projeto: [LP4 - GitHub](https://github.com/joaorizzont/LP4)

## Passos

### 1 - Instale o Docker
Comece no Docker Desktop, depois vá para o CLI.

- [Instalação no Windows](https://docs.docker.com/desktop/setup/install/windows-install/)
- [Instalação no Linux](https://docs.docker.com/desktop/setup/install/linux/)

### 2 - Abra o Docker Desktop
Se não abrir, você vai receber uma mensagem de erro quando tentar rodar os comandos.

### 3 - Configurando Dockerfile (PHP + MySQL) e o index.php

Crie dois arquivos na raiz do seu projeto:
- `Dockerfile`
- `docker-compose.yml`

#### Dockerfile
```dockerfile
FROM php:8.2-apache

RUN apt-get update && apt-get install -y \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libmariadb-dev-compat \
    && docker-php-ext-install pdo pdo_mysql \
    && docker-php-ext-enable pdo_mysql \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

WORKDIR /var/www/html

COPY src/ .

EXPOSE 80
```

#### docker-compose.yml
```yaml
services:
  web:
    build: .
    ports:
      - "3333:80"
    volumes:
      - ./src:/var/www/html
  mysql:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: password
      MYSQL_DATABASE: dblp
      MYSQL_USER: user
      MYSQL_PASSWORD: password
    ports:
      - "3306:3306"
    volumes:
      - mysql_data:/var/lib/mysql

volumes:
  mysql_data:
```

> O arquivo **Dockerfile** define os comandos que serão executados quando os containers subirem, e o **docker-compose.yml** define quais imagens e serviços serão usados.

Essa configuração escuta o arquivo `index.php` dentro da pasta `src` do seu projeto.  
**Não esqueça** de criar o arquivo `/src/index.php`.

### 4 - Rodando o Projeto

Abra o terminal na raiz do seu projeto e execute:

```bash
docker-compose up -d
```

Sempre que desligar o Docker, será necessário rodar este comando novamente para iniciar os containers.

- `up` → inicia o Docker  
- `-d` → deixa o terminal livre (modo "detached"). Se não usar o `-d`, o Docker ficará rodando no terminal, e se você fechar o terminal, ele para.

### 5 - Acessando o Projeto

O projeto estará disponível em:
```
http://localhost:8080
```

> Não é necessário reiniciar o Docker para aplicar alterações. Basta salvar os arquivos.
