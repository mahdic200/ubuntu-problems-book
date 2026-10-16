```js
// minimumIntegerDigits, useful for padding
(5).toLocaleString('fa-IR', { minimumIntegerDigits: 2 }); // "۰۵"

// combine with number formatting styles
(1234.5).toLocaleString('fa-IR', { style: 'decimal', useGrouping: false }); // "۱۲۳۴٫۵"
```