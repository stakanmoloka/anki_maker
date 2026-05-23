# Anki .apkg maker

Одностраничный HTML-инструмент для генерации колод Anki Desktop в формате `.apkg`.

Проект не требует сборки или установки зависимостей: достаточно открыть `index.html` в браузере, вставить данные в нужную вкладку и скачать готовую колоду.

## Как запустить

1. Открой `index.html` в браузере.
2. Выбери нужную вкладку.
3. Вставь входной текст.
4. Укажи название колоды.
5. Нажми кнопку генерации `.apkg`.
6. Открой скачанный файл в Anki Desktop.

Важно: для генерации используются `sql.js` и `JSZip` с CDN, поэтому браузеру нужен доступ к интернету, если эти библиотеки еще не закешированы.

## Вкладки

### sub2anki

Создает карточки из списка фраз с примером предложения и переводом.

Пример формата:

```text
1. **Break the ice** [breik the ais] - Растопить лед.
    * Sentence: "A nice joke is a great way to break the ice when you meet someone new."
    * Translation: Хорошая шутка - отличный способ растопить лед, когда встречаешь кого-то нового.
```

На лицевой стороне будет предложение с пропуском, на обратной стороне - исходная фраза, значение, транскрипция и перевод.

### words2anki

Создает простые карточки из списка слов.

Пример формата:

```text
1. **salut** - hi, hello, bye - [saly]
2. **bonjour** - hello, good morning - [bonzhur]
3. **au revoir** - goodbye - [o revwar]
```

На лицевой стороне будет слово, на обратной стороне - перевод и, если есть, транскрипция.

### triples2anki

Создает по 3 карточки на каждое слово:

- `Cloze deletion`
- `English -> meaning in English`
- `Collocation card`

Пример формата:

```text
# WORD: agree

## 1. Cloze deletion
Front:
I don't think we'll ever [...] on which movie to watch tonight; we have such different tastes.

Back:
agree

## 2. English -> meaning in English
Front:
To have the same opinion or belief as another person.

Back:
agree

## 3. Collocation card
Front:
completely [...] with someone

Back:
agree

---

# WORD: alcohol

## 1. Cloze deletion
Front:
The legal age for buying [...] varies significantly from one country to another.

Back:
alcohol

## 2. English -> meaning in English
Front:
A type of drink, such as beer or wine, that can make people drunk.

Back:
alcohol

## 3. Collocation card
Front:
avoid drinking [...]

Back:
alcohol
```

Каждая секция `##` превращается в отдельную карточку Anki. Поле `Front:` становится лицевой стороной, поле `Back:` - обратной.

## Структура

```text
.
├── index.html
└── README.md
```

Вся логика находится в `index.html`: интерфейс, парсеры входных форматов и сборка `.apkg`.

## Проверка

После изменений можно быстро проверить JavaScript-синтаксис командой:

```bash
node -e "const fs=require('fs'); const html=fs.readFileSync('index.html','utf8'); const js=html.match(/<script>([\s\S]*)<\/script>/)[1]; new Function(js); console.log('js syntax ok');"
```

