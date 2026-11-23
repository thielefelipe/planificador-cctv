# Planificador CCTV

Editor web para planificar la instalación de cámaras de seguridad sobre planos de arquitectura.
Se ubica cada cámara, se orienta y se ve qué área cubre (los muros cortan la cobertura). El resumen dimensiona el grabador, el disco y el PoE.

**App en línea:** https://thielefelipe.github.io/planificador-cctv/

## Funciones
- **Planos**: se suben como imagen (JPG/PNG) o PDF (eliges qué páginas importar). La escala se calibra marcando una medida conocida.
- **Muros**: detección automática desde el plano o dibujo manual. La cobertura se calcula con trazado de rayos y se corta en los muros.
- **Cámaras**: tipos configurables (bullet, domo, turret, PTZ, 360°). Cada una tiene dirección, ángulo de visión, alcance, distancia de identificación, fase, altura y notas.
- **Resumen**: cuenta las cámaras por tipo, fase y plano, y estima los canales del NVR, los TB de disco según los días de grabación, el consumo PoE y el costo.
- **Exportar**: plano marcado en PNG, listado en CSV (abre en Excel) y proyecto en JSON, con o sin las imágenes de los planos.
- **Deshacer y rehacer**, varios proyectos y atajos de teclado (V, C, W, M, R, D, Supr, F).

## Dónde se guardan los datos
Todo queda **en el navegador** de quien usa la app: los proyectos en `localStorage` y las imágenes de los planos en IndexedDB. Nada se sube a un servidor.
Para respaldar un proyecto o llevarlo a otro equipo, usa **Archivo → Exportar proyecto (JSON)** con las imágenes incluidas y luego **Importar**.

> Los planos y las ubicaciones de cámaras de un recinto real son información sensible: no los subas a este repositorio público.

## Desarrollo
Es un solo archivo (`index.html`) sin dependencias de compilación. pdf.js se carga desde cdnjs sólo al importar un PDF.

```bash
python3 -m http.server 8000   # y abrir http://localhost:8000
```

Si la app se abre como artifact dentro de claude.ai, usa la base de datos y el almacenamiento de archivos del artifact en lugar del navegador.

### Modelo de datos
- `floors[]`: `{id, name, w, h, pxPerM, walls:[[x1,y1,x2,y2],...], local|asset|src}`. Las coordenadas están en píxeles de la imagen.
- `cameras[]`: `{id, code, floor, x, y, dir (°, 0 = este, 90 = sur), fov, range (m), ident (m), type, phase, zone, height, notes}`
- `types[]`: `{id, name, color, fov, range, ident, mbps, poe, price}`

### Ideas pendientes
- Zonas de privacidad: aviso si una cámara cubre baños u otras zonas prohibidas.
- Mapa de calor y % de área cubierta por plano.
- Trazado del cableado hasta los switches, con metraje de UTP.
- Exportación a PDF con la tabla de cámaras.
