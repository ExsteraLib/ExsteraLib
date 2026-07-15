# ExsteraLib

ExsteraLib — официальная библиотека для разработки плагинов под **ExsteraGram**.

Она предоставляет единый API для работы практически со всеми компонентами клиента: интерфейсом, чатами, сообщениями, медиа, событиями, хуками, сетевыми запросами, AI и внутренними возможностями ExsteraGram.

---

# Установка

```python
from exsteralib import *
```

или импортировать только необходимые модули:

```python
from exsteralib import chat
from exsteralib import ui
from exsteralib import events
from exsteralib import hooks
```

---

# Структура библиотеки

```
exsteralib
│
├── ui
├── views
├── dialogs
├── themes
├── chat
├── messages
├── events
├── hooks
├── media
├── network
├── ai
├── storage
├── cache
├── navigation
├── stories
├── gifts
├── nft
```

---

# UI

Работа с интерфейсом ExsteraGram.

## Toast

### Обычный Toast

```python
ui.Toast.show("Hello")
```

### Успешное сообщение

```python
ui.Toast.show_success("Completed")
```

### Ошибка

```python
ui.Toast.show_error("Something went wrong")
```

### Длинный Toast

```python
ui.Toast.show(
    "Long message",
    duration=ui.Toast.LENGTH_LONG
)
```

---

# Dialogs

## AlertDialog

```python
dialogs.AlertDialog.builder() \
    .set_title("Delete") \
    .set_message("Delete message?") \
    .set_positive_button("Yes", lambda: print("Deleted")) \
    .set_negative_button("No", lambda: None) \
    .show()
```

---

## InputDialog

```python
dialogs.InputDialog.builder() \
    .set_title("Username") \
    .set_hint("Enter name") \
    .on_submit(lambda text: print(text)) \
    .show()
```

---

## BottomSheet

```python
dialogs.BottomSheet.builder() \
    .add_item("Option 1", callback1) \
    .add_item("Option 2", callback2) \
    .build() \
    .show()
```

---

# Views

## TextView

```python
text = views.TextView()

text.set_text("Hello")
text.set_text_size(16)
text.set_text_color("#ffffff")
```

---

## Button

```python
button = views.Button(text="Click")

button.set_on_click_listener(
    lambda: print("Clicked")
)
```

---

## LinearLayout

```python
layout = views.LinearLayout(
    orientation="vertical"
)

layout.add_view(
    views.TextView(text="Title")
)
```

---

## ScrollView

```python
scroll = views.ScrollView()

content = views.LinearLayout()

scroll.set_content(content)
```

---

# Themes

Получение информации о текущей теме.

```python
theme = themes.get_current_theme()
```

Получение цвета.

```python
themes.get_color("primary")
```

Изменение темы.

```python
themes.apply_theme("dark")
```

Регистрация собственной темы.

```python
theme = themes.Theme(name="Custom")

theme.set_color("primary", "#00FF00")

themes.register_theme(theme)
```

---

# Chat API

Работа с чатами.

## Отправить сообщение

```python
chat.send_message(
    chat_id=123,
    text="Hello"
)
```

---

## Ответить

```python
chat.send_message(
    chat_id=123,
    text="Reply",
    reply_to_msg_id=555
)
```

---

## Отложенная отправка

```python
chat.send_message(
    chat_id=123,
    text="Tomorrow",
    schedule_date=1672531200
)
```

---

## Удаление

```python
chat.delete_message(
    chat_id=123,
    message_id=456
)
```

---

## Массовое удаление

```python
chat.delete_messages(
    chat_id=123,
    message_ids=[1,2,3]
)
```

---

## Пересылка

```python
chat.forward_message(
    from_chat_id=1,
    to_chat_id=2,
    message_id=10
)
```

---

## Редактирование

```python
chat.edit_message(
    chat_id=123,
    message_id=456,
    new_text="Edited"
)
```

---

## Закрепление

```python
chat.pin_message(
    chat_id=123,
    message_id=456
)
```

---

## Получить историю

```python
history = chat.get_history(
    chat_id=123,
    limit=50
)
```

---

## Поиск сообщений

```python
messages = chat.search_messages(
    chat_id=123,
    query="hello"
)
```

---

## Получить участников

```python
members = chat.get_members(
    chat_id=123
)
```

---

## Заблокировать пользователя

```python
chat.ban_member(
    chat_id=123,
    user_id=555
)
```

---

# Events

Подписка на события приложения.

## Новое сообщение

```python
@events.on("message_received")
def handler(event):
    print(event.text)
```

---

## Пользователь печатает

```python
@events.on("typing_action")
def typing(event):
    pass
```

---

## Изменение статуса

```python
@events.on("user_status_changed")
def status(event):
    print(event.is_online)
```

---

## Собственное событие

```python
events.publish(
    "my_event",
    {
        "value":10
    }
)
```

Подписка:

```python
@events.on("my_event")
def handler(event):
    print(event)
```

---

# Hooks

Самый мощный модуль библиотеки.

Позволяет изменять любое поведение ExsteraGram.

## Before

```python
@hooks.before(
    "org.telegram.ui.ChatActivity",
    "methodName"
)
def before(this,args):
    pass
```

---

## After

```python
@hooks.after(
    "org.telegram.ui.ChatActivity",
    "methodName"
)
def after(this,result):
    return result
```

---

## Replace

```python
@hooks.replace(
    "org.telegram.SomeClass",
    "isPremium"
)
def replace(this,args):
    return True
```

---

## Hook Constructor

```python
@hooks.after_constructor(
    "org.telegram.ui.ProfileActivity"
)
def handler(this,args):
    pass
```

---

## Поле класса

```python
value = hooks.get_field(
    obj,
    "field"
)
```

Изменение

```python
hooks.set_field(
    obj,
    "field",
    value
)
```

---

## Вызов приватного метода

```python
hooks.call_method(
    obj,
    "methodName",
    []
)
```

---

## Удаление Hook

```python
hooks.unhook(handle)
```

---

## Удалить все Hook

```python
hooks.unhook_all()
```

---

# AI

Встроенные AI-инструменты.

## Перевод

```python
ai.Translator.translate(
    "Hello",
    target_lang="ru"
)
```

---

## Определение языка

```python
ai.Translator.detect(text)
```

---

## Исправление текста

```python
ai.Grammar.fix(text)
```

---

## Smart Reply

```python
ai.SmartReply.generate_replies(chat)
```

---

## Суммаризация

```python
ai.Summarizer.summarize(text)
```

---

## Speech To Text

```python
ai.SpeechToText.transcribe(
    "/sdcard/file.ogg"
)
```

---

## Text To Speech

```python
ai.TextToSpeech.synthesize(
    "Hello"
)
```

---

## Анализ тональности

```python
ai.SentimentAnalyzer.analyze(text)
```

---

## Vision

```python
ai.Vision.describe_image(
    "/sdcard/photo.jpg"
)
```

---

# Media

## Сжатие изображения

```python
media.ImageProcessor.compress(path)
```

---

## Изменение размера

```python
media.ImageProcessor.resize(
    path,
    width=800,
    height=800
)
```

---

## EXIF

```python
media.Exif.read(path)
```

Удалить:

```python
media.Exif.clear(path,new_path)
```

---

## Видео

Сжатие

```python
media.VideoProcessor.compress(path)
```

Извлечь звук

```python
media.VideoProcessor.extract_audio(path)
```

Создать GIF

```python
media.VideoProcessor.to_gif(path)
```

---

# Network

## GET

```python
network.http.get(url)
```

---

## POST

```python
network.http.post(
    url,
    json=data
)
```

---

## Скачать файл

```python
network.http.download(
    url,
    path
)
```

---

## Асинхронный запрос

```python
network.http.get_async(
    url,
    on_success,
    on_error
)
```

---

# Storage

## Сохранить

```python
storage.put_string(
    "key",
    "value"
)
```

---

## Получить

```python
storage.get_string(
    "key"
)
```

---

# Cache

```python
cache.set(
    "key",
    "value"
)
```

```python
cache.get("key")
```

---

# Navigation

Открыть чат.

```python
navigation.open_chat(chat_id)
```

Открыть настройки.

```python
navigation.open_settings()
```

---

# Stories

Создать историю.

```python
stories.post_story(
    media_path,
    caption
)
```

Получить истории пользователя.

```python
stories.get_stories(user_id)
```

---

# Gifts

Отправить подарок.

```python
gifts.send_gift(
    user_id,
    gift_id
)
```

---

# NFT

Передача NFT.

```python
nft.transfer(
    token_id,
    address,
    network
)
```

---

# Пример полноценного плагина

```python
from exsteralib import *

@events.on("message_received")
def on_message(event):

    if event.text == "/ping":

        chat.send_message(
            chat_id=event.chat_id,
            text="Pong!"
        )

        ui.Toast.show_success(
            "Command executed"
        )
```

---

# Полезные ссылки

- API Reference
- Examples
- Hooks Guide
- Events Guide
- UI Guide
- AI Guide
- Plugin Development Guide

---

# Лицензия

ExsteraLib предназначена исключительно для разработки плагинов ExsteraGram.
