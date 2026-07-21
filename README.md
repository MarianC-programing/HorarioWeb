# HorarioWeb

Prototipo estático (sin backend) para armar combinaciones de secciones de la UTP sin choques de horario.

Todo corre en el navegador: extracción de texto por OCR (Tesseract.js) o directamente desde PDF con texto real (pdf.js), captura manual de secciones y el algoritmo de combinaciones sin choque. No hay servidor ni base de datos — tus secciones se guardan con `localStorage` en el navegador que usés, así que sobreviven a recargar la página, pero son locales a ese navegador/dispositivo.

## Uso local

Abre `index.html` directamente en el navegador, o sirve la carpeta con cualquier servidor estático:

```bash
python3 -m http.server 8000
```

y visita `http://localhost:8000`.

## Publicar en GitHub Pages

```bash
cd /home/marian01/ProyectosDev/HorarioWeb
git init
git add .
git commit -m "Prototipo inicial: comparador de horarios UTP"
git branch -M main
git remote add origin <URL_DE_TU_REPO_EN_GITHUB>
git push -u origin main
```

Luego, en el repositorio en GitHub: **Settings → Pages → Branch: main → carpeta: / (root) → Save**.
El sitio quedará publicado en `https://<tu-usuario>.github.io/HorarioWeb/`.

## Flujo actual

1. **Cursos disponibles** — sube una captura del horario (imagen) o el PDF oficial. Si es un PDF con texto real (no escaneado), la extracción usa `pdf.js` y es mucho más confiable que el OCR, porque lee el texto exacto en vez de reconocerlo carácter por carácter. Si es una imagen, se usa OCR (Tesseract.js). En ambos casos, si el formato de tabla es reconocible (horas en filas, días en columnas, como el formato oficial de la UTP), aparece un botón "Detectar secciones automáticamente (beta)" que arma una propuesta de secciones editable. También puedes cargar secciones a mano: materia, sección, profesor, código y bloques de día/hora/aula.
2. **Elegir materias** — marca qué materias necesitas y tu turno preferido (mañana/tarde/noche/cualquiera).
3. **Combinaciones** — genera todas las combinaciones posibles sin choque, navegables como "Opción 1 de N", con la cuadrícula visual semanal.

### Cómo funciona la detección automática (beta)

- Tanto Tesseract.js (imágenes) como pdf.js (PDFs) entregan al motor de detección el mismo formato: una lista de "palabras" con su posición (x, y). Para PDFs, cada fragmento de texto se separa en palabras individuales repartiendo su ancho proporcionalmente, para que encaje con el resto del motor.
- El motor agrupa palabras en filas por cercanía vertical (el margen de tolerancia se calcula según la altura real del texto en cada archivo, no un valor fijo), ubica la fila de encabezado (Lunes...Domingo) tolerando errores de OCR (acentos, letras mal leídas, palabras partidas en dos), y si el texto no alcanza para reconocer los días, cae en un respaldo posicional: asume que la primera fila del recorte son los días y los asigna en orden fijo. Luego ubica filas de datos por el patrón de hora (`7:00-7:45A.M.`, tolerante a que falte algún `:`) en el borde izquierdo.
- Cada celda se interpreta como `MATERIA (SECCIÓN) AULA: X-XXX`, y las celdas con la misma materia/sección se agrupan en una sola sección con varios bloques de día/hora.
- Si una materia ocupa varias franjas horarias seguidas el mismo día en la misma aula (ej. 7:00-7:45 y 7:50-8:35), se fusionan en un solo bloque (7:00-8:35) en vez de mostrarse como bloques sueltos. Si el aula cambia a mitad de sesión, se mantienen separados.
- Si una celda tiene el nombre de la materia en una línea y `AULA: X-XXX` en la línea de abajo (común en PDFs), esa segunda línea se reconoce como parte de la misma celda en vez de perderse.
- **Autocompletado de profesor y código**: si el documento trae la tabla de leyenda (ASIGNATURA / CODASIG / CODHORA / PROFESOR / CORREO) que suele venir debajo de la cuadrícula, se lee y se cruza por nombre de materia para llenar esos campos automáticamente en cada sección detectada.
- La lectura de la cuadrícula principal se corta antes de llegar a esa tabla de leyenda (si existe), para no confundir sus filas con una continuación de la última celda de la cuadrícula — eso evitaba que un código de horario o un correo de profesor terminara colado dentro del nombre de una materia.
- **Nunca se guarda directo**: siempre pasa por una pantalla de revisión editable (con botones para agregar o quitar días/horas por sección, y campo de profesor visible) antes de agregarse a tu lista, porque el reconocimiento puede leer mal un carácter o desalinear una celda — esto es más probable con imágenes fotografiadas que con PDFs de texto real.
- **Si no detecta nada**, aparece un cuadro de diagnóstico (fondo amarillo) con las filas que se lograron leer y por qué no calificaron como encabezado o fila de datos. Sirve para reportar el problema con datos concretos en vez de "no funciona".
- Con imágenes, funciona mejor con capturas nítidas, derechas (sin rotación) y con buen contraste — fotos en ángulo, con brillo/reflejo, o muy comprimidas, bajan mucho la precisión. Con PDFs de texto real, la precisión debería ser consistentemente alta; si el PDF es un escaneo (imagen dentro del PDF), el aviso te lo indica y conviene subirlo como imagen en su lugar.

## Diseño responsive

La interfaz se adapta a pantallas de teléfono: formularios en una sola columna, filas de día/hora/aula en cuadrícula 2×2, y la tabla de resultados semanal vive dentro de un contenedor con scroll horizontal (en vez de aplastarse hasta ser ilegible).

## Próximos pasos sugeridos

- Recorte/enderezado automático de la imagen antes del OCR para mejorar la detección en capturas torcidas.
- Exportar/importar el horario cargado como JSON (para moverlo entre navegadores/dispositivos).
- Editar una sección ya guardada sin tener que borrarla y rehacerla.
- Ordenar las combinaciones sin choque por criterios como menos días en el campus o menos huecos entre clases.
