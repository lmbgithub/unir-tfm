# Plantilla LaTeX (TFE)

Fuentes en `src/`; el PDF generado es `main.pdf` en la raíz del proyecto. La compilación usa **XeLaTeX**, **biber** (bibliografía APA con `biblatex`) y **makeindex** si existe un índice.

## Requisitos

- **GNU Make**
- **XeLaTeX** (`xelatex`)
- **biber**
- **makeindex** (incluido en distribuciones TeX completas)

Distribución TeX recomendada (incluye paquetes como `fontspec`, `unicode-math`, `biblatex`, `babel`, etc.):

| Sistema | Instalación orientativa |
|--------|-------------------------|
| **macOS** | [MacTeX](https://tug.org/mactex/) o `brew install --cask mactex-no-gui`. Con BasicTeX, amplía con `sudo tlmgr update --self && sudo tlmgr install collection-fontsrecommended collection-langspanish` (y paquetes que falten según el log). |
| **Linux (Debian/Ubuntu)** | `sudo apt install make texlive-xetex texlive-lang-spanish texlive-lang-english texlive-bibtex-extra biber` (o el metapaquete `texlive-full` si prefieres todo instalado). |
| **Windows** | [MiKTeX](https://miktex.org/) o [TeX Live](https://tug.org/texlive/); habilita **XeLaTeX** y **biber** en el instalador o instálalos cuando el gestor lo solicite. |

**Fuentes:** el preámbulo intenta usar Calibri Light / Calibri y Menlo si están en el sistema; si no, usa **Carlito** (paquete TeX) y una monoespaciada sustituta. En Linux puede hacer falta instalar fuentes Calibri o confiar en Carlito.

## Compilar

En la **raíz del repositorio** (donde está el `Makefile`):

```bash
make
```

Equivale a `make all`: genera el PDF y valida que `main.pdf` exista y tenga tamaño razonable.

### Objetivos útiles

| Comando | Descripción |
|--------|-------------|
| `make` o `make all` | Compilación completa (XeLaTeX → biber → makeindex si aplica → XeLaTeX ×2) y validación del PDF. |
| `make pdf` | Solo construye `main.pdf` (mismo flujo de pasadas). |
| `make quick` | Una sola pasada de XeLaTeX; útil para borradores rápidos (referencias e índice pueden quedar desactualizados). |
| `make validate` | Comprueba `main.pdf` (requiere PDF ya generado). |
| `make clean` | Elimina auxiliares en la raíz (`.aux`, `.log`, `.bcf`, etc.). |
| `make cleanall` | Como `clean` y además borra `main.pdf`. |
| `make help` | Lista los objetivos anteriores. |

Salida: **`main.pdf`** en el directorio del proyecto.

## Problemas frecuentes

- **`xelatex` o `biber` no encontrado:** la ruta de la distribución TeX no está en el `PATH`; reinicia la terminal tras instalar o añade el directorio `bin` de TeX al `PATH`.
- **Falta un paquete LaTeX:** instálalo con el gestor de tu distribución (`tlmgr`, MiKTeX Console, `apt`, etc.) según el mensaje de error.
- **Bibliografía vacía o citas `[?]`:** ejecuta `make` completo (no solo `make quick`) para que corra `biber`.
