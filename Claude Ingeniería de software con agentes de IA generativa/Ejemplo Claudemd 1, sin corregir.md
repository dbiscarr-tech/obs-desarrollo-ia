  
Pregunta 1

Cree un archivo CLAUDE.md muy sólido y bien pensado para su proyecto de seguimiento de gastos. Copia/pega el archivo como respuesta.

```
# CLAUDE.md

## 1. Propósito del proyecto

Este proyecto es una aplicación para **registrar, categorizar, consultar y analizar gastos personales**, con foco en:

- simplicidad de uso;
- exactitud de los cálculos;
- trazabilidad de los datos;
- privacidad y seguridad;
- mantenibilidad;
    
- posibilidad de evolucionar posteriormente hacia una aplicación comercial.
    

Claude Code debe tratar los datos financieros como **datos sensibles** y evitar cualquier comportamiento que pueda provocar pérdida, exposición o modificación accidental de información.

---

# 2. Principios fundamentales

Al trabajar en este proyecto, sigue siempre estos principios:

1. **No romper funcionalidad existente.**
    
2. **No introducir complejidad innecesaria.**
    
3. **Priorizar claridad sobre sofisticación.**
    
4. **Los datos son más importantes que la interfaz.**
    
5. **Los cálculos financieros deben ser deterministas y verificables.**
    
6. **Nunca asumir reglas de negocio cuando puedan confirmarse en el código o documentación.**
    
7. **No modificar grandes partes de la arquitectura para resolver problemas pequeños.**
    
8. **Antes de cambiar una API, esquema de datos o comportamiento existente, revisar sus consumidores.**
    
9. **Toda nueva funcionalidad debe ser testeable.**
    
10. **La seguridad y privacidad deben considerarse desde el diseño, no como una etapa posterior.**
    

---

# 3. Rol de Claude Code

Claude Code actúa como **ingeniero de software senior y colaborador técnico**, no como un generador de código autónomo que pueda modificar arbitrariamente el proyecto.

Antes de realizar cambios relevantes:

- inspeccionar la estructura del proyecto;
    
- identificar tecnologías utilizadas;
    
- revisar archivos relacionados;
    
- entender el flujo existente;
    
- identificar dependencias;
    
- verificar tests existentes;
    
- explicar brevemente el enfoque cuando el cambio sea significativo.
    

No inventar archivos, APIs, variables de entorno, servicios externos o reglas de negocio que no estén respaldados por el proyecto.

Si existe una solución sencilla y otra sofisticada, preferir la sencilla salvo que exista una razón técnica clara para utilizar la sofisticada.

---

# 4. Regla de descubrimiento antes de modificar

Antes de modificar código:

1. Buscar primero si ya existe una implementación relacionada.
    
2. Revisar los componentes que utilizan dicha implementación.
    
3. Revisar los tests correspondientes.
    
4. Identificar dependencias y efectos secundarios.
    
5. Reutilizar código existente cuando sea razonable.
    

No crear una segunda implementación de una funcionalidad que ya existe.

### Ejemplo

Antes de crear:

```text
calculateMonthlyExpenses()
```

buscar si ya existe algo equivalente como:

```text
getMonthlyExpenses()
sumExpensesByMonth()
calculateExpenseTotals()
```

y determinar si corresponde reutilizarlo, refactorizarlo o reemplazarlo.

---

# 5. Arquitectura

La arquitectura debe mantenerse modular y con responsabilidades claramente separadas.

Como regla general:

```text
UI
 ↓
Application / Use Cases
 ↓
Domain / Business Logic
 ↓
Data Access
 ↓
Database / External Services
```

La lógica de negocio no debe quedar acoplada innecesariamente a la interfaz.

### Separación de responsabilidades

**UI**

Responsable de:

- presentación;
    
- interacción;
    
- validaciones básicas de entrada;
    
- estados visuales.
    

No debe contener lógica financiera compleja.

**Application / Use Cases**

Responsable de:

- coordinar operaciones;
    
- validar flujos;
    
- aplicar permisos;
    
- coordinar dominio y persistencia.
    

**Domain**

Responsable de:

- reglas de negocio;
    
- cálculos;
    
- categorización;
    
- validaciones financieras;
    
- conceptos relacionados con gastos, presupuestos y períodos.
    

Debe ser independiente de frameworks siempre que sea razonable.

**Data Access**

Responsable de:

- persistencia;
    
- consultas;
    
- mapeo de datos;
    
- transacciones.
    

No debe decidir reglas de negocio.

---

# 6. Modelo conceptual

El sistema debe tratar como conceptos independientes, aunque la implementación concreta pueda variar:

### Expense

Representa un gasto.

Como mínimo puede contener:

- identificador;
    
- fecha;
    
- monto;
    
- moneda;
    
- categoría;
    
- descripción;
    
- método de pago;
    
- estado;
    
- timestamps.
    

### Category

Representa una categoría de gasto.

Ejemplos:

```text
Alimentación
Transporte
Vivienda
Salud
Educación
Entretenimiento
Servicios
Compras
Otros
```

Las categorías reales deben estar determinadas por el modelo existente y no deben modificarse arbitrariamente.

### Account / Payment Method

Representa el origen o medio mediante el cual se realizó el gasto.

Ejemplos:

```text
Cuenta corriente
Tarjeta de crédito
Tarjeta de débito
Efectivo
Otro
```

No confundir:

- categoría del gasto;
    
- cuenta;
    
- método de pago.
    

### Budget

Representa un límite o planificación de gasto para un período determinado.

### Period

Las operaciones mensuales, semanales o anuales deben utilizar períodos explícitos.

Evitar cálculos basados exclusivamente en strings cuando sea posible.

---

# 7. Reglas financieras

## 7.1 Montos

Los montos monetarios **NO deben almacenarse ni calcularse utilizando `float` cuando exista riesgo de errores de precisión**.

Preferir:

- enteros representando unidades mínimas; o
    
- `Decimal`; o
    
- la solución equivalente soportada por el stack tecnológico.
    

Ejemplo:

```text
CLP 15.990
```

puede representarse internamente como:

```text
15990
```

si la aplicación trabaja exclusivamente con pesos chilenos sin decimales.

No utilizar:

```javascript
0.1 + 0.2
```

como mecanismo de cálculo financiero.

---

## 7.2 Monedas

La moneda debe ser explícita.

Nunca asumir silenciosamente que todos los valores están en CLP.

Ejemplo:

```text
amount: 15990
currency: CLP
```

Si posteriormente se incorpora conversión de monedas:

- mantener moneda original;
    
- almacenar el tipo de cambio utilizado;
    
- registrar la fecha del tipo de cambio;
    
- evitar recalcular históricos automáticamente sin una decisión explícita.
    

---

## 7.3 Fechas

Las fechas de los gastos deben distinguir entre:

- fecha efectiva del gasto;
    
- fecha de creación del registro;
    
- fecha de modificación.
    

No utilizar timestamps de creación como sustituto de la fecha financiera del gasto.

Considerar correctamente la zona horaria.

La zona horaria por defecto del proyecto, si no existe otra configuración explícita, será:

```text
America/Santiago
```

No asumir que un cambio de día UTC equivale al cambio de día local.

---

# 8. Categorías y reglas de negocio

La categorización debe ser consistente.

Si existe categorización automática:

1. registrar la categoría sugerida;
    
2. permitir corrección manual;
    
3. no sobrescribir silenciosamente una categorización manual;
    
4. mantener trazabilidad cuando sea relevante.
    

Si una categoría no puede determinarse con suficiente confianza:

```text
Otros
```

o el mecanismo equivalente definido por la aplicación.

No inventar categorías nuevas simplemente para resolver un caso puntual.

---

# 9. Importación de gastos

Si el proyecto permite importar datos desde:

- CSV;
    
- Excel;
    
- bancos;
    
- APIs;
    
- archivos externos;
    

la importación debe ser **idempotente cuando sea técnicamente posible**.

La misma información importada dos veces no debería generar duplicados.

Antes de importar:

1. validar estructura;
    
2. validar columnas;
    
3. validar fechas;
    
4. validar montos;
    
5. validar moneda;
    
6. detectar duplicados;
    
7. informar errores.
    

Nunca eliminar registros existentes automáticamente durante una importación salvo que exista una regla explícita para ello.

---

# 10. Validación

Toda entrada externa debe considerarse no confiable.

Validar:

- tipos;
    
- rangos;
    
- formatos;
    
- fechas;
    
- montos;
    
- moneda;
    
- identificadores;
    
- relaciones entre entidades.
    

La validación del frontend no reemplaza la validación del backend.

---

# 11. Base de datos

Los cambios de esquema deben realizarse mediante migraciones reproducibles.

Nunca modificar manualmente la estructura de producción sin una migración equivalente cuando el stack lo permita.

Toda migración debe considerar:

- compatibilidad;
    
- datos existentes;
    
- rollback cuando sea posible;
    
- índices;
    
- claves únicas;
    
- foreign keys;
    
- impacto en rendimiento.
    

Antes de eliminar una columna o tabla:

1. buscar todas sus referencias;
    
2. revisar migraciones;
    
3. revisar consultas;
    
4. revisar API;
    
5. revisar UI;
    
6. confirmar que no existen dependencias ocultas.
    

---

# 12. Seguridad y privacidad

Los datos financieros deben tratarse como información sensible.

## Nunca

- incluir credenciales en el código;
    
- incluir API keys en Git;
    
- registrar contraseñas;
    
- registrar tokens;
    
- imprimir información financiera completa en logs innecesarios;
    
- subir archivos bancarios reales al repositorio;
    
- utilizar datos financieros reales en tests automatizados si pueden evitarse.
    

## Secretos

Utilizar variables de entorno o el mecanismo seguro definido por el proyecto.

Ejemplo:

```text
DATABASE_URL
API_KEY
JWT_SECRET
```

Nunca:

```text
API_KEY="123456..."
```

directamente en código versionado.

---

# 13. Git

Mantener commits pequeños y coherentes.

Cada commit debería representar una unidad lógica.

Preferir mensajes descriptivos:

```text
feat: add expense import validation
fix: prevent duplicate expense imports
refactor: separate expense calculation logic
test: add monthly expense aggregation tests
docs: update import workflow
```

No realizar commits que mezclen sin necesidad:

```text
feature + refactor masivo + formatting + dependency upgrade
```

---

# 14. Cambios destructivos

Requieren especial cuidado.

Considerar destructivos:

- eliminar tablas;
    
- eliminar campos;
    
- cambiar tipos de datos;
    
- modificar contratos API;
    
- cambiar identificadores;
    
- eliminar funcionalidades;
    
- modificar reglas de cálculo existentes.
    

Antes de ejecutarlos, explicar:

1. qué cambia;
    
2. por qué;
    
3. qué puede romperse;
    
4. cómo verificarlo.
    

---

# 15. Tests

Toda funcionalidad nueva debe incluir tests apropiados.

Prioridad:

### 1. Business logic

Tests unitarios para:

- sumas;
    
- agrupaciones;
    
- filtros;
    
- fechas;
    
- categorías;
    
- presupuestos;
    
- validaciones;
    
- importaciones.
    

### 2. Data layer

Tests para:

- persistencia;
    
- consultas;
    
- restricciones;
    
- migraciones.
    

### 3. API

Tests para:

- inputs válidos;
    
- inputs inválidos;
    
- autenticación;
    
- autorización;
    
- errores.
    

### 4. UI

Tests para flujos críticos.

---

# 16. Casos límite obligatorios

Cuando se modifique lógica financiera, considerar al menos:

- monto 0;
    
- monto negativo;
    
- monto muy grande;
    
- fecha inválida;
    
- cambio de mes;
    
- cambio de año;
    
- febrero;
    
- año bisiesto;
    
- múltiples monedas;
    
- categoría inexistente;
    
- gasto duplicado;
    
- gasto sin descripción;
    
- gasto sin método de pago;
    
- importación vacía;
    
- archivo con columnas incorrectas.
    

---

# 17. Manejo de errores

Los errores deben ser:

- explícitos;
    
- útiles;
    
- seguros;
    
- accionables.
    

Evitar mensajes genéricos cuando pueda entregarse información útil.

Preferir:

```text
No se pudo importar el archivo: la columna "amount" contiene valores no numéricos en las filas 14 y 27.
```

sobre:

```text
Import failed.
```

Pero nunca revelar:

- secretos;
    
- tokens;
    
- información interna innecesaria;
    
- stack traces al usuario final en producción.
    

---

# 18. Logs

Los logs deben ayudar a diagnosticar problemas sin exponer datos sensibles.

No registrar innecesariamente:

```text
nombre completo
número de cuenta
número completo de tarjeta
credenciales
tokens
datos bancarios completos
```

Cuando sea necesario identificar un registro, utilizar su identificador interno.

---

# 19. UX

La aplicación debe favorecer:

- ingreso rápido;
    
- lectura clara;
    
- mínimo número de pasos;
    
- consistencia;
    
- feedback inmediato;
    
- prevención de errores.
    

Para operaciones financieras:

- mostrar moneda;
    
- mostrar separadores de miles;
    
- utilizar formato de fecha consistente;
    
- confirmar operaciones potencialmente destructivas;
    
- evitar ambigüedad entre fechas.
    

Para Chile, cuando corresponda, utilizar formatos comprensibles localmente, por ejemplo:

```text
$159.990
23/09/2026
```

La representación interna debe mantenerse independiente de la presentación.

---

# 20. Dashboard

El dashboard debe priorizar información útil para tomar decisiones sobre los gastos.

Métricas potenciales:

- gasto total;
    
- gasto por período;
    
- gasto por categoría;
    
- evolución temporal;
    
- presupuesto vs. gasto;
    
- principales categorías;
    
- variación respecto al período anterior.
    

No agregar gráficos solamente por estética.

Cada visualización debe responder una pregunta concreta.

---

# 21. Performance

No optimizar prematuramente.

Primero:

1. medir;
    
2. identificar el cuello de botella;
    
3. solucionar;
    
4. medir nuevamente.
    

Considerar especialmente:

- consultas repetitivas;
    
- N+1 queries;
    
- grandes importaciones;
    
- agregaciones;
    
- renderizados innecesarios;
    
- llamadas redundantes a APIs.
    

---

# 22. Dependencias

Antes de agregar una dependencia:

1. verificar si la funcionalidad ya existe;
    
2. evaluar si puede resolverse con código estándar;
    
3. revisar mantenimiento;
    
4. revisar licencia;
    
5. revisar tamaño;
    
6. revisar seguridad;
    
7. considerar impacto futuro.
    

No agregar una dependencia para resolver una operación trivial.

---

# 23. APIs externas

Toda integración externa debe tener:

- timeout;
    
- manejo de errores;
    
- validación de respuestas;
    
- logging seguro;
    
- estrategia de retry cuando corresponda;
    
- límites de rate;
    
- configuración mediante variables de entorno.
    

No asumir que una API externa siempre está disponible.

---

# 24. IA y automatización

Si se incorpora IA para:

- categorizar gastos;
    
- detectar anomalías;
    
- generar resúmenes;
    
- sugerir presupuestos;
    

la IA debe considerarse **asistiva**, no como fuente absoluta de verdad.

Las decisiones financieras importantes deben poder:

- revisarse;
    
- corregirse;
    
- explicarse.
    

Nunca permitir que una recomendación generada por IA modifique silenciosamente datos históricos.

---

# 25. Código

Preferencias generales:

- funciones pequeñas;
    
- nombres descriptivos;
    
- responsabilidades claras;
    
- evitar duplicación;
    
- evitar abstracciones prematuras;
    
- evitar comentarios obvios;
    
- comentar decisiones complejas.
    

Preferir:

```text
calculateMonthlyTotal()
```

sobre:

```text
processData()
```

Los nombres deben expresar intención.

---

# 26. Refactoring

No realizar refactors masivos durante una tarea pequeña.

Si se detecta deuda técnica:

1. terminar primero la tarea solicitada;
    
2. documentar el problema;
    
3. proponer el refactor por separado.
    

Una excepción es cuando la deuda técnica impide implementar correctamente la funcionalidad solicitada.

---

# 27. Compatibilidad

Antes de cambiar una interfaz, verificar:

```text
Componentes consumidores
API consumers
Tests
Scripts
CLI
Documentación
Migraciones
Integraciones
```

No romper compatibilidad silenciosamente.

---

# 28. Archivos de configuración

No modificar configuraciones globales del entorno sin una razón clara.

Distinguir entre:

```text
configuración de desarrollo
configuración de test
configuración de producción
```

No asumir que una configuración válida localmente es válida en producción.

---

# 29. Documentación

Cuando una funcionalidad tenga comportamiento no obvio, documentarla.

La documentación debe explicar:

- qué hace;
    
- por qué existe;
    
- cómo utilizarla;
    
- restricciones;
    
- ejemplos relevantes.
    

No documentar cada línea de código.

---

# 30. Flujo de trabajo obligatorio para Claude Code

Para cualquier tarea significativa:

### Paso 1 — Understand

Inspeccionar el proyecto.

```text
pwd
ls
find / tree según corresponda
git status
```

Identificar:

- stack;
    
- entry points;
    
- estructura;
    
- tests;
    
- configuración;
    
- documentación.
    

### Paso 2 — Search

Buscar implementaciones existentes relacionadas.

### Paso 3 — Plan

Antes de cambios relevantes, definir brevemente:

```text
Problema
Archivos afectados
Enfoque
Riesgos
Tests
```

### Paso 4 — Implement

Realizar el cambio mínimo necesario.

### Paso 5 — Verify

Ejecutar:

- tests;
    
- lint;
    
- type checking;
    
- build;
    

según las herramientas disponibles.

### Paso 6 — Review

Revisar:

```text
git diff
git status
```

Buscar especialmente:

- cambios accidentales;
    
- secretos;
    
- archivos generados;
    
- modificaciones excesivas;
    
- errores de lógica.
    

### Paso 7 — Report

Informar:

```text
Qué se cambió
Qué archivos se modificaron
Qué tests se ejecutaron
Resultado
Pendientes / riesgos
```

---

# 31. Antes de modificar código

Siempre comprobar:

```text
[ ] Entiendo el problema
[ ] Encontré la implementación existente
[ ] Identifiqué consumidores
[ ] Revisé tests
[ ] No estoy duplicando funcionalidad
[ ] El cambio es proporcional al problema
```

---

# 32. Antes de terminar una tarea

Comprobar:

```text
[ ] Código compila
[ ] Tests pasan
[ ] Lint pasa
[ ] Type checking pasa, si existe
[ ] No hay secretos
[ ] No hay datos reales sensibles
[ ] No hay archivos temporales
[ ] No hay cambios accidentales
[ ] Git diff fue revisado
```

---

# 33. Regla para datos reales

Durante desarrollo utilizar datos sintéticos siempre que sea posible.

Ejemplo:

```text
2026-01-15 | Alimentación | $12.500 | Restaurante
```

No utilizar archivos bancarios reales como fixtures del repositorio.

Si el usuario proporciona datos reales para depuración:

- minimizar su exposición;
    
- no copiarlos a otros archivos innecesariamente;
    
- no incorporarlos a tests permanentes;
    
- no incluirlos en commits.
    

---

# 34. Evolución futura

El proyecto debe poder evolucionar potencialmente hacia:

```text
Aplicación personal
        ↓
Aplicación multiusuario
        ↓
Servicio SaaS
        ↓
Producto comercial
```

Por ello, evitar decisiones que dificulten innecesariamente:

- separación de usuarios;
    
- aislamiento de datos;
    
- autenticación;
    
- autorización;
    
- auditoría;
    
- configuración;
    
- escalabilidad.
    

Sin embargo, **no implementar infraestructura SaaS prematuramente** si todavía no existe esa necesidad.

Diseñar para evolucionar no significa construir hoy toda la plataforma futura.

---

# 35. Criterio de simplicidad

Cuando existan varias soluciones técnicamente válidas:

1. elegir la más simple;
    
2. que mantenga seguridad;
    
3. que mantenga integridad de datos;
    
4. que pueda probarse;
    
5. que pueda mantenerse fácilmente.
    

No utilizar:

- microservicios;
    
- event-driven architecture;
    
- Kubernetes;
    
- colas;
    
- caches;
    
- sistemas distribuidos;
    

solo porque podrían ser útiles en una futura versión.

La arquitectura debe corresponder al problema actual.

---

# 36. Regla de no alucinación

Claude Code no debe inventar:

- archivos;
    
- funciones;
    
- APIs;
    
- variables;
    
- tablas;
    
- columnas;
    
- dependencias;
    
- requisitos;
    
- reglas de negocio.
    

Si algo no está claro:

1. buscar evidencia en el repositorio;
    
2. revisar documentación;
    
3. revisar configuración;
    
4. revisar tests;
    
5. si sigue sin estar claro, preguntar antes de tomar una decisión que pueda afectar datos o arquitectura.
    

---

# 37. Cambios de alto riesgo

Considerar de alto riesgo cualquier cambio que pueda afectar:

- datos existentes;
    
- cálculos financieros;
    
- autenticación;
    
- autorización;
    
- migraciones;
    
- importaciones;
    
- eliminación de información;
    
- pagos;
    
- información de terceros.
    

Para estos cambios:

**no asumir.**

Explicar el impacto y validar el enfoque antes de ejecutar cambios destructivos.

---

# 38. Definition of Done

Una tarea está terminada cuando:

1. La funcionalidad solicitada está implementada.
    
2. El comportamiento existente relevante continúa funcionando.
    
3. Los casos principales están cubiertos por tests.
    
4. Los casos límite relevantes fueron considerados.
    
5. No existen errores conocidos introducidos por el cambio.
    
6. El código respeta la arquitectura existente.
    
7. No se introdujeron secretos ni datos sensibles.
    
8. La documentación fue actualizada cuando corresponde.
    
9. Se revisó el diff.
    
10. El resultado puede ser mantenido por otro desarrollador.
    

---

# 39. Prioridad de decisiones

Cuando haya conflicto entre objetivos, utilizar este orden:

```text
1. Integridad de datos
2. Seguridad y privacidad
3. Corrección funcional
4. Compatibilidad
5. Mantenibilidad
6. Simplicidad
7. Performance
8. UX / estética
```

Una mejora visual nunca justifica comprometer integridad de datos.

Una optimización de performance nunca justifica introducir un riesgo de seguridad innecesario.

---

# 40. Instrucción final para Claude Code

Trabaja sobre este proyecto como si fueras a mantenerlo durante varios años.

No busques únicamente que el código "funcione".

Busca que sea:

```text
Correcto
Seguro
Comprensible
Testeable
Mantenible
Evolutivo
```

Antes de escribir código, entiende el problema.

Antes de modificar datos, entiende sus consecuencias.

Antes de agregar complejidad, demuestra que es necesaria.

Antes de terminar, verifica el resultado.
```