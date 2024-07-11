# INSTRUÇÕES (GENERICAS)
## 01) INTRODUÇÃO E SETUP
### INTRODUÇÃO:
Tweepy é uma biblioteca Python que facilita a interação com a API do Twitter. Se você deseja criar bots do Twitter, automatizar tarefas, coletar dados ou simplesmente interagir com o Twitter a partir de seus scripts Python, Tweepy é uma ferramenta poderosa e de fácil utilização. Com Tweepy, você pode postar tweets, ler timelines, buscar tweets, seguir usuários, e muito mais, tudo isso utilizando uma interface de programação amigável.

### SETUP:
Para começar a usar Tweepy, você precisa seguir algumas etapas simples para configurar seu ambiente e autenticar seu aplicativo no Twitter.

1. **Crie uma conta de desenvolvedor no Twitter e configure um aplicativo:**

   - Acesse o [Twitter Developer Portal](https://developer.twitter.com/).
   - Inscreva-se para uma conta de desenvolvedor, se ainda não tiver uma.
   - Crie um novo projeto e, dentro deste projeto, crie um novo aplicativo.
   - Após a criação do aplicativo, você receberá as credenciais necessárias (API key, API key secret, Access token e Access token secret).

2. **Instale o Tweepy:**

   Use o pip para instalar a biblioteca Tweepy:
   ```bash
   pip install tweepy
   ```

3. **Configure o seu ambiente:**

   Armazene suas credenciais do Twitter em um arquivo `.env` (opcional, mas recomendado para segurança):
   ```plaintext
   API_KEY=your_api_key
   API_KEY_SECRET=your_api_key_secret
   ACCESS_TOKEN=your_access_token
   ACCESS_TOKEN_SECRET=your_access_token_secret
   ```

4. **Escreva o código para autenticação e postagem de tweets:**

   Aqui está um exemplo básico de como autenticar no Twitter e fazer um tweet usando as credenciais armazenadas no arquivo `.env`:

   ```python
   import os
   import tweepy
   from dotenv import load_dotenv

   # Carregar variáveis de ambiente do arquivo .env
   load_dotenv()

   # Obter as credenciais do arquivo .env
   API_KEY = os.getenv("API_KEY")
   API_KEY_SECRET = os.getenv("API_KEY_SECRET")
   ACCESS_TOKEN = os.getenv("ACCESS_TOKEN")
   ACCESS_TOKEN_SECRET = os.getenv("ACCESS_TOKEN_SECRET")

   # Autenticar no Twitter
   auth = tweepy.OAuth1UserHandler(API_KEY, API_KEY_SECRET, ACCESS_TOKEN, ACCESS_TOKEN_SECRET)
   api = tweepy.API(auth)

   # Verificar a autenticação
   try:
       api.verify_credentials()
       print("Autenticação bem-sucedida")
   except Exception as e:
       print("Erro na autenticação", e)

   # Fazer um tweet
   try:
       api.update_status("Olá, mundo! Este é meu primeiro tweet do bot.")
       print("Tweet enviado com sucesso!")
   except Exception as e:
       print("Erro ao enviar o tweet", e)
   ```

5. **Automatize o bot (opcional):**

   Dependendo do que você quer que seu bot faça, você pode agendar tweets, responder a mentions, seguir novos seguidores, etc. Para agendar tarefas, você pode usar bibliotecas como `schedule` ou `APScheduler`.

   Exemplo de bot que responde a mentions:
   
   ```python
   import os
   import tweepy
   from dotenv import load_dotenv

   # Carregar variáveis de ambiente do arquivo .env
   load_dotenv()

   # Obter as credenciais do arquivo .env
   API_KEY = os.getenv("API_KEY")
   API_KEY_SECRET = os.getenv("API_KEY_SECRET")
   ACCESS_TOKEN = os.getenv("ACCESS_TOKEN")
   ACCESS_TOKEN_SECRET = os.getenv("ACCESS_TOKEN_SECRET")

   # Autenticar no Twitter
   auth = tweepy.OAuth1UserHandler(API_KEY, API_KEY_SECRET, ACCESS_TOKEN, ACCESS_TOKEN_SECRET)
   api = tweepy.API(auth)

   # Verificar a autenticação
   try:
       api.verify_credentials()
       print("Autenticação bem-sucedida")
   except Exception as e:
       print("Erro na autenticação", e)

   # Responder a mentions
   class MentionListener(tweepy.StreamListener):
       def on_status(self, status):
           print(f"Tweet recebido de @{status.user.screen_name}: {status.text}")
           if '@SeuTwitterHandle' in status.text:
               try:
                   api.update_status(f"@{status.user.screen_name} Obrigado pela mention!", in_reply_to_status_id=status.id)
                   print("Resposta enviada com sucesso!")
               except Exception as e:
                   print("Erro ao enviar a resposta", e)

   listener = MentionListener()
   stream = tweepy.Stream(auth=api.auth, listener=listener)
   stream.filter(track=['@SeuTwitterHandle'])
   ```

   Substitua `@SeuTwitterHandle` pelo seu próprio handle do Twitter. Este exemplo configurará um bot que responde automaticamente a qualquer menção ao seu handle no Twitter.

## 02) ATUALIZANDO O PERFIL DO BOT
Para atualizar o perfil do seu bot no Twitter usando Tweepy, você pode modificar o código para incluir funcionalidades de atualização de perfil. Aqui está como você pode proceder:

1. **Atualizando o Código**:
   Primeiro, certifique-se de que seu ambiente está configurado corretamente com as credenciais do Twitter no arquivo `.env` e que o Tweepy está instalado e atualizado.

2. **Implementação**:
   Abaixo está um exemplo de como você pode atualizar o perfil do bot, incluindo a alteração de descrição, nome de exibição e imagem de perfil:

```python
import os
import tweepy
from dotenv import load_dotenv

# Carregar variáveis de ambiente do arquivo .env
load_dotenv()

# Obter as credenciais do arquivo .env
API_KEY = os.getenv("API_KEY")
API_KEY_SECRET = os.getenv("API_KEY_SECRET")
ACCESS_TOKEN = os.getenv("ACCESS_TOKEN")
ACCESS_TOKEN_SECRET = os.getenv("ACCESS_TOKEN_SECRET")

# Autenticar no Twitter
auth = tweepy.OAuthHandler(API_KEY, API_KEY_SECRET)
auth.set_access_token(ACCESS_TOKEN, ACCESS_TOKEN_SECRET)
api = tweepy.API(auth)

# Verificar a autenticação
try:
    api.verify_credentials()
    print("Autenticação bem-sucedida")
except Exception as e:
    print("Erro na autenticação", e)

# Atualizar perfil do bot
def update_profile(new_name, new_description, new_profile_image):
    try:
        # Atualizar nome de exibição
        api.update_profile(name=new_name)
        print(f"Nome de exibição atualizado para: {new_name}")

        # Atualizar descrição
        api.update_profile(description=new_description)
        print(f"Descrição atualizada para: {new_description}")

        # Atualizar imagem de perfil
        api.update_profile_image(filename=new_profile_image)
        print("Imagem de perfil atualizada")

    except Exception as e:
        print("Erro ao atualizar o perfil:", e)

# Exemplo de uso da função
if __name__ == "__main__":
    # Definir novos dados do perfil
    new_name = "Nome do Bot Atualizado"
    new_description = "Nova descrição do Bot no Twitter"
    new_profile_image = "caminho/para/sua/nova/imagem.jpg"  # Substitua pelo caminho da sua imagem

    # Chamada para atualizar o perfil
    update_profile(new_name, new_description, new_profile_image)
```

3. **Observações**:
   - Certifique-se de ter permissões adequadas para atualizar o perfil do bot no Twitter.
   - Substitua `caminho/para/sua/nova/imagem.jpg` pelo caminho correto da nova imagem que você deseja utilizar.
   - Teste cuidadosamente as alterações para garantir que tudo esteja funcionando conforme esperado.

Execute este código ajustado para atualizar o perfil do seu bot no Twitter com as novas informações e imagem de perfil.

## 03) POSTANDO TWEETS E ENVIANDO MÍDIA
Para postar tweets e enviar mídia (como imagens) usando o Tweepy no Twitter, você pode seguir este exemplo básico de código. Certifique-se de que suas credenciais do Twitter estão configuradas corretamente no arquivo `.env` e que você tem permissões adequadas para postar tweets e mídia na conta do bot no Twitter.

### Exemplo de Código:
```python
import os
import tweepy
from dotenv import load_dotenv

# Carregar variáveis de ambiente do arquivo .env
load_dotenv()

# Obter as credenciais do arquivo .env
API_KEY = os.getenv("API_KEY")
API_KEY_SECRET = os.getenv("API_KEY_SECRET")
ACCESS_TOKEN = os.getenv("ACCESS_TOKEN")
ACCESS_TOKEN_SECRET = os.getenv("ACCESS_TOKEN_SECRET")

# Autenticar no Twitter
auth = tweepy.OAuthHandler(API_KEY, API_KEY_SECRET)
auth.set_access_token(ACCESS_TOKEN, ACCESS_TOKEN_SECRET)
api = tweepy.API(auth)

# Verificar a autenticação
try:
    api.verify_credentials()
    print("Autenticação bem-sucedida")
except Exception as e:
    print("Erro na autenticação", e)

# Função para postar tweet com mídia
def post_tweet_with_media(tweet_text, media_path):
    try:
        # Postar tweet com mídia
        media = api.media_upload(media_path)
        api.update_status(status=tweet_text, media_ids=[media.media_id])
        print("Tweet enviado com sucesso!")
    except Exception as e:
        print("Erro ao enviar o tweet:", e)

# Exemplo de uso da função
if __name__ == "__main__":
    tweet_text = "Olá, mundo! Este é um tweet com mídia."
    media_path = "caminho/para/sua/imagem.jpg"  # Substitua pelo caminho da sua imagem

    # Chamada para postar tweet com mídia
    post_tweet_with_media(tweet_text, media_path)
```

### Explicação do Código:
1. **Autenticação**: Autentica no Twitter usando as credenciais fornecidas no arquivo `.env`.

2. **Verificação de Credenciais**: Verifica se a autenticação foi bem-sucedida.

3. **Função `post_tweet_with_media`**:
   - Esta função recebe o texto do tweet e o caminho da mídia (imagem, vídeo, etc.).
   - Usa `api.media_upload(media_path)` para carregar a mídia para o Twitter.
   - Usa `api.update_status()` para postar o tweet com a mídia carregada.

4. **Exemplo de Uso**:
   - Dentro do bloco `if __name__ == "__main__":`, você define o texto do tweet e o caminho da mídia que deseja postar.
   - Chama `post_tweet_with_media(tweet_text, media_path)` para realizar a postagem.

### Observações Importantes:
- Certifique-se de substituir `"caminho/para/sua/imagem.jpg"` pelo caminho correto da sua imagem ou mídia que deseja postar.
- O Tweepy suporta postagem de imagens e vídeos. Para outros tipos de mídia, consulte a documentação do Tweepy para métodos apropriados.
- Sempre teste cuidadosamente antes de implementar em produção para garantir que tudo esteja funcionando conforme esperado, especialmente ao lidar com mídias e autenticação.
- Verifique as políticas de uso do Twitter em relação a postagens de mídia e texto para garantir conformidade.

Esse código deve permitir que seu bot poste tweets com mídia de forma eficiente usando o Tweepy.

## 04) INTERAGINDO COM TWEETS
Para interagir com tweets usando o Tweepy no Twitter, você pode implementar funcionalidades como responder a tweets, retweetar, favoritar e seguir usuários. Abaixo, vou mostrar como você pode realizar cada uma dessas interações.

### 1. Responder a Tweets
Para responder a tweets de usuários específicos:

```python
import os
import tweepy
from dotenv import load_dotenv

# Carregar variáveis de ambiente do arquivo .env
load_dotenv()

# Obter as credenciais do arquivo .env
API_KEY = os.getenv("API_KEY")
API_KEY_SECRET = os.getenv("API_KEY_SECRET")
ACCESS_TOKEN = os.getenv("ACCESS_TOKEN")
ACCESS_TOKEN_SECRET = os.getenv("ACCESS_TOKEN_SECRET")

# Autenticar no Twitter
auth = tweepy.OAuthHandler(API_KEY, API_KEY_SECRET)
auth.set_access_token(ACCESS_TOKEN, ACCESS_TOKEN_SECRET)
api = tweepy.API(auth)

# Verificar a autenticação
try:
    api.verify_credentials()
    print("Autenticação bem-sucedida")
except Exception as e:
    print("Erro na autenticação", e)

# Função para responder a tweets
def reply_to_tweet(tweet_id, reply_text):
    try:
        api.update_status(
            status=reply_text,
            in_reply_to_status_id=tweet_id,
            auto_populate_reply_metadata=True
        )
        print(f"Resposta enviada para o tweet ID {tweet_id}")
    except tweepy.TweepError as e:
        print(f"Erro ao responder ao tweet ID {tweet_id}: {e}")

# Exemplo de uso da função
if __name__ == "__main__":
    tweet_id = "ID_DO_TWEET_AQUI"  # Substitua pelo ID do tweet ao qual você quer responder
    reply_text = "Olá! Esta é uma resposta automatizada."

    # Chamada para responder ao tweet
    reply_to_tweet(tweet_id, reply_text)
```

### 2. Retweetar um Tweet
Para retweetar um tweet:

```python
# Função para retweetar um tweet
def retweet_tweet(tweet_id):
    try:
        api.retweet(tweet_id)
        print(f"Retweet realizado para o tweet ID {tweet_id}")
    except tweepy.TweepError as e:
        print(f"Erro ao retweetar o tweet ID {tweet_id}: {e}")

# Exemplo de uso da função
if __name__ == "__main__":
    tweet_id = "ID_DO_TWEET_AQUI"  # Substitua pelo ID do tweet que você deseja retweetar

    # Chamada para retweetar o tweet
    retweet_tweet(tweet_id)
```

### 3. Favoritar um Tweet
Para favoritar (curtir) um tweet:

```python
# Função para favoritar um tweet
def like_tweet(tweet_id):
    try:
        api.create_favorite(tweet_id)
        print(f"Tweet ID {tweet_id} favoritado")
    except tweepy.TweepError as e:
        print(f"Erro ao favoritar o tweet ID {tweet_id}: {e}")

# Exemplo de uso da função
if __name__ == "__main__":
    tweet_id = "ID_DO_TWEET_AQUI"  # Substitua pelo ID do tweet que você deseja favoritar

    # Chamada para favoritar o tweet
    like_tweet(tweet_id)
```

### 4. Seguir um Usuário
Para seguir um usuário no Twitter:

```python
# Função para seguir um usuário
def follow_user(user_id):
    try:
        api.create_friendship(user_id)
        print(f"Você seguiu o usuário ID {user_id}")
    except tweepy.TweepError as e:
        print(f"Erro ao seguir o usuário ID {user_id}: {e}")

# Exemplo de uso da função
if __name__ == "__main__":
    user_id = "ID_DO_USUÁRIO_AQUI"  # Substitua pelo ID do usuário que você deseja seguir

    # Chamada para seguir o usuário
    follow_user(user_id)
```

### Observações Importantes:
- **IDs de Tweets e Usuários**: Os IDs de tweets e usuários podem ser obtidos a partir da URL do tweet ou do perfil do usuário.
- **Tratamento de Erros**: É importante implementar tratamento de erros para lidar com situações como tweets ou operações inexistentes, ou erros de autenticação.
- **Permissões**: Certifique-se de que sua conta de API do Twitter tenha permissões adequadas para realizar as ações desejadas (como postar, retweetar, favoritar, seguir).

Essas funções permitem que seu bot interaja de forma dinâmica com tweets no Twitter usando o Tweepy, adaptando-se às necessidades do seu projeto específico.

## 05) RASPANDO TWEETS DE USUÁRIOS
Para raspar tweets de usuários específicos usando Tweepy no Twitter, você pode seguir uma abordagem que envolve a utilização da API de busca avançada do Twitter. Vou mostrar como configurar isso no código Python usando Tweepy.

### Exemplo de Código para Raspar Tweets de Usuários:
```python
import os
import tweepy
from dotenv import load_dotenv

# Carregar variáveis de ambiente do arquivo .env
load_dotenv()

# Obter as credenciais do arquivo .env
API_KEY = os.getenv("API_KEY")
API_KEY_SECRET = os.getenv("API_KEY_SECRET")
ACCESS_TOKEN = os.getenv("ACCESS_TOKEN")
ACCESS_TOKEN_SECRET = os.getenv("ACCESS_TOKEN_SECRET")

# Autenticar no Twitter
auth = tweepy.OAuthHandler(API_KEY, API_KEY_SECRET)
auth.set_access_token(ACCESS_TOKEN, ACCESS_TOKEN_SECRET)
api = tweepy.API(auth, wait_on_rate_limit=True, wait_on_rate_limit_notify=True)

# Verificar a autenticação
try:
    api.verify_credentials()
    print("Autenticação bem-sucedida")
except Exception as e:
    print("Erro na autenticação", e)

# Função para raspar tweets de um usuário
def scrape_user_tweets(username, count=10):
    try:
        tweets = api.user_timeline(screen_name=username, count=count, tweet_mode="extended")
        for tweet in tweets:
            print(f"TWEET ID: {tweet.id}")
            print(f"Autor: @{tweet.user.screen_name}")
            print(f"Texto: {tweet.full_text}")
            print("-" * 50)
    except tweepy.TweepError as e:
        print(f"Erro ao raspar tweets de @{username}: {e}")

# Exemplo de uso da função
if __name__ == "__main__":
    username = "nome_do_usuário_aqui"  # Substitua pelo nome de usuário do Twitter que você deseja raspar
    count = 10  # Número de tweets a serem raspar (opcional, padrão é 10)

    # Chamada para raspar tweets do usuário
    scrape_user_tweets(username, count)
```

### Explicação do Código:
1. **Autenticação**: Autentica no Twitter usando as credenciais fornecidas no arquivo `.env`.

2. **Verificação de Credenciais**: Verifica se a autenticação foi bem-sucedida.

3. **Função `scrape_user_tweets`**:
   - Esta função recebe o nome de usuário do Twitter (`username`) e o número de tweets a serem raspar (`count`).
   - Usa `api.user_timeline()` para obter os tweets mais recentes do usuário especificado.
   - Itera sobre os tweets obtidos e imprime o ID do tweet, nome de usuário, texto completo do tweet, etc.

4. **Exemplo de Uso**:
   - Dentro do bloco `if __name__ == "__main__":`, você define o nome de usuário do Twitter que deseja raspar e opcionalmente o número de tweets.
   - Chama `scrape_user_tweets(username, count)` para realizar a raspagem de tweets do usuário especificado.

### Observações Importantes:
- **Modo de Tweet (`tweet_mode`)**: O parâmetro `tweet_mode="extended"` é usado para garantir que o texto completo dos tweets seja obtido, incluindo tweets com mais de 140 caracteres.
- **Tratamento de Erros**: O código inclui tratamento básico de erros para lidar com situações como falhas na obtenção de tweets devido a erros na API ou restrições de taxa.
- **Rate Limit**: A configuração `wait_on_rate_limit=True` permite que o Tweepy aguarde automaticamente quando atingir o limite de taxa da API do Twitter, reduzindo assim os erros de taxa excedida.

Esse código deve permitir que seu bot ou script Python raspe tweets de usuários específicos no Twitter usando a biblioteca Tweepy de forma eficaz. Certifique-se de ajustar conforme necessário para atender às suas necessidades específicas de raspagem de dados.

## 06) SCRAPING TWITTER SEARCH
Para realizar scraping de buscas no Twitter usando a API do Tweepy, podemos usar a funcionalidade de busca avançada oferecida pela API do Twitter. Isso permite buscar por tweets com base em palavras-chave, hashtags, usuários específicos, entre outros parâmetros.

Aqui está um exemplo de como você pode configurar isso no seu código Python usando Tweepy:

### Exemplo de Código para Scraping de Busca no Twitter:
```python
import os
import tweepy
from dotenv import load_dotenv

# Carregar variáveis de ambiente do arquivo .env
load_dotenv()

# Obter as credenciais do arquivo .env
API_KEY = os.getenv("API_KEY")
API_KEY_SECRET = os.getenv("API_KEY_SECRET")
ACCESS_TOKEN = os.getenv("ACCESS_TOKEN")
ACCESS_TOKEN_SECRET = os.getenv("ACCESS_TOKEN_SECRET")

# Autenticar no Twitter
auth = tweepy.OAuthHandler(API_KEY, API_KEY_SECRET)
auth.set_access_token(ACCESS_TOKEN, ACCESS_TOKEN_SECRET)
api = tweepy.API(auth, wait_on_rate_limit=True, wait_on_rate_limit_notify=True)

# Verificar a autenticação
try:
    api.verify_credentials()
    print("Autenticação bem-sucedida")
except Exception as e:
    print("Erro na autenticação", e)

# Função para realizar busca no Twitter
def search_tweets(query, count=10):
    try:
        tweets = tweepy.Cursor(api.search, q=query, lang="pt", tweet_mode="extended").items(count)
        for tweet in tweets:
            print(f"TWEET ID: {tweet.id}")
            print(f"Autor: @{tweet.user.screen_name}")
            print(f"Texto: {tweet.full_text}")
            print("-" * 50)
    except tweepy.TweepError as e:
        print(f"Erro ao buscar tweets: {e}")

# Exemplo de uso da função
if __name__ == "__main__":
    query = "Python OR #Programming"  # Substitua pela sua query de busca desejada
    count = 10  # Número de tweets a serem buscados (opcional, padrão é 10)

    # Chamada para realizar a busca
    search_tweets(query, count)
```

### Explicação do Código:
1. **Autenticação**: Autentica no Twitter usando as credenciais fornecidas no arquivo `.env`.

2. **Verificação de Credenciais**: Verifica se a autenticação foi bem-sucedida.

3. **Função `search_tweets`**:
   - Esta função recebe uma query de busca (`query`) e o número de tweets a serem buscados (`count`).
   - Usa `tweepy.Cursor(api.search, q=query, lang="pt", tweet_mode="extended").items(count)` para buscar tweets no Twitter.
     - `q=query`: Especifica a query de busca. Você pode usar operadores booleanos (AND, OR, NOT) e também pode buscar por hashtags, usuários, etc.
     - `lang="pt"`: Define o idioma dos tweets (neste caso, português).
     - `tweet_mode="extended"`: Garante que o texto completo dos tweets seja obtido.
     - `.items(count)`: Limita o número de tweets retornados pela busca.
   - Itera sobre os tweets obtidos e imprime o ID do tweet, nome de usuário, texto completo do tweet, etc.

4. **Exemplo de Uso**:
   - Dentro do bloco `if __name__ == "__main__":`, você define a query de busca desejada (`query`) e opcionalmente o número de tweets (`count`).
   - Chama `search_tweets(query, count)` para realizar a busca de tweets no Twitter.

### Observações Importantes:
- **Operadores de Busca**: Você pode usar operadores booleanos como `AND`, `OR`, `NOT` na sua query de busca para refinar os resultados.
- **Limite de Taxa**: O Tweepy gerencia automaticamente o limite de taxa da API do Twitter com `wait_on_rate_limit=True`, o que significa que ele vai esperar quando atingir o limite de taxa antes de continuar.
- **Tratamento de Erros**: O código inclui um tratamento básico de erros para lidar com situações como falhas na busca de tweets devido a erros na API ou restrições de taxa.

Este exemplo permite que seu bot ou script Python busque tweets no Twitter com base em critérios específicos, adaptando-se às suas necessidades de scraping de dados. Certifique-se de ajustar conforme necessário para atender às suas especificações de projeto.

## 07) RASPANDO TWEETS EM TEMPO REAL
Para raspar tweets em tempo real usando Tweepy no Twitter, você pode usar o recurso de streaming oferecido pela API do Twitter. Isso permite capturar tweets conforme são publicados em tempo real, baseados em filtros específicos, como palavras-chave, hashtags ou até mesmo tweets de usuários específicos.

Aqui está um exemplo de como configurar o streaming de tweets em tempo real usando Tweepy:

### Exemplo de Código para Raspagem de Tweets em Tempo Real:
```python
import os
import tweepy
from dotenv import load_dotenv

# Carregar variáveis de ambiente do arquivo .env
load_dotenv()

# Obter as credenciais do arquivo .env
API_KEY = os.getenv("API_KEY")
API_KEY_SECRET = os.getenv("API_KEY_SECRET")
ACCESS_TOKEN = os.getenv("ACCESS_TOKEN")
ACCESS_TOKEN_SECRET = os.getenv("ACCESS_TOKEN_SECRET")

# Autenticar no Twitter
auth = tweepy.OAuthHandler(API_KEY, API_KEY_SECRET)
auth.set_access_token(ACCESS_TOKEN, ACCESS_TOKEN_SECRET)
api = tweepy.API(auth, wait_on_rate_limit=True, wait_on_rate_limit_notify=True)

# Verificar a autenticação
try:
    api.verify_credentials()
    print("Autenticação bem-sucedida")
except Exception as e:
    print("Erro na autenticação", e)

# Criar um stream listener
class MyStreamListener(tweepy.StreamListener):
    def on_status(self, status):
        print(f"TWEET ID: {status.id}")
        print(f"Autor: @{status.user.screen_name}")
        print(f"Texto: {status.text}")
        print("-" * 50)

    def on_error(self, status_code):
        if status_code == 420:
            return False  # Retorna False no caso de atingir o limite de taxa (420)

# Iniciar o streaming
if __name__ == "__main__":
    my_stream_listener = MyStreamListener()
    my_stream = tweepy.Stream(auth=api.auth, listener=my_stream_listener)

    # Filtros de palavras-chave para busca em tempo real
    keywords = ["Python", "Data Science", "Machine Learning"]

    try:
        my_stream.filter(track=keywords, languages=["pt"])  # Filtrar tweets por palavras-chave e idioma
    except Exception as e:
        print("Erro durante o streaming:", e)
```

### Explicação do Código:
1. **Autenticação**: Autentica no Twitter usando as credenciais fornecidas no arquivo `.env`.

2. **Verificação de Credenciais**: Verifica se a autenticação foi bem-sucedida.

3. **Classe `MyStreamListener`**:
   - Esta classe herda de `tweepy.StreamListener` e sobrescreve os métodos `on_status` e `on_error`.
   - `on_status(self, status)`: É chamado quando um novo tweet que corresponde aos filtros é recebido. Imprime o ID do tweet, nome de usuário e texto completo do tweet.
   - `on_error(self, status_code)`: Trata erros durante o streaming, como atingir o limite de taxa (status code 420).

4. **Início do Streaming**:
   - Dentro do bloco `if __name__ == "__main__":`, cria uma instância de `MyStreamListener` e `tweepy.Stream`.
   - Define filtros usando `my_stream.filter(track=keywords, languages=["pt"])`, onde `keywords` são as palavras-chave a serem filtradas e `languages` define o idioma dos tweets (neste caso, português).

### Observações Importantes:
- **Streaming em Tempo Real**: Este código permite que seu script capture tweets em tempo real conforme são publicados no Twitter, com base nos filtros definidos.
- **Tratamento de Erros**: Inclui um tratamento básico de erros para lidar com problemas durante o streaming, como atingir o limite de taxa da API.
- **Limitações**: O Twitter impõe limitações no número de tweets que podem ser capturados em streaming, e o Tweepy lida automaticamente com os detalhes de conexão e limites de taxa.

Certifique-se de ajustar as palavras-chave (`keywords`) e outros parâmetros conforme suas necessidades específicas de scraping de dados em tempo real do Twitter.

## 08) COLETANDO SEGUIDORES E SEGUIDORES DE USUÁRIOS (AMIGOS)
Para coletar seguidores e amigos (usuários que uma conta segue) usando Tweepy no Twitter, você pode usar os métodos disponíveis na API do Tweepy para obter essas informações. Aqui está como você pode fazer isso:

### Coletando Seguidores de um Usuário:
```python
import os
import tweepy
from dotenv import load_dotenv

# Carregar variáveis de ambiente do arquivo .env
load_dotenv()

# Obter as credenciais do arquivo .env
API_KEY = os.getenv("API_KEY")
API_KEY_SECRET = os.getenv("API_KEY_SECRET")
ACCESS_TOKEN = os.getenv("ACCESS_TOKEN")
ACCESS_TOKEN_SECRET = os.getenv("ACCESS_TOKEN_SECRET")

# Autenticar no Twitter
auth = tweepy.OAuthHandler(API_KEY, API_KEY_SECRET)
auth.set_access_token(ACCESS_TOKEN, ACCESS_TOKEN_SECRET)
api = tweepy.API(auth, wait_on_rate_limit=True, wait_on_rate_limit_notify=True)

# Verificar a autenticação
try:
    api.verify_credentials()
    print("Autenticação bem-sucedida")
except Exception as e:
    print("Erro na autenticação", e)

# ID do usuário do qual você deseja obter os seguidores
user_id = "usuario_alvo"

# Obter lista de seguidores
followers = []
for follower in tweepy.Cursor(api.followers_ids, user_id=user_id).items():
    followers.append(follower)

print(f"Seguidores de {user_id}: {followers}")
```

### Coletando Amigos (Usuários Seguidos) de um Usuário:
```python
# ID do usuário do qual você deseja obter os amigos (usuários seguidos)
user_id = "usuario_alvo"

# Obter lista de amigos (usuários seguidos)
friends = []
for friend in tweepy.Cursor(api.friends_ids, user_id=user_id).items():
    friends.append(friend)

print(f"Amigos (usuários seguidos) de {user_id}: {friends}")
```

### Explicação do Código:
1. **Autenticação**: Autentica no Twitter usando as credenciais fornecidas no arquivo `.env`.

2. **Verificação de Credenciais**: Verifica se a autenticação foi bem-sucedida.

3. **Obtenção de Seguidores**:
   - Utiliza `tweepy.Cursor(api.followers_ids, user_id=user_id).items()` para iterar sobre todos os IDs dos seguidores do usuário especificado (`user_id`).
   - Adiciona cada ID de seguidor à lista `followers`.

4. **Obtenção de Amigos**:
   - Utiliza `tweepy.Cursor(api.friends_ids, user_id=user_id).items()` para iterar sobre todos os IDs dos amigos (usuários seguidos) do usuário especificado (`user_id`).
   - Adiciona cada ID de amigo à lista `friends`.

5. **Impressão dos Resultados**:
   - Imprime as listas `followers` e `friends`, que contêm os IDs dos seguidores e amigos, respectivamente.

### Observações Importantes:
- **Limitações**: O Twitter impõe limitações no número de solicitações que podem ser feitas para obter seguidores e amigos. O Tweepy lida automaticamente com os detalhes de conexão e limites de taxa.
- **Dados dos Usuários**: Os IDs coletados são IDs numéricos dos usuários. Para obter mais informações sobre cada usuário (como nome de usuário, nome real, etc.), você precisará fazer solicitações adicionais à API do Twitter.

Certifique-se de substituir `"usuario_alvo"` pelo nome de usuário ou ID do usuário real do qual você deseja obter os seguidores e amigos.

## 09) SEGUINDO USUÁRIOS E EXTRAINDO DADOS DO USUÁRIO
Para seguir usuários e extrair dados do perfil usando Tweepy no Twitter, você pode utilizar os métodos disponíveis na API do Tweepy para realizar essas operações. Aqui está como você pode fazer isso:

### Seguir Usuários:
Para seguir usuários, você pode usar o método `api.create_friendship()` do Tweepy. Este método permite seguir um usuário específico.

```python
import os
import tweepy
from dotenv import load_dotenv

# Carregar variáveis de ambiente do arquivo .env
load_dotenv()

# Obter as credenciais do arquivo .env
API_KEY = os.getenv("API_KEY")
API_KEY_SECRET = os.getenv("API_KEY_SECRET")
ACCESS_TOKEN = os.getenv("ACCESS_TOKEN")
ACCESS_TOKEN_SECRET = os.getenv("ACCESS_TOKEN_SECRET")

# Autenticar no Twitter
auth = tweepy.OAuthHandler(API_KEY, API_KEY_SECRET)
auth.set_access_token(ACCESS_TOKEN, ACCESS_TOKEN_SECRET)
api = tweepy.API(auth, wait_on_rate_limit=True, wait_on_rate_limit_notify=True)

# Verificar a autenticação
try:
    api.verify_credentials()
    print("Autenticação bem-sucedida")
except Exception as e:
    print("Erro na autenticação", e)

# Nome de usuário do usuário que você deseja seguir
screen_name = "username_alvo"

# Seguir o usuário
try:
    api.create_friendship(screen_name)
    print(f"Você começou a seguir @{screen_name}")
except tweepy.TweepError as e:
    print(f"Erro ao seguir @{screen_name}: {e}")
```

### Extrair Dados do Perfil de um Usuário:
Para extrair dados do perfil de um usuário, você pode usar o método `api.get_user()` do Tweepy. Este método retorna um objeto `User` que contém informações detalhadas sobre o usuário.

```python
# Nome de usuário do usuário do qual você deseja extrair os dados do perfil
screen_name = "username_alvo"

# Obter dados do perfil do usuário
try:
    user = api.get_user(screen_name)
    print(f"Dados do perfil de @{screen_name}:")
    print(f"Nome: {user.name}")
    print(f"Descrição: {user.description}")
    print(f"Localização: {user.location}")
    print(f"Seguidores: {user.followers_count}")
    print(f"Amigos (seguindo): {user.friends_count}")
    print(f"Tweets: {user.statuses_count}")
except tweepy.TweepError as e:
    print(f"Erro ao obter dados do perfil de @{screen_name}: {e}")
```

### Explicação do Código:
1. **Autenticação**: Autentica no Twitter usando as credenciais fornecidas no arquivo `.env`.

2. **Verificação de Credenciais**: Verifica se a autenticação foi bem-sucedida.

3. **Seguir Usuário**:
   - Utiliza `api.create_friendship(screen_name)` para começar a seguir o usuário com o nome de usuário especificado (`screen_name`).

4. **Extrair Dados do Perfil**:
   - Utiliza `api.get_user(screen_name)` para obter um objeto `User` que contém informações detalhadas sobre o perfil do usuário com o nome de usuário especificado (`screen_name`).
   - Imprime informações como nome, descrição, localização, número de seguidores, número de amigos (seguindo) e número de tweets.

5. **Tratamento de Erros**: Inclui tratamento de erros para lidar com situações onde o usuário não pode ser seguido ou onde os dados do perfil não podem ser recuperados.

Certifique-se de substituir `"username_alvo"` pelo nome de usuário real do usuário que você deseja seguir e do qual deseja extrair dados do perfil.

## 10) ENVIANDO MENSAGENS DIRETAS
Para enviar mensagens diretas usando o Tweepy no Twitter, você pode usar o método `api.send_direct_message()`. Este método permite enviar uma mensagem direta para outro usuário no Twitter. Aqui está como você pode implementar isso:

```python
import os
import tweepy
from dotenv import load_dotenv

# Carregar variáveis de ambiente do arquivo .env
load_dotenv()

# Obter as credenciais do arquivo .env
API_KEY = os.getenv("API_KEY")
API_KEY_SECRET = os.getenv("API_KEY_SECRET")
ACCESS_TOKEN = os.getenv("ACCESS_TOKEN")
ACCESS_TOKEN_SECRET = os.getenv("ACCESS_TOKEN_SECRET")

# Autenticar no Twitter
auth = tweepy.OAuthHandler(API_KEY, API_KEY_SECRET)
auth.set_access_token(ACCESS_TOKEN, ACCESS_TOKEN_SECRET)
api = tweepy.API(auth, wait_on_rate_limit=True, wait_on_rate_limit_notify=True)

# Verificar a autenticação
try:
    api.verify_credentials()
    print("Autenticação bem-sucedida")
except Exception as e:
    print("Erro na autenticação", e)

# Nome de usuário do destinatário da mensagem direta
recipient_screen_name = "username_destinatario"

# Corpo da mensagem direta
message_text = "Olá! Esta é uma mensagem direta enviada pelo meu bot."

# Enviar a mensagem direta
try:
    api.send_direct_message(recipient_screen_name, text=message_text)
    print(f"Mensagem direta enviada para @{recipient_screen_name}: {message_text}")
except tweepy.TweepError as e:
    print(f"Erro ao enviar mensagem direta para @{recipient_screen_name}: {e}")
```

### Explicação do Código:
1. **Autenticação**: Autentica no Twitter usando as credenciais fornecidas no arquivo `.env`.

2. **Verificação de Credenciais**: Verifica se a autenticação foi bem-sucedida.

3. **Envio de Mensagem Direta**:
   - Utiliza `api.send_direct_message(recipient_screen_name, text=message_text)` para enviar uma mensagem direta para o usuário com o nome de usuário especificado (`recipient_screen_name`).
   - `message_text` é o corpo da mensagem direta que você deseja enviar.

4. **Tratamento de Erros**: Inclui tratamento de erros para lidar com situações onde a mensagem direta não pode ser enviada, por exemplo, devido a restrições do Twitter ou problemas na rede.

Certifique-se de substituir `"username_destinatario"` pelo nome de usuário real do destinatário da mensagem direta e ajustar o texto da mensagem conforme necessário para suas necessidades.

## 11) RASPANDO TENDÊNCIAS DO TWITTER
Para raspar tendências do Twitter usando Tweepy, você pode usar o método `api.trends_place()` fornecido pela API do Twitter no Tweepy. Este método permite obter as tendências atuais para um determinado lugar específico. Aqui está como você pode implementar isso:

```python
import os
import tweepy
from dotenv import load_dotenv

# Carregar variáveis de ambiente do arquivo .env
load_dotenv()

# Obter as credenciais do arquivo .env
API_KEY = os.getenv("API_KEY")
API_KEY_SECRET = os.getenv("API_KEY_SECRET")
ACCESS_TOKEN = os.getenv("ACCESS_TOKEN")
ACCESS_TOKEN_SECRET = os.getenv("ACCESS_TOKEN_SECRET")

# Autenticar no Twitter
auth = tweepy.OAuthHandler(API_KEY, API_KEY_SECRET)
auth.set_access_token(ACCESS_TOKEN, ACCESS_TOKEN_SECRET)
api = tweepy.API(auth, wait_on_rate_limit=True, wait_on_rate_limit_notify=True)

# Verificar a autenticação
try:
    api.verify_credentials()
    print("Autenticação bem-sucedida")
except Exception as e:
    print("Erro na autenticação", e)

# ID do lugar (no caso, mundial)
WORLD_WOEID = 1

# Obter tendências mundiais
try:
    trends = api.trends_place(WORLD_WOEID)
    trends_list = trends[0]['trends']

    print("Tendências mundiais no Twitter:")
    for trend in trends_list:
        print(f"- {trend['name']}")

except tweepy.TweepError as e:
    print(f"Erro ao obter tendências: {e}")
```

### Explicação do Código:
1. **Autenticação**: Autentica no Twitter usando as credenciais fornecidas no arquivo `.env`.

2. **Verificação de Credenciais**: Verifica se a autenticação foi bem-sucedida.

3. **Obtenção de Tendências Mundiais**:
   - Define `WORLD_WOEID` como 1, que é o WOEID (Where On Earth ID) para o mundo inteiro.
   - Usa `api.trends_place(WORLD_WOEID)` para obter as tendências atuais para o mundo inteiro.
   - Itera sobre a lista de tendências (`trends_list`) e imprime o nome de cada tendência.

4. **Tratamento de Erros**: Inclui tratamento de erros para lidar com situações onde as tendências não podem ser obtidas, por exemplo, devido a restrições do Twitter ou problemas na rede.

Certifique-se de ajustar o código conforme necessário para adaptá-lo às suas necessidades específicas, como escolher um lugar específico usando o WOEID apropriado, se você estiver interessado em tendências locais.