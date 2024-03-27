# It's quiz telegram bot with API Reverso_Context

You need to connect your reverso context accout to this bot and you'll get words from your favorite list.
Now this bot works only with Russian and English dictionary. 
You'll get words in Russian transation and you need to choose correct option in English. The English words will be as the same.

You can use command /help for choose correct quiz command. You need to pick command every time after each question.

List of commands:
```
/eng -- launch only English words
/rus -- launch only Russian words
/ran  -- launch random words (It might English or Russian translation). Before launch you need to create local dictonary of this words /cr_dict
/ran10 -- launch random 10 words (It might English or Russian translation). Before launch you need to create loacal list of this words /cr_ran10
/last20 -- launch random 10 words (It might English or Russian translation). Before launch you need to create loacal list of this words /cr_last20
/last50 -- launch random 50 words (It might English or Russian translation). Before launch you need to create loacal list of this words /cr_last50
```
The dictionary automaticaly updates every night at 12.

Below some information about Reverso API

## reverso_context_api
Simple Python API for [Reverso Context](https://context.reverso.net)

## Installation
```pip install reverso-context-api```

## Entry point
All operations are implemented in class `Client`.     
```python3
from reverso_context_api import Client
```

It takes source and target languages ("de"/"en"/"ru"/etc) as required arguments. They will be used as defaults, but you can set another pair for each operation. <br>
```python3
client = Client("de", "en")
```

Also you can pass login credentials to be able to work with your favorites:<br>
```python3
client = Client("en", "ru", credentials=("email", "password"))
```

## Supported features
* Getting translations:<br>
```python3
>>> list(client.get_translations("braucht"))
['needed', 'required', 'need', 'takes', 'requires', 'take', 'necessary'...]
```
* Getting translation samples (context):<br>
```python3
>>> client = Client("en", "ru")
>>> for context in client.get_translation_samples("shenanigans", cleanup=True):
...     print(context)
("The simple fact is the city is going broke cleaning up after Homer's drunken shenanigans...", 
 "Простой факт: город разорится, восстанавливаясь после пьяных проделок Гомера...")
("You need a 12-step program for shenanigans addicts.", 
 "Тебе нужно пройти 12-ти ступенчатую программу зависимости от проделок.")
```
* Getting search suggestions:<br>
```python3
>>> list(Client("de", "en").get_search_suggestions("bew")))
['Bewertung', 'Bewegung', 'bewegen', 'bewegt', 'bewusst', 'bewirkt', 'bewertet'...]
```
* Getting your saved favorites (you'll have to pass login credentials):
```python3
>>> client = Client("en", "de", credentials=("email", "password"))
>>> next(client.get_favorites())
{'source_lang': 'de', 'source_text': 'imstande', 'source_context': 'Ich würde lachen, doch ich bin biologisch nicht imstande dazu.', 
 'target_lang': 'ru', 'target_text': 'способен', 'target_context': 'Я бы посмеялся, но мой вид на это не способен.'}
```

All the methods return iterators (to hide paging and to not request more results than needed)
