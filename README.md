# Trabajo Práctico – Informática I

Dos proyectos que aplican a la vida real lo visto en clase: **autómatas**, **análisis léxico** y **semántica**.

## Índice

1. [Validador de formularios con autómatas](#1-validador-de-formularios-con-autómatas)
2. [Mini-compilador / validador de expresiones](#2-mini-compilador--validador-de-expresiones)

---

## 1. Validador de formularios con autómatas

### Descripción

Menú con 4 validadores. Cada uno es un **DFA (autómata finito determinista)** definido por una **tabla de transición**, y el programa muestra el **recorrido de estados** paso a paso.

### Validadores

| # | Validador | Regla | Ejemplo válido |
|---|-----------|-------|----------------|
| 1 | Cédula | 6 o 7 dígitos (ignora puntos) | `1.234.567` |
| 2 | Teléfono celular | `09` + 8 dígitos (ignora espacios y guiones) | `0981 123-456` |
| 3 | Email | `local@dominio.ext` | `usuario@correo.com` |
| 4 | Placa | 3 o 4 letras + 3 dígitos | `ABC 123` / `ABCD 123` |

### Cómo funciona

1. Se elige un validador desde el menú.
2. Se ingresa el texto y se **normaliza** (se descartan los caracteres ignorables: puntos, espacios, guiones).
3. Se recorre el DFA símbolo por símbolo consultando la tabla de transición.
4. Se muestra la secuencia de estados (`q0 → q1 → ... → qn`) y si termina en un **estado de aceptación**.

---

## 2. Mini-compilador / validador de expresiones

### Descripción

Lee líneas escritas en un mini lenguaje, las analiza y las evalúa.

### Comandos

| Entrada | Acción |
|---------|--------|
| `var x = 3` | Declara una variable |
| `x = x * 2` | Asigna (solo si `x` ya fue declarada) |
| `x + 8 * y` | Calcula la expresión |
| `tabla` | Muestra la tabla de símbolos |
| `salir` | Termina el programa |

### Fases

#### Fase 1 – Léxico (tokenizar)

Separa el texto en **tokens**:

- número
- identificador
- operador
- paréntesis
- `var`

Si aparece un carácter inválido (por ejemplo `&`) → **error léxico**.

#### Fase 2 – Sintaxis

Funciones del analizador: `expresion`, `termino`, `factor`.

- Respeta la **precedencia**: `*` y `/` antes que `+` y `-`.
- Respeta los **paréntesis**.
- Si falta un `)` → **error sintáctico**.

#### Fase 3 – Semántica

Usa la **tabla de símbolos** (`nombres[]` y `valores[]`) para detectar:

- Variable **no declarada**
- Variable **declarada dos veces**
- **División por cero**

### Ejemplo de sesión

```
> var x = 3
> x = x * 2
> x + 8 * y
Error semántico: variable 'y' no declarada
> var y = 2
> x + 8 * y
22
> x / 0
Error semántico: división por cero
> x & 1
Error léxico: carácter inválido '&'
> tabla
x = 6
y = 2
> salir
```

---

## Conexión con la clase

| Tema visto en clase | Dónde se aplica |
|---------------------|-----------------|
| Autómatas (DFA, tabla de transición) | Proyecto 1: los 4 validadores |
| Ejemplos léxicos | Proyecto 2: el tokenizador |
| Verificador de variables no declaradas | Proyecto 2: tabla de símbolos y chequeos semánticos |

Juntas, las 3 fases (léxico → sintaxis → semántica) forman el **esquema de un compilador real**.