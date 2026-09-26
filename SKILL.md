---
name: carta-natal
description: >
  Interpretar una carta natal con tránsitos actuales. Usar cuando el usuario pida
  leer su carta natal, analizar planetas en signo y casa, Ascendente, ejes,
  síntesis de la carta, zodíaco chino, o tránsitos del momento (estilo
  Astro-Dienst). Entregable en formato descargable para apps web.
version: 1.0.0
author: ivanirt
license: MIT
metadata:
  tags: [astrology, natal-chart, transits, interpretation, web-app]
  category: divination
  related_skills: [tarot, ifa]
---

# Carta Natal con Tránsitos

Skill para interpretar una carta natal completa cruzada con los tránsitos
planetarios actuales. Diseñada para ser embebida en una app web: el frontend
calcula o recibe las posiciones, y esta skill gobierna la lectura.

## Cuándo usar

- "Lee mi carta natal"
- "¿Qué significan mis tránsitos de este mes?"
- "Analiza mi Ascendente y mis casas"
- "Síntesis de mi carta"
- Cualquier pedido que combine carta natal + tránsitos actuales

## Datos de entrada requeridos

1. **Fecha de nacimiento** (obligatoria)
2. **Hora exacta de nacimiento** (obligatoria para Ascendente y casas; si falta,
   declararlo explícitamente)
3. **Ciudad de nacimiento** (para latitud/longitud y zona horaria)
4. **Fecha de tránsito** (por defecto: hoy)

Nunca inventar posiciones planetarias. Si no hay efemérides o API disponible,
pedir al usuario que pegue las posiciones (por ejemplo exportadas de
Astro-Dienst o astro-seek) o calcularlas con una librería como Kerykeion.

## Estructura de la lectura

### 1. Planetas en signo y casa

Para cada planeta (Sol, Luna, Mercurio, Venus, Marte, Júpiter, Saturno,
Urano, Neptuno, Plutón) reportar:

- Signo y grado
- Casa (sistema Placidus por defecto; mencionar si se usa otro)
- Significado del planeta en ese signo
- Significado del planeta en esa casa
- Dignidad: regencia, exaltación, caída, destierro (mencionar solo
  cuando sea relevante)

### 2. Ascendente y ejes

- Ascendente (ASC) y Descendente (DSC): signo y significado
- Medio Cielo (MC) e Início del Cielo (IC): signo y área de vida
- Eje lunar (Nodo Norte / Nodo Sur): signo, casa y tema kármico

### 3. Aspectos principales

- Conjunción, oposición, trígono, cuadratura, sextil
- Orbe máximo: 8° para mayores, 3° para menores
- Destacar los aspectos que forman patrones (T cuadrado, Gran Trígono,
  Yod) si existen

### 4. Síntesis

- Elemento dominante (fuego, tierra, aire, agua)
- Modalidad dominante (cardinal, fijo, mutable)
- Stelliums o concentraciones
- Tema central de la carta en 2-3 frases

### 5. Zodíaco chino

- Animal del año de nacimiento
- Elemento chino
- Cómo complementa o contrasta con el tropical occidental

### 6. Tránsitos actuales

Para cada planeta en tránsito que haga aspecto a un punto natal
(orbe ≤ 3° para conjunciones/oposiciones, ≤ 2° para el resto):

- Planeta en tránsito, signo y grado
- Punto natal activado (planeta, ASC, MC, Nodo)
- Aspecto y orbe exacto
- Interpretación: qué área de vida se activa y cómo
- Duración aproximada del efecto

Priorizar: tránsitos de Saturno, Júpiter, Urano, Neptuno y Plutón
(efectos de largo plazo), luego Marte y Venus, y por último Mercurio y Luna
(efectos de días).

## Reglas de contenido

1. **Marco como energías y tendencias**, no como certezas ni predicciones
   de eventos concretos (muerte, enfermedad, accidentes).
2. **Incluir disclaimer**: "Entretenimiento simbólico. No sustituye consejo
   profesional."
3. **No suavizar** una conclusión solo para agradar, pero tampoco convertir
   franqueza en crueldad.
4. **Leer el conjunto antes de aislar**: luminares, Ascendente, regentes y
   patrones dominantes pesan más que un planeta aislado.
5. **Diferenciar escuelas** (tropical, sideral, védica) cuando cambie la
   respuesta.

## Formato de salida para web

Entregar la lectura en secciones con encabezados markdown (`##`, `###`)
para que el frontend pueda renderizarla directo. Cada sección debe ser
autocontenida: un usuario que lea solo esa sección entiende el punto.

Ejemplo de encabezado por planeta:

```
### Sol en Tauro, casa 2
[significado signo] [significado casa] [dignidad si aplica]
```

Y por tránsito:

```
### Tránsito: Saturno ⭐ Júpiter natal (orbe 1°24')
[planeta tránsito] [aspecto] [punto natal] [orbe]
[interpretación] [duración]
```

## Limitaciones

- Sin hora de nacimiento: Ascendente, casas y posición lunar quedan
  inciertos. Declararlo, no adivinarlo.
- Sin efemérides: no calcular tránsitos de memoria; pedir datos o usar API.
- Esta skill interpreta; el cálculo astronómico es responsabilidad del
  backend o de la librería que elijas.
