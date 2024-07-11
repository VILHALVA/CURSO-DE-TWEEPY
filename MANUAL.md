# MANUAL
## INSTALAÇÃO:
1. **Instalar Python**:
   Primeiro, você precisa ter o Python instalado na sua máquina. Baixe a versão mais recente do [site oficial do Python](https://www.python.org/downloads/) e siga as instruções de instalação.

2. **Instale o Tweepy:**
   - Você pode instalar o Tweepy usando pip:
     ```bash
     pip install tweepy
     ```

## CRIANDO UM PROJETO:
Para criar um bot no Twitter usando a biblioteca Tweepy em Python, você precisará seguir estas etapas:

1. **Crie uma conta de desenvolvedor no Twitter e crie um aplicativo:**
   - Vá até o [Twitter Developer Portal](https://developer.twitter.com/) e inscreva-se para uma conta de desenvolvedor.
   - Crie um novo projeto e, dentro deste projeto, crie um novo aplicativo. O Twitter fornecerá as chaves e tokens de acesso necessários (API key, API key secret, Access token, e Access token secret).

2. **Configure o seu ambiente:**
   - Armazene suas credenciais do Twitter em um arquivo `.env` (opcional, mas recomendado para segurança):
     ```plaintext
     API_KEY=your_api_key
     API_KEY_SECRET=your_api_key_secret
     ACCESS_TOKEN=your_access_token
     ACCESS_TOKEN_SECRET=your_access_token_secret
     ```

3. **Escreva o código do bot:**
   - Abaixo está um exemplo básico de como configurar o Tweepy e fazer um tweet usando as credenciais armazenadas em um arquivo `.env`:
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

## PASSOS ADICIONAIS:
- **Automatizar o bot:** Dependendo do que você quer que seu bot faça, você pode agendar tweets, responder a mentions, seguir novos seguidores, etc.
- **Usar bibliotecas de agendamento:** Para automatizar tarefas em intervalos regulares, você pode usar bibliotecas como `schedule` ou `APScheduler`.
- **Executar o bot continuamente:** Considere usar serviços de hospedagem como Heroku, AWS, ou mesmo um servidor próprio para manter seu bot funcionando continuamente.

## EXEMPLO DE BOT QUE RESPONDE A MENTIONS:
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