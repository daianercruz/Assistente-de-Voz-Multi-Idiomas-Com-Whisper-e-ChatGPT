# 🎙️ Assistente de Voz com Python no Google Colab

Um assistente de voz completo rodando no Google Colab que combina gravação de áudio, reconhecimento de fala, integração com ChatGPT e síntese de voz.

---

## 📋 Visão Geral

Este projeto implementa um pipeline de assistente de voz de ponta a ponta com 4 etapas principais:

```
🎤 Gravar voz  →  🧠 Transcrever  →  💬 ChatGPT  →  🔊 Sintetizar resposta
```

| Etapa | Tecnologia | Função |
|-------|-----------|--------|
| 1 | JavaScript (MediaStream API) + Python | Gravação de áudio no navegador |
| 2 | OpenAI Whisper | Transcrição de fala para texto |
| 3 | OpenAI ChatGPT (gpt-4o-mini) | Processamento e geração de resposta |
| 4 | gTTS (Google Text-to-Speech) | Síntese de voz da resposta |

---

## 🚀 Como Usar

### Pré-requisitos

- Conta no [Google Colab](https://colab.research.google.com/)
- Chave de API da [OpenAI](https://platform.openai.com/api-keys)
- Microfone disponível no navegador

### Passo a passo

1. Abra o notebook no Google Colab
2. Configure sua chave de API da OpenAI na variável `OPENAI_API_KEY`
3. Defina o idioma desejado na variável `language` (ex: `'pt'` para português)
4. Execute todas as células em ordem
5. Quando solicitado, permita o acesso ao microfone no navegador
6. Fale sua pergunta e aguarde a resposta em áudio!

---

## 📦 Dependências

As bibliotecas são instaladas automaticamente ao executar o notebook:

```bash
pip install git+https://github.com/openai/whisper.git
pip install openai
pip install gTTS
```

---

## ⚙️ Configuração

### Idioma
```python
language = 'pt'  # Português
# Outros exemplos: 'en' (inglês), 'es' (espanhol), 'fr' (francês)
```

### Modelo do Whisper
```python
model = whisper.load_model("small")
```

Modelos disponíveis (do mais leve ao mais preciso):

| Modelo | Tamanho | Velocidade | Precisão |
|--------|---------|-----------|---------|
| `tiny` | ~39 MB | Muito rápido | Baixa |
| `base` | ~74 MB | Rápido | Média |
| `small` | ~244 MB | Moderado | Boa ✅ |
| `medium` | ~769 MB | Lento | Muito boa |
| `large` | ~1.5 GB | Muito lento | Máxima |

### Tempo de gravação
```python
record_file = record(sec=5)  # Padrão: 5 segundos
```

---

## 🏗️ Arquitetura

```
┌─────────────────────────────────────────────────────────┐
│                     Google Colab                        │
│                                                         │
│  ┌──────────┐    ┌──────────┐    ┌──────────────────┐  │
│  │JavaScript│───▶│  Python  │───▶│  Whisper (local) │  │
│  │MediaStream│   │b64decode │    │  Transcrição STT │  │
│  └──────────┘    └──────────┘    └────────┬─────────┘  │
│                                           │             │
│  ┌──────────┐    ┌──────────┐             │             │
│  │   gTTS   │◀───│ ChatGPT  │◀────────────┘             │
│  │ TTS voz  │    │  API     │                           │
│  └────┬─────┘    └──────────┘                           │
│       │                                                 │
│       ▼                                                 │
│  🔊 Reprodução de Áudio                                 │
└─────────────────────────────────────────────────────────┘
```

---

## 🔒 Segurança

> ⚠️ **Atenção:** Nunca exponha sua chave de API publicamente!

Recomenda-se usar o gerenciador de segredos do Google Colab em vez de inserir a chave diretamente no código:

```python
from google.colab import userdata
os.environ['OPENAI_API_KEY'] = userdata.get('OPENAI_API_KEY')
```

---

## 📁 Arquivos Gerados

| Arquivo | Descrição |
|---------|-----------|
| `/content/request_audio.wav` | Áudio gravado pelo usuário |
| `/content/response_audio.wav` | Resposta sintetizada em voz |

---

## 🐛 Problemas Conhecidos

- **Permissão de microfone negada:** Certifique-se de permitir o acesso ao microfone quando o navegador solicitar.
- **Erro de API:** Verifique se a chave da OpenAI está correta e possui créditos disponíveis.
- **Qualidade de transcrição ruim:** Tente usar um modelo Whisper maior (ex: `medium`) ou grave em um ambiente com menos ruído.
- **Whisper lento:** No plano gratuito do Colab, modelos maiores podem demorar. Use `small` ou `base` para melhor performance.

---

## Referências

- [Gravação de áudio no Colab](https://gist.github.com/korakot/c21c3476c024ad6d56d5f48b0bca92be)
- [OpenAI Whisper](https://github.com/openai/whisper)
- [OpenAI API](https://platform.openai.com/docs)
- [gTTS Documentation](https://gtts.readthedocs.io/)
- [Artigo DIO](https://web.dio.me/articles/conversando-por-voz-com-o-chatgpt-utilizando-whisper-openai-e-python)
