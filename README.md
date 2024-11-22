# dio-tradutor

```markdown
# Document Translator

Este repositório contém um script Python para traduzir documentos do formato `.docx` utilizando a API de Tradução do Azure.

## Requisitos

- Python 3.x
- Bibliotecas Python:
  - `requests`
  - `python-docx`

## Instalação

Para instalar as bibliotecas necessárias, execute:

```bash
pip install requests python-docx
```

## Configuração

Antes de executar o script, você precisa configurar suas credenciais da API de Tradução do Azure:

1. Substitua `YOUR_SUBSCRIPTION_KEY` pelo seu chave de assinatura.
2. Substitua `YOUR_ENDPOINT` pelo seu endpoint da API.
3. Substitua `YOUR_LOCATION` pela sua localização.

## Uso

O script principal está dividido em duas funções:

1. **translate_document(document_text, target_language)**: Esta função traduz o texto de um documento para o idioma alvo especificado.
2. **translate_document(path)**: Esta função lê um documento `.docx`, traduz cada parágrafo e salva o documento traduzido.

### Exemplo de Uso

```python
import requests
from docx import Document
import os

subscription_key = "YOUR_SUBSCRIPTION_KEY"
endpoint = "YOUR_ENDPOINT"
location = "YOUR_LOCATION"
target_language = 'pt-br'

def translate_document(document_text, target_language):
    path = '/translate'
    constructed_url = endpoint + path

    headers = {
        'Ocp-Apim-Subscription-Key': subscription_key,
        'Ocp-Apim-Subscription-Region': location,
        'Content-type': 'application/json',
        'X-ClientTraceId': str(os.urandom(16))
    }

    body = [{
        'text': document_text
    }]
    params = {
        'api-version': '3.0',
        'from': 'en',
        'to': target_language
    }

    request_result = requests.post(constructed_url, params=params, headers=headers, json=body)
    response = request_result.json()
    return response[0]['translations'][0]['text']

def translate_document(path):
    document = Document(path)
    full_text = []
    for paragraph in document.paragraphs:
        translate_text = translate_document(paragraph.text, target_language)
        full_text.append(translate_text)

    translated_doc = Document()
    for line in full_text:
        translated_doc.add_paragraph(line)
    translated_doc.save('translated_doc.docx')

    path_translated = path.replace('.docx', '_translated.docx')
    translated_doc.save(path_translated)
    return path_translated

input_file = "/content/Azure Translation Services.docx"
translate_document(input_file)
```

## Licença

Este projeto está licenciado sob os termos da licença MIT. Veja o arquivo LICENSE para mais detalhes.
```
