# slayfuck

Личная посадочная страница со ссылкой на Telegram-канал `@solllace_1`, фоновой музыкой и анимированными лепестками.

## Стек
Одна HTML-страница (`index.html`) с инлайновыми CSS и JS. Без сборки, без зависимостей — просто открывай в браузере.

- `index.html` — разметка, стили, анимация букв, лепестки (canvas), кнопка музыки.
- `photo.webp` / `photo.jpeg` — фон, подхватывается через `image-set` (webp для современных браузеров, jpeg как фоллбэк и для `og:image`).
- `glamur.mp3` — фоновый трек (96 kbps, ~2 МБ).
- `manifest.webmanifest` + `sw.js` — PWA: страницу можно «установить» на телефон, и она работает оффлайн после первого визита.
- `apple-touch-icon.png`, `icon-512.png` — иконки для iOS/Android.

## Локальный запуск
Service worker не работает из `file://`, так что нужен локальный HTTP-сервер:

```sh
python3 -m http.server 8000
# открой http://localhost:8000/
```

## Деплой
`.github/workflows/pages.yml` автоматически публикует содержимое `main` на GitHub Pages при каждом push.
Чтобы включить Pages в первый раз:
1. Settings → Pages → **Source: GitHub Actions**.
2. Push в `main`, дождись зелёной галочки в Actions.
3. Страница появится по адресу `https://pheisn.github.io/slayfuck/`.

## Полезные команды
```sh
# Пережать трек, если заменил mp3
ffmpeg -i input.mp3 -b:a 96k glamur.mp3

# Обновить webp после замены jpeg
cwebp -q 82 photo.jpeg -o photo.webp

# Пересобрать иконки из нового фото
convert photo.jpeg -gravity center -crop 1:1 +repage -resize 180x180 apple-touch-icon.png
convert photo.jpeg -gravity center -crop 1:1 +repage -resize 512x512 icon-512.png
pngquant --force --quality 70-85 --speed 1 --output icon-512.png icon-512.png
```

После замены любого ассета подними номер версии в `sw.js` (`const CACHE = 'slayfuck-v2'`), иначе пользователи будут получать старое из кеша.
