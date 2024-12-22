# Extraction and loading

## 1. Create virtual Enviroment
Criando o ambiente virtual no linux para poder instalar as bibliotecas do projeto.

```bash
python3 -m venv .venv
source .venv/bin/activate
```

## 2. Install requeriments
Instalando as bibliotecas presentes no requeriments.

```bash
pip install -r requeriments.txt
```

## 3. Create meltano Project
Inicializando o projeto do meltano para criar espaço de 'extract' e 'load'.

```bash
meltano init adventure-works-postgres
```

## 4. Add postgres extractor
Como a fonte do Adventure Works (AW) se encontra no postres, é necessário utilizar tap-postgres, para que o meltano
adicione o extractor para o postgress.

```bash
meltano add extractor tap-postgres

meltano add loader target-snowflake
```

## 5. Create .env file with postgres and snowflake credentials

```bash

export TAP_POSTGRES_DATABASE='postgres_database'
export TAP_POSTGRES_HOST='postgres_host'
export TAP_POSTGRES_PASSWORD='postgres_password'
export TAP_POSTGRES_PORT='5432'
export TAP_POSTGRES_USER='postgres_user'

export TARGET_SNOWFLAKE_ACCOUNT='zzzzzz-gb0000'
export TARGET_SNOWFLAKE_DATABASE='ADVENTURE_WORKS_RAW_DB'
export TARGET_SNOWFLAKE_PASSWORD='password'
export TARGET_SNOWFLAKE_ROLE='ACCOUNTADMIN'
export TARGET_SNOWFLAKE_USER='snowflake_user'
export TARGET_SNOWFLAKE_WAREHOUSE='COMPUTE_WH'

```

Depois de configurar as credenciais, rodar o 'source .env' no terminal para que ele identifique as credenciais no 'meltano.yml'.

## 6. Loading files in snowflake
A função meltano 'elt' faz com que o pipeline rode, como não adicionamos a etapa de transformação, o meltano vai entender que
funciona como 'el' e realizará a extração do postgres e junto a isso, direcionar os arquivos para o database no snowflake.

```bash
meltano elt tap-postgres target-snowflake
```
