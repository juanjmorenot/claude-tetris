---
name: clima
description: Obtiene el clima actual de una ciudad usando wttr.in por curl, sin API key. Úsala cuando el usuario pida el clima, temperatura o pronóstico de una ubicación.
---

# Clima

Trae el clima vía `curl` a `wttr.in` (gratis, sin API key).

## Uso

```bash
curl -s "wttr.in/<CIUDAD>?format=3"
```

Ejemplo (reporte corto, una línea):
```bash
curl -s "wttr.in/Bogota?format=3"
```

Reporte detallado (3 días, sin colores ANSI):
```bash
curl -s "wttr.in/Bogota?T"
```

Si el usuario no da ciudad, usa por defecto Naguanagua, Edo. Carabobo, Venezuela:
```bash
curl -s "wttr.in/Naguanagua,Venezuela?format=3"
```

Responde al usuario solo con el dato relevante (temperatura/condición), sin pegar el output crudo salvo que lo pida.
