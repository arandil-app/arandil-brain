---
type: reference
title: "Arandil — Identidad Visual y Tono"
description: "Paleta de colores, tipografía, tono de comunicación y guías de diseño"
tags: [design, identidad, visual, paleta, tipografia]
timestamp: 2026-07-04
---

# Arandil — Identidad Visual y Tono

> **⚠️ Mantenido por Perplexity + Rodhan. Claude Code NO modifica este archivo de forma autónoma.**

---

## Paleta de Colores

### Light Mode
```
Primario: #4F6AF5 (azul profundo)
Correcto: #22C55E (verde éxito)
Error: #EF4444 (rojo error)
Advertencia: #F59E0B (amarillo)
Fondo: #F8FAFC (gris muy claro)
Superficie: #FFFFFF (blanco)
Texto primario: #1E293B (gris oscuro)
Texto secundario: #64748B (gris medio)
```

### Dark Mode
```
Primario: #4F6AF5 (mismo azul)
Correcto: #22C55E (mismo verde)
Error: #EF4444 (mismo rojo)
Fondo: #0F172A (azul muy oscuro)
Superficie: #1E293B (gris oscuro)
Texto primario: #F1F5F9 (blanco roto)
Texto secundario: #94A3B8 (gris claro)
```

---

## Tipografía

**Mobile:**
- Principal: System default (San Francisco iOS, Roboto Android)
- Matemáticas: Monospace para números y operadores

**Web/Landing:**
- Headings: Inter Bold
- Body: Inter Regular
- Code/Math: JetBrains Mono

---

## Tono de Comunicación

### Principios
- **Amigable pero no infantil:** Respeto por la inteligencia del usuario
- **Motivador sin presión:** "Sigue así" en lugar de "¡Debes mejorar!"
- **Claro y directo:** Evitar jerga técnica innecesaria
- **Sin culpar:** Nunca "error tuyo", siempre "revisemos esto juntos"

### Ejemplos
**❌ No:**
> "Respuesta incorrecta. Debes repasar álgebra básica."

**✅ Sí:**
> "Este tema puede ser confuso. Veamos paso a paso dónde está el truco."

---

## Iconografía

- Estilo: Outline, no filled (más limpio)
- Librería: Lucide Icons (React Native compatible)
- Tamaño mínimo: 24x24px

---

## Animaciones

- Feedback de respuesta: 300ms (verde/rojo en opción)
- Transiciones de pantalla: 200ms ease-out
- Logros (Rive): 800ms one-shot
- **Regla:** Cero animaciones decorativas durante pregunta activa

---

## Sombras

```css
/* Tarjetas */
box-shadow: 0 1px 3px rgba(0,0,0,0.08);

/* Modales */
box-shadow: 0 4px 12px rgba(0,0,0,0.12);
```

**Regla:** Nunca más profundas que esto (evitar efecto "flotante" exagerado)

---

## Botones

- **Primario:** Fondo `#4F6AF5`, texto blanco
- **Secundario:** Borde `#4F6AF5`, texto `#4F6AF5`, fondo transparente
- **Peligro:** Fondo `#EF4444`, texto blanco
- **Border radius:** 14-16px (redondeado moderno pero no excesivo)
- **Altura mínima:** 48px (touch target)

---

## Reglas de UI

1. **Tamaño mínimo de texto:** 16sp (legibilidad)
2. **Contraste:** Mínimo 4.5:1 (WCAG AA)
3. **Touch targets:** Mínimo 48x48px
4. **Espaciado:** Múltiplos de 8px (8, 16, 24, 32...)

---

**Última actualización:** 2026-07-04  
**Próxima actualización:** Al definir identidad visual completa (logo, brand assets)
