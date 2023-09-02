
# OpenIA Libreria de Python

La biblioteca OpenAI Python proporciona un acceso conveniente a la API OpenAI desde aplicaciones escritas en el lenguaje Python. Incluye un conjunto predefinido de clases para recursos API que se inicializan dinámicamente a partir de respuestas API, lo que lo hace compatible con una amplia gama de versiones de la API OpenAI.
## Documentation

Se pueden encontrar mas ejemplos de OpenAI respecto en la libreria de python en [API reference](https://platform.openai.com/docs/api-reference?lang=python) y en el [OpenAI Cookbook.](https://github.com/openai/openai-cookbook/)


## Instalacion

No necesita este código fuente a menos que desee modificar el paquete. Si sólo desea utilizar el paquete, simplemente ejecute:

```bash
pip install --upgrade openai
```
Instalar desde la fuente con:

```bash
python setup.py install
```
## Dependencias Opcionales

Instale dependencias para openai.embeddings_utils:

```bash
pip install openai[embeddings]
```
Las bibliotecas de datos como numpy y pandas no se instalan de forma predeterminada debido a su tamaño. Son necesarios para algunas funciones de esta biblioteca, pero generalmente no para comunicarse con la API. Si encuentra un MissingDependencyError, hay que instalarlo con:

```bash
pip install openai[datalib]
```

## Despliegue

La biblioteca debe configurarse con la API KEY de su cuenta que está disponible en el sitio web. Configúrelo con la variable de entorno OPENAI_API_KEY antes de usar la biblioteca:

```python
export OPENAI_API_KEY='sk-...'
```
De igual forma, tener en cuenta el uso de los roles de "user" "content" para el mismo, revisar: [chat.py](https://github.com/ElecSYC/chatgpt_apy/blob/main/chat.py)

A continuacion, un ejemplo anexo de despliegue e implementacion

```python
import openai
openai.api_key = "sk-..."

# Lista de modelos
models = openai.Model.list()

# Imprime el primer ID del modelo
print(models.data[0].id)

# Crear bajo el parametro chat_completion
chat_completion = openai.ChatCompletion.create(model="gpt-3.5-turbo",
messages=[{"role": "user", "content": "Hello world"}])

# Imprime el chat_completion
print(chat_completion.choices[0].message.content)
```


## Interfaz de la linea de comandos

Esta biblioteca proporciona además una utilidad de línea de comandos openai que facilita la interacción con la API desde su terminal.

```python
# lista de modelos
openai api models.list

# crear un chat completion (gpt-3.5-turbo, gpt-4, etc.)
openai api chat_completions.create -m gpt-3.5-turbo -g user "Hello world"

# crear un completion (text-davinci-003, text-davinci-002, ada, babbage, curie, davinci, etc.)
openai api completions.create -m ada -p "Hello world"

# generar imagenes via DALL·E API
openai api image.create -p "two dogs playing chess, cartoon" -n 1

# usar openia a traves de un proxy
openai --proxy=http://proxy.com api models.list
```

## Chat completions

Se pueden llamar modelos conversacionales como gpt-3.5-turbo utilizando el punto final de finalización del chat.

```python
import openai
openai.api_key = "sk-..." 
# inserta tu API KEY de las opciones disponibles para ti

completion = openai.ChatCompletion.create(model="gpt-3.5-turbo", 
messages=[{"role": "user", "content": "Hello world"}])
print(completion.choices[0].message.content)
```
## Completions

Los modelos de texto como babbage-002 o davinci-002 (y nuestros legacy completions models) se pueden llamar utilizando el punto final de terminaciones.

```python
import openai
openai.api_key = "sk-..."  # supply your API key however you choose

completion = openai.Completion.create(model="davinci-002", prompt="Hello world")
print(completion.choices[0].text)
```






## Autoria

- [@octokatherine](https://www.github.com/octokatherine)
- [@simonpfish](https://github.com/openai/openai-cookbook/)

## Requerimientos

- Python 3.7.1 +

En general, se puede encontrar soporte de Python que utilizan la mayoria de usuarios en diversos procesos, para documentacion adicional. Si tiene problemas con alguna versión, se puede verificar en la página de soporte. [Support Page](https://help.openai.com/en/)

## Creditos

Esta libreria fue referencia de [Stripe Python Library.](https://github.com/stripe/stripe-python)
