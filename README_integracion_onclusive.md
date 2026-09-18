# Integración del extractor Onclusive

## Archivos
- `app_modificado_extractor_onclusive.py`: copia del programa original con un nuevo modo en el menú.
- `extractor_onclusive.py`: módulo independiente que realiza la extracción.

## Qué hace el nuevo modo
1. Pide un **nombre del tema** en lugar de un actor político.
2. Recibe Excel/CSV de Onclusive.
3. Conserva **X, Facebook, Instagram y TikTok**.
4. Elimina RT/retuits.
5. Elimina duplicados por enlace y por texto+fuente.
6. Limpia URLs incluidas en el contenido, códigos tipo `:warning:` y IDs numéricos de Facebook.
7. Recupera `@usuario` desde columnas del archivo y, cuando el permalink lo permite, desde la URL.
8. Opcionalmente intenta resolver usuarios públicos faltantes de Instagram/TikTok sin login.
9. Recorta publicaciones largas alrededor del nombre del tema.
10. Genera Word, HTML y TXT.

## Formato Word
- Encabezado `REDES SOCIALES (N)`.
- Lista de cuentas con conteo.
- Desglose por fecha.
- Fuente en negritas.
- Fuente, texto y link dentro de **un mismo párrafo**, separados por saltos manuales (Shift+Enter).
- Un párrafo nuevo entre menciones.
- Hipervínculos activos.

## Instalación
Coloca ambos `.py` en la misma carpeta del repositorio. Si tu archivo principal tiene otro nombre, puedes usar solo `extractor_onclusive.py` e integrar:

```python
from extractor_onclusive import render_extractor_onclusive
```

Añade al selector:

```python
"Extracción de menciones Onclusive por tema"
```

y en el flujo principal:

```python
if tipo_analisis == "Extracción de menciones Onclusive por tema":
    render_extractor_onclusive()
```

## Dependencias
Además de las que tu app ya usa:

```text
streamlit
pandas
python-docx
openpyxl
requests
```

`requests` solo se usa para el intento opcional de resolver usuarios públicos de Instagram/TikTok.

## Limitación de Instagram
Un enlace como `instagram.com/p/CODIGO/` o `instagram.com/reel/CODIGO/` **no contiene el nombre de usuario**. El módulo intenta obtenerlo de la página pública solo cuando activas la opción correspondiente. Si Instagram requiere login o bloquea la consulta, se conserva el nombre de autor que venga en Onclusive; no se inventa un `@usuario`.
