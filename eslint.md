## Установка

```bash
npm install --save-dev eslint @eslint/js globals
```
---

## Зачем нужен каждый пакет

- `eslint` — сам линтер.
- `@eslint/js` — готовый набор правил (recommended).
- `globals` — список глобальных переменных (чтобы линтер не ругался на window, document).

---

## Как запустить?

```bash
npm run lint
```

Для автоисправления:

```bash
npm run lint -- --fix
```

---

## Готовый шаблон конфига

> Создается новый файл в корневой папке проекта и называете его eslint.config.js.

```javascript
// eslint.config.js (CommonJS)
const js = require("@eslint/js");
const globals = require("globals");

module.exports = [
  js.configs.recommended,
  {
    languageOptions: {
      globals: { ...globals.browser },
    },
    rules: {
      "no-var": "error",
      "prefer-const": "error",
      "no-console": "warn",
    },
  },
];
```

---

## Настройки правил (как их настраивать)

Правила в ESLint можно настраивать. Например, для no-console:

- `"error"` — запрещает console полностью.

- `"warn"` — выдает предупреждение (желтым).

- `["warn", { allow: ["warn", "error"] }]` — разрешает только console.warn и console.error (обычно так делают в реальных проектах).

---

## Расширения js/mjs

Смотрите в package.json на строку "type".

Выбирайте расширение .js и пишите код через `require` и `module.exports`, если:

- В вашем package.json указано "type": "commonjs" (или этой строки нет вообще, так как CommonJS — это стандарт по умолчанию для Node.js).
- Вы хотите, чтобы файл работал в старых версиях Node.js или в проектах, где еще не перешли на современный синтаксис.

Выбирайте расширение .mjs и пишите код через import и export default, если:

- В вашем package.json указано `"type": "module"` (тогда можно использовать даже просто .js), НО вы хотите принудительно использовать ESM-синтаксис в конкретном файле, даже если весь проект настроен на CommonJS.
- Вы хотите использовать современный синтаксис, но не хотите менять настройки всего проекта в package.json.

> расширение .cjs работает наоборот: оно нужно, если в package.json стоит "type": "module", но для конкретного файла вам нужен старый синтаксис require.

---