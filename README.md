Retail-Demo Tradução para Português Brasil
Traduzido por: Hylan Silva
Data de Tradução: 06/09/2022

# Exemplo de Bot para o segmento de Varejo

Este é um bot construído para o segmento de varejo, feito para ajudar a conhecer os conteúdos iniciais da tecnologia, exemplos de como implementar recursos específicos e casos de uso de amostras. Construído usando Rasa 2.3.1


## Instalando as dependências

Inicie no Terminal:
```bash
pip install -r requirements.txt
```


## Inicie o Bot

Use `rasa train` para treinar o modelo.

Depois, para executar, primeiro configure o seu servidor de ação em uma nova janela do terminal:
```bash
rasa run actions
```

Em outra janela, inicie o duckling server ( para extrair as entidades):
```bash
docker run -p 8000:8000 rasa/duckling
```

Em seguida fale com o seu com o seu bot enquanto ele está ligado
```bash
rasa shell --debug
```

Nota: vesse modo `--debug` produzirá um monte de saídas a ajudá-lo a entender como o bot está funcionando por cima. 
se você quiser apenas falar com o bot, você pode retirar esta flag.

## Visão geral dos arquivos

`data/stories.yml` - contém o enrredo

`data/nlu.yml` - contém os dados de linguagem natural para treino


`data/rules.yml` - contém as regras que fazem o bot responder as perguntas

`actions/actions.py` - contem o código de ações determinadas

`domain.yml` - o arquivo de domínio, incluindo modelos de resposta de bot

`config.yml` - configurações de treinamento para a trilha de instruções da linguagem natural e seus conjuntos de politicas.

`tests/test_stories.yml` - casos para teste ponta a ponta


### Coisas que você pode perguntar ao Bot

1. Verifique o estatus de uma ordem
2. Retorne um item
3. Apague an item
4. Procure um item no inventário de sapatos
5. Inscreva-se nas atualizações do produto

O Bot pode modificar formulários de computação e até cancelar um formulário, mas não pode retornar um formulário após a sua substituição.

Os flucos principais mostram o bot retornando ou alterando informações dentro de um banco de dados SQLite ( o arquivo `example.db`). Você pode usar o `initialize.db` para alterar os dades existentes nesses arquivos.

Para poder explicar, veja o exemplo a baixo, o bot faz orders para os seguintes indereços de e-mail:

- `example@rasa.com`
- `me@rasa.com`
- `me@gmail.com`

E retorna os dados sobre os sapatos de acordo com o que está no estoque (tamanho/cor):

```
inventory = [(7, 'blue'),
             (8, 'blue'),
             (9, 'blue'),
             (10, 'blue'),
             (11, 'blue'),
             (12, 'blue'),
             (7, 'black'),
             (8, 'black'),
             (9, 'black'),
             (10, 'black')
            ]
```

## Testando o Bot

Você pode iniciar a testar o bot e a interação com as conversas apartir de: `rasa test`.
este comando fará com que apareça o texto a seguir no prompt de comando:
 [o teste de ponta a ponta](https://rasa.com/docs/rasa/user-guide/testing-your-assistant/#end-to-end-testing) pegando as conversações dentro de: `tests/test_stories.yml`.

Observe que, se o duckling não estiver em execução ao fazer isso, você verá algumas falhas.

## Rasa X Deploy

Para [fazer o deploy do bot](https://rasa.com/docs/rasa/user-guide/how-to-deploy/), é altamente recomendável que você faça o uso de 
[script de deploy de uma linha](https://rasa.com/docs/rasa-x/installation-and-setup/one-line-deploy-script/) para o  Rasa X. Como parte da implantação, você precisará configurar a [integração com o git](https://rasa.com/docs/rasa-x/installation-and-setup/integrated-version-control/#connect-your-rasa-x-server-to-a-git-repository) para extrair seus arquivos e configurações, e com isso criar ou puchar uma imagem de ação do servidor.


## Inicio do Servidor de Ação

Você precisará ter instalado o Docker em ordem para poder iniciar o servidor. Se você não fez nenhuma alteração no código, também pode usar o [Arquivo do Docker](https://hub.docker.com/repository/docker/cdesmar/retail-demo) em vez de você mesmo contruir o seu.

É recomendado o uso de um [processo CI/CD automatizado](https://rasa.com/docs/rasa/user-guide/setting-up-ci-cd) para manter seus servidor de ação atualizado em um ambiente de produção.

---

# Retail Example Bot

This is a sample retail bot to help provide starter content, examples of how to implement particular features, and sample use cases. Built using Rasa 2.3.1

## Install dependencies

Run:
```bash
pip install -r requirements.txt
```

## Run the bot

Use `rasa train` to train a model.

Then, to run, first set up your action server in one terminal window:
```bash
rasa run actions
``` 

 In another window, run the duckling server (for entity extraction):
```bash
docker run -p 8000:8000 rasa/duckling
```

Then talk to your bot by running:
```
rasa shell --debug
``` 

Note that `--debug` mode will produce a lot of output meant to help you understand how the bot is working
under the hood. To simply talk to the bot, you can remove this flag.


## Overview of the files

`data/stories.yml` - contains stories

`data/nlu.yml` - contains NLU training data


`data/rules.yml` - contains the rules upon which the bot responds to queries

`actions/actions.py` - contains custom action/api code

`domain.yml` - the domain file, including bot response templates

`config.yml` - training configurations for the NLU pipeline and policy ensemble

`tests/test_stories.yml` - end-to-end test stories


## Things you can ask the bot

1. Check the status of an order
2. Return an item
3. Cancel an item
4. Search a product inventory for shoes
5. Subscribe to product updates

The bot can handle switching forms and cancelling a form, but not resuming a form after switching yet.

The main flows have the bot retrieving or changing information in a SQLite database (the file `example.db`). You can use `initialize.db` to change the data that exists in this file.

For the purposes of illustration, the bot has orders for the following email addresses:

- `example@rasa.com`
- `me@rasa.com`
- `me@gmail.com`

And these are the shoes that should show as in stock (size, color):

```
inventory = [(7, 'blue'),
             (8, 'blue'),
             (9, 'blue'),
             (10, 'blue'),
             (11, 'blue'),
             (12, 'blue'),
             (7, 'black'),
             (8, 'black'),
             (9, 'black'),
             (10, 'black')
            ]
``` 

## Testing the bot

You can test the bot on test conversations by running  `rasa test`.
This will run [end-to-end testing](https://rasa.com/docs/rasa/user-guide/testing-your-assistant/#end-to-end-testing) on the conversations in `tests/test_stories.yml`.

Note that if duckling isn't running when you do this, you'll see some failures.

## Rasa X Deployment

To [deploy this bot](https://rasa.com/docs/rasa/user-guide/how-to-deploy/), it is highly recommended to make use of the
[one line deploy script](https://rasa.com/docs/rasa-x/installation-and-setup/one-line-deploy-script/) for Rasa X. As part of the deployment, you'll need to set up [git integration](https://rasa.com/docs/rasa-x/installation-and-setup/integrated-version-control/#connect-your-rasa-x-server-to-a-git-repository) to pull in your data and
configurations, and build or pull an action server image.

## Action Server Image

You will need to have docker installed in order to build the action server image. If you haven't made any changes to the action code, you can also use
the [public image on Dockerhub](https://hub.docker.com/repository/docker/cdesmar/retail-demo) instead of building it yourself.

It is recommended to use an [automated CI/CD process](https://rasa.com/docs/rasa/user-guide/setting-up-ci-cd) to keep your action server up to date in a production environment.


## :gift: License
Licensed under the GNU General Public License v3. Copyright 2021 Rasa Technologies
GmbH. [Copy of the license](https://github.com/RasaHQ/retail-demo/blob/main/LICENSE).
Licensees may convey the work under this license. There is no warranty for the work.
