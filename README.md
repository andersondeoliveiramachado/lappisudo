# Bot do LAPPIS

## LAPPISUDO

Este projeto teve como base a [Tais](http://github.com/lappis-unb/tais). Com o objetivo de ser divertido e apresentar o laboratório.

## Bot

### RocketChat

```sh
sudo docker-compose up -d rocketchat
# aguarde 3 minutos para o rocketchat terminar de levantar
sudo docker-compose up bot
```

Para que a assistente virtual inicie a conversa você deve criar um `trigger`.
Para isso, entre no rocketchat como `admin`, e vá no painel do Livechat na
seção de Triggers, clique em `New Trigger`. Preencha o Trigger da seguinte forma:

```yaml
Enabled: Yes
Name: Start Talk
Description: Start Talk
Condition: Visitor time on site
    Value: 3
Action: Send Message
 Value: Impersonate next agent from queue
 Value: Olá!
```

O valor `http://localhost:8080/` deve ser a URL de acesso do Bot.

* Muda a cor do `livechat` nas configurações da Rocket.Chat

`#0068b4`

#### Instalação

Para executar o bot em um site você precisa inserir o seguinte Javascript na sua página

```js
<!-- Start of Rocket.Chat Livechat Script -->
<script type="text/javascript">
(function(w, d, s, u) {

    // !!! Mudar para o seu host AQUI !!!
    host = 'http://localhost:3000';
    // !!! ^^^^^^^^^^^^^^^^^^^^^^^^^^ !!!

    w.RocketChat = function(c) { w.RocketChat._.push(c) }; w.RocketChat._ = []; w.RocketChat.url = u;
    var h = d.getElementsByTagName(s)[0], j = d.createElement(s);
    j.async = true; j.src = host + '/packages/rocketchat_livechat/assets/rocketchat-livechat.min.js?_=201702160944';
    h.parentNode.insertBefore(j, h);
})(window, document, 'script', host + '/livechat');
</script>
<!-- End of Rocket.Chat Livechat Script -->
```

**Atenção**: Você precisa alterar a variavel `host` dentro do código acima para a url do site onde estará
o seu Rocket.Chat.




### Telegram

* Atualize as variáveis de ambiente:

```sh
- TELEGRAM_BOT_USERNAME=lappisbot
- TELEGRAM_TOKEN=token
- TELEGRAM_WEBHOOK=webhook
```

* Caso ainda não tenha treinado o bot execute:

```sh
docker-compose run --rm bot make train
```

* Execute o script do bot no telegram:

```sh
docker-compose up bot-telegram
```

* A porta utilizada é a 5005


### Console

```sh
sudo docker-compose run --rm bot make train
sudo docker-compose run --rm bot make run-console
```

### Train Online

```
sudo docker-compose run --rm bot make train
sudo docker-compose run --rm bot make train-online
```




## Notebooks - Análise de dados

### Setup

Levante o container `notebooks`

```sh
docker-compose up -d notebooks
```

Acesse o notebook em `localhost:8888`




## Tutorial para levantar toda a stack

```sh
sudo docker-compose up -d rocketchat

sudo docker-compose up -d kibana
sudo docker-compose run --rm -v $PWD/analytics:/analytics bot python /analytics/setup_elastic.py

# aguarde 3 minutos para o rocketchat terminar de levantar
sudo docker-compose up -d bot
```
