# Mapa de proyectos de la Estación Científica Agua y Páramo

El mapa utiliza `base_datos_proyectos_investigacion.csv` como fuente pública de los proyectos. El libro `Lista_Investigaciones_ECAP_RS.xlsx` incluye la hoja `Base_mapa`, preparada como versión editable de esa misma tabla. Al reemplazar o actualizar el CSV y recargar la página, los puntos, contadores, filtros y fichas informativas se reconstruyen automáticamente.

La versión pública está disponible en **[nachorock73.github.io/ecap-mapa-proyectos](https://nachorock73.github.io/ecap-mapa-proyectos/)**.

## Ejecutar el mapa localmente

No se debe abrir el HTML con una dirección que empiece por `file:///`, porque el navegador bloquea la lectura automática del CSV.

1. Abrir una terminal en esta carpeta (`investigacion`).
2. Ejecutar uno de estos comandos:

   ```powershell
   python -m http.server 8000
   ```

   En Windows también puede funcionar:

   ```powershell
   py -m http.server 8000
   ```

3. Mantener abierta la terminal mientras se usa el mapa.
4. Abrir en el navegador: <http://localhost:8000>
5. Para detener el servidor, volver a la terminal y presionar `Ctrl + C`.

El mapa necesita conexión a Internet para descargar Leaflet, Papa Parse y las teselas del mapa base.

## Buscar proyectos

El campo **Búsqueda por palabras** consulta simultáneamente título, autoría, línea prioritaria, tipo, estado y resumen. Al escribir dos o más caracteres se muestran debajo hasta ocho coincidencias ordenadas por relevancia, con el título, la autoría y un fragmento del resumen; las palabras coincidentes aparecen resaltadas. Al seleccionar un resultado, el mapa habilita sus filtros, acerca la vista al punto y abre su ficha lateral.

Al pasar el cursor sobre un punto aparece una vista breve con tipo, estado, título, línea prioritaria, autoría y año. Al hacer clic se abre una ficha fija a la derecha con esa información, el resumen, las coordenadas y el presupuesto. El texto de la ficha puede seleccionarse y el botón **Cerrar** la oculta.

## Actualizar la base con Excel

1. Hacer una copia de respaldo de `Lista_Investigaciones_ECAP_RS.xlsx` y de `base_datos_proyectos_investigacion.csv`.
2. Abrir `Lista_Investigaciones_ECAP_RS.xlsx` y editar la hoja `Base_mapa`.
3. No modificar los nombres ni el orden de los encabezados de esa hoja.
4. Agregar, eliminar o actualizar registros. El campo `presupuesto` puede quedar vacío cuando el dato todavía no esté disponible.
5. Exportar únicamente la hoja `Base_mapa` mediante **Archivo → Guardar como → CSV UTF-8 delimitado por comas (.csv)**.
6. Conservar exactamente el nombre `base_datos_proyectos_investigacion.csv` y reemplazar el archivo anterior en esta carpeta.
7. Recargar <http://localhost:8000>. El mapa solicita una copia reciente del CSV para evitar que el navegador muestre una versión almacenada en caché.

Antes de exportar, cada proyecto nuevo debe tener una `latitud` y una `longitud` válidas. No se deben combinar celdas ni agregar títulos por encima de los encabezados en `Base_mapa`. Para retirar un proyecto del mapa se elimina su fila completa; para ocultarlo temporalmente es preferible conservar una copia de respaldo fuera del CSV publicado.

Las comas y saltos de línea dentro de títulos o resúmenes son válidos cuando Excel guarda correctamente esos campos entre comillas. Las tildes, la `ñ` y otros caracteres se conservan al usar CSV UTF-8.

## Columnas de la base

Los diez encabezados deben existir, aunque algunos valores individuales puedan quedar vacíos.

| Columna | Uso | Regla por registro |
| --- | --- | --- |
| `titulo` | Nombre mostrado en el cuadro informativo | Opcional; si está vacío se muestra “Sin título” |
| `autores` | Autoría del proyecto | Opcional |
| `anio` | Año del proyecto o documento | Opcional |
| `Línea prioritaria de investigación ECAP` | Color del punto y filtro por línea prioritaria | Opcional; un valor nuevo crea automáticamente una opción de filtro |
| `tipo` | Filtro por tipo de archivo/proyecto | Opcional; un valor nuevo crea automáticamente una opción de filtro |
| `estado` | Filtro y contadores de proyectos en curso/finalizados | Opcional |
| `latitud` | Coordenada geográfica norte/sur en WGS84 | Obligatoria; número entre `-90` y `90` |
| `longitud` | Coordenada geográfica este/oeste en WGS84 | Obligatoria; número entre `-180` y `180` |
| `resumen` | Resumen mostrado al consultar el punto | Opcional |
| `presupuesto` | Aporte o presupuesto del proyecto en USD; se muestra en la ficha lateral | Opcional; usar un número sin símbolo de moneda o dejar vacío |

Para las coordenadas se recomienda usar punto decimal, por ejemplo `-0.230391` y `-78.154659`. El lector también tolera una coma decimal cuando el valor está correctamente entre comillas dentro del CSV.

## Sistema de coordenadas de los puntos

Los puntos están almacenados en el sistema geográfico **WGS 84, EPSG:4326**, expresado en grados decimales:

- `latitud`: posición norte/sur, entre `-90` y `90`;
- `longitud`: posición este/oeste, entre `-180` y `180`;
- el orden en el CSV es `latitud`, luego `longitud`;
- en Ecuador, ambos valores suelen ser negativos para ubicaciones al sur del ecuador y al oeste de Greenwich;
- se usa punto como separador decimal y no se añaden símbolos de grados, minutos, segundos ni letras de hemisferio.

Cuando un documento declara coordenadas, la base conserva el valor exacto convertido a grados decimales si el original estaba en otro formato. No se deben desplazar ni redondear esos puntos para evitar coincidencias visuales: el mapa separa únicamente su representación en pantalla cuando dos registros comparten la misma coordenada, sin modificar el CSV.

Si una fuente entrega coordenadas proyectadas —por ejemplo UTM—, deben transformarse a **EPSG:4326** con el huso y datum indicados por el documento antes de incorporarlas. Si el datum o el huso no están claros, no se debe asumir una conversión.

## Capas cartográficas

El filtro muestra las capas en este orden: límites provinciales, cantonales y parroquiales; Ejes FONAG; Áreas de Conservación Hídrica; Quito urbano; cobertura de la tierra 2024; vías; y ríos. Las vías se representan en gris y los ríos en celeste brillante. El mapa base usa directamente las teselas públicas de OpenStreetMap y no requiere una clave de API.

Al abrir el mapa, las únicas capas cartográficas activas son **Ejes FONAG** y **Cobertura de la tierra 2024**; los puntos de los proyectos también se muestran completos. Todas las demás capas y los tres tipos de estaciones comienzan desactivados.

Las vías y los ríos publicados fueron recortados geométricamente con el límite de Ejes FONAG antes de transformarse a WGS 84 geográfico e incorporarse al HTML. La versión pública contiene únicamente las porciones interiores resultantes; no necesita descargar archivos SHP durante su uso.

La cobertura 2024 fue disuelta mediante su clasificación de nivel 2 y posteriormente recortada con Ejes FONAG. La capa publicada contiene una geometría por clase presente dentro del ámbito. Su flecha permite desplegar u ocultar la leyenda cromática; el páramo se representa en morado para distinguirlo con claridad.

## Estaciones

El apartado **Estaciones** permite activar de forma independiente las estaciones hidrológicas, meteorológicas y pluviométricas. Todas se muestran con símbolos triangulares y un color diferente por tipo. Al situar el cursor sobre una estación se muestran su nombre, tipo, código, altitud, estado y provincia cuando esos atributos están disponibles. La versión publicada contiene 60 estaciones activas transformadas a WGS 84.

## Validación automática

Al cargar la base, el mapa:

- ignora filas completamente vacías;
- descarta únicamente las filas cuya latitud o longitud estén vacías, no sean numéricas o estén fuera de rango;
- mantiene funcionando el resto de los registros;
- muestra en la consola del navegador el número de cada fila descartada y su motivo;
- muestra un mensaje visible si falta algún encabezado o si el CSV no puede leerse.

Para revisar avisos técnicos en Chrome o Edge, abrir las herramientas de desarrollo con `F12` y consultar la pestaña **Consola**.

## Archivos principales

- `index.html`: entrada compatible con GitHub Pages.
- `mapa_proyectos.html`: mapa institucional y lógica de lectura del CSV.
- `base_datos_proyectos_investigacion.csv`: fuente pública que consume el mapa.
- `Lista_Investigaciones_ECAP_RS.xlsx`: base editable institucional; su hoja `Base_mapa` replica la tabla publicada e incluye el campo de presupuesto.
- `logo.png`: logotipo del encabezado.

El personal encargado de actualizar proyectos puede trabajar en la hoja `Base_mapa` del Excel y exportarla como CSV. No necesita ejecutar los scripts de `_work_consolidacion`.

## Actualizar la versión publicada en GitHub Pages

El repositorio público es [Nachorock73/ecap-mapa-proyectos](https://github.com/Nachorock73/ecap-mapa-proyectos). La solución es completamente estática y usa rutas relativas, por lo que no requiere servidor de aplicaciones ni base SQL.

Para publicar una actualización de la base:

1. Validar primero el CSV en la versión local.
2. Reemplazar `base_datos_proyectos_investigacion.csv` en la raíz del repositorio, sin cambiar su nombre.
3. Confirmar el cambio mediante un commit en GitHub o con Git.
4. Esperar a que GitHub Pages termine el despliegue.
5. Abrir <https://nachorock73.github.io/ecap-mapa-proyectos/> y verificar el número de proyectos, los filtros y varios puntos.

La versión pública contiene información y coordenadas exactas; cualquier fila nueva debe contar con autorización institucional antes de incorporarse.
