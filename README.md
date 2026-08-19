# Proyectos en la Estación Científica Agua y Páramo

Mapa interactivo institucional de los proyectos vinculados con la Estación Científica Agua y Páramo (ECAP).

## Abrir el mapa

La versión pública se consulta en **[nachorock73.github.io/ecap-mapa-proyectos](https://nachorock73.github.io/ecap-mapa-proyectos/)** mediante GitHub Pages. El mapa permite:

- filtrar proyectos por tipo, estado y línea prioritaria de investigación ECAP;
- consultar título, autores, año, resumen y coordenadas de cada proyecto;
- activar y filtrar capas de límites territoriales, vías, ejes del FONAG y Áreas de Conservación Hídrica;
- ajustar la vista a los proyectos y capas seleccionados.

## Archivos publicados

- `index.html`: entrada del sitio.
- `mapa_proyectos.html`: interfaz y cartografía interactiva.
- `base_datos_proyectos_investigacion.csv`: base consolidada de proyectos.
- `logo.png`: identidad visual del encabezado.

El sitio es estático. Para actualizar los proyectos, se reemplaza el CSV conservando exactamente sus encabezados y su codificación UTF-8.

## Uso local

El navegador debe acceder al mapa mediante un servidor local, no con una dirección `file:///`. Desde esta carpeta puede ejecutarse:

```powershell
python -m http.server 8000
```

Luego se abre <http://localhost:8000>. Se necesita conexión a Internet para cargar las librerías cartográficas y el mapa base.
