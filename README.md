# Vampgram — flat PWA build

Все файлы проекта находятся в корне ZIP. Вложенных папок нет.

## Что изменено

- Полностью плоская структура файлов: `index.html`, Firebase-конфиги, правила, Service Worker, манифест и все иконки находятся рядом.
- Форма выбора фото профиля принимает изображения `image/*` без лимита 500 КБ для исходного файла.
- Большие фото профиля автоматически уменьшаются и переводятся в JPEG до безопасного размера для Firestore (не более 460 КБ).
- Фото, отправляемые в чат, автоматически уменьшаются до максимальной стороны 3200 px и сжимаются до 5.5 МБ; исходный размер выбранной фотографии не ограничен фиксированным клиентским лимитом.
- Обновлены иконки приложения и иконки установки для Android, iOS и рабочего стола.
- Добавлена кнопка установки PWA. На iPhone показываются шаги Safari → Поделиться → На экран Домой.
- Обновлён кэш Service Worker до `vampgram-shell-v3`.

## Важное ограничение Firestore

Исходную фотографию можно выбрать любого размера, но перед отправкой приложение оптимизирует её. Это нужно, потому что файлы здесь хранятся фрагментами в Cloud Firestore, а не в файловом CDN. Изображения в профиле хранятся в пределах 460 КБ после обработки, изображения в чате — до 5.5 МБ после обработки. Документы, аудио и видео по-прежнему ограничены 6 МБ. Для действительно неограниченных видео и файлов нужен отдельный файловый сервис; в этой сборке Firebase Storage не добавлялся.

## Firebase

Проект использует Firebase Authentication и Cloud Firestore. Firebase Storage и Realtime Database не используются.

1. Проверь Firebase Authentication → Sign-in method → Email/Password.
2. Создай/проверь Firestore в режиме Native.
3. Опубликуй `firestore.rules` в Firebase Console → Firestore → Rules.
4. При необходимости создай индексы из `firestore.indexes.json`.
5. Разверни хостинг и правила:

```bash
firebase deploy --only hosting,firestore:rules,firestore:indexes
```

Конфигурация Firebase уже находится в `firebase.json` и `.firebaserc`.

## Запуск локально

Windows: открой `start-local.bat`.

Или запусти в этой же папке:

```bash
python -m http.server 8000
```

Открой `http://127.0.0.1:8000`.

## Установка на устройства

- **Android:** открой опубликованный HTTPS-сайт в Chrome и нажми кнопку загрузки внутри Vampgram. Также можно выбрать «Установить приложение» в меню браузера.
- **iPhone / iPad:** открой сайт в Safari, нажми «Поделиться» → «На экран Домой» → «Добавить».
- **ПК:** используй значок установки в адресной строке Chrome/Edge или кнопку установки в приложении.

Архив содержит PWA, а не скомпилированный APK. Для APK нужна отдельная сборка Android WebView/Capacitor.

## Структура файлов в корне

- `index.html` — приложение
- `firebase.json`, `.firebaserc` — Firebase CLI
- `firestore.rules`, `firestore.indexes.json` — Firestore
- `manifest.webmanifest`, `sw.js` — PWA
- `vampgram-192.png`, `vampgram-512.png`, `vampgram-1024.png`, `apple-touch-icon.png`, `favicon.png`, `vampgram.svg` — иконки
- `start-local.bat` — локальный запуск Windows
- `README.md` — эта инструкция
