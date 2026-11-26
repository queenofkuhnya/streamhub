# StreamHub — для разработчика

[← назад к README](../README.md)

## Сборка StreamHub.exe
```
npm install
npx @yao-pkg/pkg . --compress GZip --no-bytecode --public --public-packages "*" --targets node22-win-x64 --output dist/StreamHub.exe   # точка входа — main.js
```
Иконка: перед сборкой вшей её в основу Node (`build/seticon.mjs`, нужен пакет `resedit`) и собери с `PKG_NODE_PATH`, указывающим на изменённую основу:
```
node build/seticon.mjs <кэш pkg>/fetched-v22.x-win-x64 base-icon.bin build/StreamHub.ico 0.10.5
PKG_NODE_PATH=base-icon.bin npx @yao-pkg/pkg . --compress GZip --no-bytecode --public --public-packages "*" --targets node22-win-x64 --output dist/StreamHub.exe
```
Всё из папки `public` попадает внутрь exe. Настройки создаются в `%LOCALAPPDATA%\StreamHub` при первом запуске (или в `data` рядом с exe в портативном режиме).

### Выпуск новой версии (обновление по кнопке)
Программа ищет обновления в GitHub Releases репозитория из поля `updateRepo` в `package.json`. Репозиторий должен быть **публичным** (исходники класть туда не обязательно).
1. Подними `version` в `package.json` (например, `0.9.4`) и собери `dist/StreamHub.exe`.
2. На GitHub: *Releases → Draft a new release*, тег `v0.9.4`, в описании напиши, что нового (это увидят стримеры в плашке).
3. Прикрепи файл, который называется ровно **`StreamHub.exe`** (zip можно приложить рядом для новых пользователей) → *Publish release*.

Программа проверяет обновления через 8 секунд после запуска и потом раз в 6 часов. Скачанный файл сверяется по размеру и SHA-256 (GitHub считает хэш сам).
