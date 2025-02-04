# J.A.R.V.I.S.

<div align="center">
  <img src="./images/jarvis.jpg">
</div>

**J.A.R.V.I.S.** significa "Just A Rather Very Intelligent System", que em português pode ser traduzido para "Apenas Um Sistema Bastante Inteligente"

Você pode criar uma aplicação simples em Python que permite interagir com o ChatGPT por meio de comandos de voz! Para isso, você pode usar algumas bibliotecas populares para captura de áudio e reconhecimento de fala, junto com a API do OpenAI para enviar os textos transcritos e obter respostas.

Aqui está um guia básico para configurar esse sistema:

### Requisitos:
1. **Python** (Certifique-se de ter o Python instalado).
2. **Bibliotecas Python**:
   - `openai`: Para interagir com o ChatGPT.
   - `speech_recognition`: Para capturar e transcrever o áudio da fala.
   - `pyttsx3`: Para sintetizar a fala (respostas do ChatGPT).
3. **API Key da OpenAI**: Você precisará de uma chave de API da OpenAI, que pode ser obtida ao se inscrever no [site da OpenAI](https://beta.openai.com/signup/).

### Passos:

1. **Instalar as bibliotecas necessárias**:

```bash
pip install openai speechrecognition pyttsx3 pyaudio
```

- **`speechrecognition`**: Para capturar e transcrever a fala.
- **`pyttsx3`**: Para converter texto em fala (respostas do ChatGPT).
- **`pyaudio`**: Para capturar o áudio via microfone.

2. **Código do Projeto**:

```python
import openai
import speech_recognition as sr
import pyttsx3

# Configuração da chave da API da OpenAI
openai.api_key = "SUA_API_KEY_AQUI"  # Substitua pela sua chave da API da OpenAI

# Função para capturar a fala e transcrever em texto
def ouvir_comando():
    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("Aguardando comando...")
        r.adjust_for_ambient_noise(source)  # Ajuste para ruído ambiente
        audio = r.listen(source)  # Escuta o áudio

    try:
        print("Reconhecendo...")
        comando = r.recognize_google(audio, language="pt-BR")  # Usa o Google para transcrição
        print(f"Você disse: {comando}")
        return comando
    except sr.UnknownValueError:
        print("Não entendi o que você disse.")
        return None
    except sr.RequestError:
        print("Erro ao tentar se conectar ao serviço de reconhecimento.")
        return None

# Função para obter resposta do ChatGPT
def obter_resposta_do_chatgpt(pergunta):
    try:
        response = openai.Completion.create(
            engine="text-davinci-003",  # Você pode usar o modelo mais recente disponível
            prompt=pergunta,
            max_tokens=150,
            temperature=0.7
        )
        resposta = response.choices[0].text.strip()
        return resposta
    except Exception as e:
        print(f"Erro ao obter resposta do ChatGPT: {e}")
        return "Desculpe, não consegui obter uma resposta."

# Função para falar a resposta
def falar_resposta(resposta):
    engine = pyttsx3.init()
    engine.setProperty("rate", 150)  # Define a velocidade da fala
    engine.say(resposta)
    engine.runAndWait()

# Função principal
def main():
    while True:
        comando = ouvir_comando()
        if comando:
            if "sair" in comando.lower():
                print("Saindo...")
                break

            resposta = obter_resposta_do_chatgpt(comando)
            print(f"Resposta do ChatGPT: {resposta}")
            falar_resposta(resposta)

if __name__ == "__main__":
    main()
```

### Explicação do Código:

1. **Captura de Áudio (Reconhecimento de Fala)**:
   - Usamos a biblioteca `speech_recognition` para capturar áudio do microfone e convertê-lo para texto. O método `listen` captura o som, e `recognize_google` usa o serviço de reconhecimento do Google para transcrever a fala em texto.

2. **Interação com o ChatGPT**:
   - Usamos a API do **OpenAI** para enviar a entrada transcrita para o ChatGPT e obter uma resposta. A resposta é retornada e processada.

3. **Fala de Resposta**:
   - Usamos o `pyttsx3`, que é uma biblioteca de conversão de texto em fala, para que a resposta do ChatGPT seja falada de volta para o usuário.

4. **Fluxo Principal**:
   - O programa entra em um loop onde fica ouvindo comandos de voz. Quando o usuário falar, o áudio será transcrito, enviado para o ChatGPT e a resposta será falada de volta.

### Personalizações e Melhorias:
- **Idioma**: A transcrição de fala está configurada para o português brasileiro (`language="pt-BR"`) na função `recognize_google`. Caso queira usar outro idioma, altere esse parâmetro.
- **Controle de Saída**: O loop pode ser controlado para parar quando o usuário disser algo como "sair". Isso é detectado na linha `if "sair" in comando.lower():`.
- **Customizações do ChatGPT**: A chave `temperature` no método `Completion.create` controla a criatividade da resposta. Para respostas mais objetivas, você pode diminuir esse valor.

### Executando a aplicação:
1. Substitua `"SUA_API_KEY_AQUI"` pela sua chave de API da OpenAI.
2. Execute o script. A aplicação começará a ouvir comandos de voz.
3. Fale algo e veja o ChatGPT responder em voz.

### Considerações Finais:
Esse é um exemplo básico para integrar a captura de voz e interação com o ChatGPT. Você pode aprimorar a interface, adicionar mais funcionalidades, melhorar a detecção de comandos, e até integrar com uma interface gráfica, dependendo do que precisar.

Se precisar de mais ajuda com algum detalhe ou aprimoramento, estou à disposição!
