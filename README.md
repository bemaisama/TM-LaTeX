# TM-LaTeX
TM LaTeX es un micro-motor y UI sin dependencias que convierte texto o selecciones en LaTeX de una sola línea. Normaliza operadores y decimales, detecta fracciones, añade \left...\right, soporta formatos (line, array, aligned, cases, pmatrix/bmatrix), preserva saltos, ofrece atajos (Alt+L, Alt+Shift+L) e inserta o copia al portapapeles.


# TM LaTeX — “una sola línea” para la web

Convierte texto o selecciones en **LaTeX de una sola línea**, con UI flotante y atajos. Ideal para Google Docs, LMS y foros.

> Archivos principales: `engine-core.js` (motor) y `ui.js` (interfaz).

---

## ✨ Características

* Conversión instantánea a `$…$` (inline) o `$$…$$` (display).
* Normaliza operadores (× ÷ ≤ ≥ ≠ ≈), decimales (., ,) y entidades HTML.
* Detecta fracciones atómicas `a/b → \frac{a}{b}` y trata `x` entre números como multiplicación.
* Formatos: `line`, `array` (1, 2 o N), `aligned`, `gathered`, `cases`, `pmatrix`, `bmatrix`.
* Opción de `\left(\right)` automática por longitud.
* Respeta saltos de línea como `\\` (opcional).
* UI sin dependencias, Shadow DOM, persistencia en `localStorage`.
* Inserción directa en la selección o copia al portapapeles.

---

## ⌨️ Atajos

* **Alt+L**: abrir/cerrar panel.
* **Alt+Shift+L**: aplicar rápido a la selección (sin abrir UI).
* **Alt+A** (con panel abierto): aplicar en el documento.
* **Ctrl/Cmd+C**: copiar vista previa.
* **Esc**: cerrar.

---

## 🚀 Instalación

### Opción A: Userscript (Tampermonkey)

1. Instala [Tampermonkey].
2. Crea un nuevo script y pega el contenido de `engine-core.js` y `ui.js` (o usa tu bundle).
3. Ajusta los `@match` según los sitios donde quieras usarlo.

### Opción B: Incrustado en tu página

```html
<script src="/path/engine-core.js"></script>
<script src="/path/ui.js"></script>
<!-- Se autoinicializa: botón lateral “LaTeX” y hotkeys -->
```

---

## 🧭 Uso básico

1. Selecciona texto en la página **o** pega en el panel (Alt+L).
2. Revisa/ajusta opciones (decimal, formato, etc.).
3. **Aplicar** para reemplazar la selección o **Copiar** para portapapeles.
4. Para máxima velocidad, usa **Alt+Shift+L** sobre la selección.

---

## ⚙️ Opciones (principales)

| Opción                                     | Tipo / Ejemplo                          | Descripción                                    |
| ------------------------------------------ | --------------------------------------- | ---------------------------------------------- |
| `decimal`                                  | `"."` / `","`                           | Separador decimal preferido.                   |
| `wrapMode`                                 | `"inline"` / `"display"`                | Envoltura `$…$` o `$$…$$`.                     |
| `preserveLineBreaks`                       | `true/false`                            | Convierte saltos en `\\`.                      |
| `insertQuadBetweenBlocks`                  | `true/false`                            | Inserta `\quad` entre bloques.                 |
| `fracAtomic` / `atomicMaxLen`              | `true`, `2`                             | `a/b` → `\frac{a}{b}` si ambos “cortos”.       |
| `autoLeftRight` / `autoLeftRightThreshold` | `true`, `20`                            | Añade `\left(\right)` si el interior excede N. |
| `dontRewrapIfHasDollar`                    | `true/false`                            | No reenvolver si ya hay `$`.                   |
| `treatXBetweenNumbersAsMul`                | `true/false`                            | `2x3` → `2 \times 3`.                          |
| `format`                                   | `"line"`, `"arrayN"`, `"aligned"`, etc. | Entorno de salida.                             |
| `arrayN` / `arrayAlign`                    | `2`, `"ll"`                             | Columnas y alineación.                         |
| `opMap` / `numOpNumOverride`               | objeto                                  | Mapa de operadores.                            |
| `removeDiacritics`                         | `true/false`                            | Quitar tildes del texto.                       |

> Los valores por defecto están en `TM_LaTeX.DEFAULT_OPTS`.

---

## 🧪 API del motor

```js
// Convertir una cadena con opciones (opcionales)
const out = TM_LaTeX.convert('3x(2+4)/5', {
  wrapMode: 'display', decimal: '.', fracAtomic: true
});

// Defaults y utilidades
TM_LaTeX.DEFAULT_OPTS
TM_LaTeX.utils.decodeEntities(str)
TM_LaTeX.utils.normalizeUnicodeOperators(str)
TM_LaTeX.utils.detectOperators(str)
TM_LaTeX.utils.deepMerge(base, extra)
TM_LaTeX.utils.balanceReport(str)
TM_LaTeX.utils.excludeTextBlocks(latex)
```

---

## 🧾 Ejemplos

* `3x(2+4)/5` → `$$ \frac{3 \times (2+4)}{5} $$`
* `a) 2,5 : 0,5   b) 3/4 + 1/2` (coma decimal) →
  `$$ \begin{array}{ll} a)~2,5 \times 0,5 \quad b)~\frac{3}{4}+\frac{1}{2} \end{array} $$`

---

## 🔒 Privacidad y permisos

* No realiza llamadas de red.
* Usa `localStorage` para opciones/estado.
* Inserta texto en la selección actual (documento e iframes); si falla, copia al portapapeles.

---

## 🧩 Compatibilidad

* Navegadores modernos con Shadow DOM y portapapeles.
* Funciona embebido o como userscript.

---

## 🛠️ Desarrollo

* JS plano, sin dependencias ni bundlers obligatorios.
* Estructura sugerida del repo:

  ```
  /engine-core.js
  /ui.js
  /docs/screencast.gif   (opcional)
  /docs/panel.png        (opcional)
  ```
* PRs y issues son bienvenidos.

---

## 📜 Licencia

MIT. Ver `LICENSE`.

---

## 🇬🇧 README (English)

**TM LaTeX** converts plain text or selections into **single-line LaTeX** with a lightweight, dependency-free UI and hotkeys.

**Features**

* `$…$` or `$$…$$` wrapping; operator & decimal normalization; HTML entity decoding.
* Atomic fractions (`a/b → \frac{a}{b}`), `x` between numbers as multiplication.
* Output formats: `line`, `array(1/2/N)`, `aligned`, `gathered`, `cases`, `pmatrix`, `bmatrix`.
* Optional `\left(\right)` based on content length; preserve line breaks as `\\`.
* Shadow-DOM UI, persistent options, direct insertion or clipboard fallback.

**Hotkeys**

* Alt+L (toggle), Alt+Shift+L (quick apply), Alt+A (apply), Ctrl/Cmd+C (copy), Esc (close).

**Install**

* Userscript (Tampermonkey) or embed `<script src="engine-core.js"></script><script src="ui.js"></script>`.

**API**

```js
const out = TM_LaTeX.convert('3x(2+4)/5', { wrapMode: 'display' });
```

**Privacy**

* No network calls; uses `localStorage`; inserts into current selection or copies to clipboard.

**License**

* MIT.

---

## ❤️ Agradecimientos

A toda la comunidad que empuja herramientas simples para flujos reales de docentes, creadores y estudiantes.
