# Historia - Árbol de Decisiones

## Estructura del Proyecto

```
web/                  ← Esto va a GitHub Pages
  index.html          ← La web que ve ella en el iPhone
  story.json          ← Los datos del árbol (se actualiza)

admin/                ← Tu herramienta local (NO se sube)
  lab.html            ← Abre en navegador: crear, leer, subir
```

## Cómo funciona

### WEB (index.html)
- Ella abre la web en el iPhone
- Ve la entrada actual (texto, imagen, animación...)
- Elige decisiones → avanza por el árbol
- Menú izquierda: diario (escribe pensamientos), guardar imagen
- Botón carta abajo: exporta JSON con todo lo que hizo → te lo envía por WhatsApp

### LAB (lab.html)
- **Tab "Crear"**: Creas entradas con preview en tiempo real
- **Tab "Leer Export"**: Cargas el JSON que te envió → reporte legible
- **Tab "Subir"**: Con token GitHub subes story.json actualizado
- **Izquierda**: Árbol navegable de todas las entradas
- **Derecha**: Decisiones de la entrada seleccionada

---

## CÓMO AÑADIR COSAS (Guía de Extensión)

### Nuevo tipo de entrada en la WEB

En `index.html`, busca `AÑADIR NUEVOS RENDERERS AQUÍ` y pega:

```javascript
RENDERERS.miTipo = function(data) {
  return `<div class="fade-in">MI HTML AQUÍ</div>`;
};
```

### Nuevo tipo de entrada en el LAB

En `lab.html`, busca `AÑADIR NUEVOS TEMPLATES AQUÍ` y pega:

```javascript
ENTRY_TEMPLATES.miTipo = {
  label: 'Mi Tipo',
  icon: '🎯',
  formHTML() {
    const d = getCurrentData();
    return `<div class="form-group">
      <label>ID</label>
      <input type="text" id="f-id" value="${selectedEntryId || ''}">
    </div>
    <div class="form-group">
      <label>Mi campo</label>
      <input type="text" id="f-miCampo" value="${d.miCampo || ''}">
    </div>`;
  },
  getData() {
    return { miCampo: document.getElementById('f-miCampo')?.value || '' };
  },
  preview(data) {
    return `<div>${data.miCampo || 'vacío'}</div>`;
  }
};
```

### Nueva opción en el menú desplegable (WEB)

En `index.html`, busca `AÑADIR NUEVAS OPCIONES AQUÍ` y pega:

```html
<div class="panel-item" onclick="miFuncion()">
  <svg viewBox="0 0 24 24"><path d="..."/></svg>
  <span>Mi Opción</span>
</div>
```

### Nueva sección en el reporte del lector

En `lab.html`, busca `AÑADIR NUEVAS SECCIONES DE REPORTE AQUÍ` y pega:

```javascript
report += `── MI SECCIÓN ──\n`;
report += `Dato: ${data.state?.miCampo || 'n/a'}\n\n`;
```

### Nueva tab en el LAB

1. En `#tabs-bar` añade: `<div class="tab" onclick="switchTab('miTab')">Mi Tab</div>`
2. Debajo de los tab-content añade: `<div class="tab-content" id="tab-miTab">...</div>`

### Cambiar estilos globales

- **WEB**: Cambia las variables en `:root` al inicio del CSS
- **LAB**: Cambia las variables en `:root` al inicio del CSS

---

## Subir a GitHub Pages

1. Crea repo en GitHub
2. Sube la carpeta `web/` (index.html + story.json)
3. Settings → Pages → Source: main → /root
4. La URL será: `https://tuusuario.github.io/tu-repo/`

## Flujo de trabajo

1. Abres `lab.html` en tu navegador
2. Creas entradas, defines decisiones
3. Subes con token o descargas story.json y lo subes manual
4. Ella abre la web, juega, exporta JSON
5. Te envía el JSON por WhatsApp
6. Lo cargas en "Leer Export" → ves qué eligió
7. Creas nuevas entradas basándote en sus decisiones
8. Repites
