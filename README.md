# Dori.pro — система управления аптечной сетью

Лендинг **Dori.pro** — облачной системы управления аптечной сетью: POS-кассы, склад, автозакупки, автораспределение товара по аптекам (FEFO), логистика, отчётность и KPI.

## Технологии

- [Vue 3](https://vuejs.org/) (`<script setup>` SFC)
- [Vite](https://vite.dev/)
- [Tailwind CSS](https://tailwindcss.com/)
- GitHub Actions + GitHub Pages (автодеплой)

## Запуск локально

```bash
npm install
npm run dev
```

Сборка и предпросмотр продакшена:

```bash
npm run build
npm run preview
```

## Деплой

Сайт развёрнут на GitHub Pages: <https://dori.pro>.

При каждом `git push` в ветку `main` GitHub Actions собирает проект (`npm run build`) и публикует папку `dist` автоматически (`.github/workflows/deploy.yml`). Кастомный домен задаётся файлом `public/CNAME`.

DNS домена `dori.pro` настроен на GitHub Pages: apex-записи `A` указывают на IP-адреса GitHub Pages, а `www` — CNAME на `digitaiam.github.io`.

## Форма заявки

Заявки с сайта отправляются через [Formspree](https://formspree.io/) на почту компании.
