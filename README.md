# Cruza Horarios · UTP

Una herramienta para armar tu horario de clases sin que las materias choquen entre sí — sin tener que hacerlo a mano, cuadro por cuadro, probando combinaciones.

🔗 **Sitio en vivo:** https://marianc-programing.github.io/HorarioWeb/

## ¿Qué problema resuelve?

En la UTP, cada materia se ofrece en varias secciones (grupos), cada una con su propio día, hora y aula. Elegir qué secciones tomar para que no se te crucen dos clases al mismo tiempo, y que además caigan en el turno que prefieres, es un rompecabezas tedioso de hacer manualmente.

Esta herramienta automatiza esa parte: le subes tu horario oficial (como imagen o PDF), le dices qué materias necesitas y en qué turno prefieres estudiar, y te muestra todas las combinaciones posibles que **no se cruzan entre sí**, una por una, con la cuadrícula semanal visual — igual que un horario oficial.

## Cómo se usa

1. **Cursos disponibles** — subes la captura o el PDF del horario. El sitio intenta leer la tabla automáticamente (materia, días, horas, aula, profesor) y te muestra lo detectado en una pantalla de revisión editable, para que corrijas cualquier error antes de guardar nada. También puedes cargar secciones a mano si prefieres.
2. **Elegir materias** — marcas qué materias necesitas cursar y tu turno preferido (mañana, tarde, noche, o cualquiera).
3. **Combinaciones** — el sitio genera todas las combinaciones de secciones que no chocan entre sí, y las navegas de una en una ("Opción 1 de N") viendo cómo quedaría tu semana.

## Cómo lee el horario automáticamente

El sitio puede leer dos tipos de archivo:

- **PDF con texto real** (el formato en que suele exportarse el horario oficial): se lee el texto exacto, sin necesidad de reconocimiento óptico — es el método más confiable.
- **Imágenes** (capturas o fotos): se usa reconocimiento óptico de caracteres (OCR) para extraer el texto, con tolerancia a errores comunes de lectura (acentos, letras confundidas, palabras partidas).

En ambos casos, el sitio reconoce la estructura típica del horario de la UTP (horas en filas, días en columnas) y, si el documento incluye la tabla de asignaturas con profesores y códigos, la cruza automáticamente para completar esos datos sin que tengas que escribirlos. Nada se guarda sin que antes lo confirmes en la pantalla de revisión — el reconocimiento automático puede equivocarse, así que siempre queda la oportunidad de corregir o completar a mano.

## Privacidad

Todo el procesamiento ocurre en tu propio navegador: la lectura del documento, la comparación de horarios y el guardado de tus secciones. Nada se sube a un servidor externo ni se comparte con nadie — el sitio no tiene backend. Tus secciones quedan guardadas localmente en el navegador que usaste, así que seguirán ahí si recargas la página, pero no se sincronizan entre dispositivos distintos.

## Estado del proyecto

Este es un prototipo en desarrollo activo. Algunas ideas para más adelante:

- Editar una sección ya guardada sin tener que borrarla y volver a cargarla.
- Ordenar las combinaciones sin choque por criterios como menos días en el campus o menos huecos entre clases.
- Exportar el horario elegido para importarlo a un calendario (Google Calendar, etc.).
