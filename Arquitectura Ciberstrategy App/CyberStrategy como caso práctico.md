

---

# 1. Primero: ¿qué es realmente arquitectura de software?

Una forma sencilla de entenderla es:

> **La arquitectura define las decisiones estructurales que determinan cómo se organiza un sistema, cómo se relacionan sus partes y qué tan fácil será cambiarlo.**

No es solamente:

* carpetas;
* frameworks;
* patrones;
* diagramas;
* clases;
* microservicios.

Una arquitectura responde preguntas como:

> ¿Dónde vive esta regla?

> ¿Quién puede modificar este dato?

> ¿Qué parte conoce a PostgreSQL?

> ¿Qué ocurre si mañana cambiamos PostgreSQL?

> ¿Qué pasa si cambia una regla del negocio?

> ¿Cómo evitamos que dos módulos hagan el mismo cálculo de manera diferente?

> ¿Cómo agregamos una funcionalidad sin romper cinco existentes?

Eso es arquitectura.

---

# 2. El problema fundamental: gestionar la complejidad

Todo software empieza aparentemente sencillo.

Por ejemplo:

```text
Riesgos
 ├── crear
 ├── editar
 ├── eliminar
 └── listar
```

Entonces parece suficiente:

```text
React
   ↓
Server Action
   ↓
Drizzle
   ↓
PostgreSQL
```

Perfecto.

Pero CyberStrategy empieza a crecer:

```text
Riesgos
Evaluaciones
Controles
Marcos
Iniciativas
KPIs
Objetivos
Roadmap
Usuarios
Permisos
Dashboard
```

Y aparecen relaciones:

```text
Risk ←→ Initiative
Risk ←→ Assessment
Control ←→ Assessment
Control ←→ Initiative
Initiative ←→ Objective
KPI ←→ Initiative
```

Y reglas:

```text
risk = probability × impact

gap = target - current

KPI status = measurement vs target

initiative overdue = dueDate < today

assessment:
Draft → In Progress → Closed
```

Ahí empieza la arquitectura.

---

# 3. Primera gran idea: separar responsabilidades

Uno de los principios más importantes es **Separation of Concerns**.

La idea es sencilla:

> Una parte del sistema debería tener una responsabilidad clara.

Por ejemplo, no queremos esto:

```text
RiskForm.tsx

 ├── dibuja formulario
 ├── valida datos
 ├── calcula riesgo
 ├── verifica permisos
 ├── ejecuta SQL
 ├── registra auditoría
 └── manda notificación
```

Funciona.

Pero imagina que mañana cambia la fórmula del riesgo.

Tendrías que buscar dónde está implementada.

Peor aún: podría estar implementada en:

```text
RiskForm
Dashboard
RiskAction
Report
```

Y cada una podría hacer algo ligeramente diferente.

---

# 4. Una arquitectura más saludable

Podríamos tener:

```text
UI
 ↓
Application
 ↓
Domain
 ↓
Infrastructure
```

Por ejemplo:

```text
RiskForm
    ↓
createRisk()
    ↓
Risk rules
    ↓
Risk repository
    ↓
Database
```

Cada nivel tiene una responsabilidad.

### UI

Se preocupa de:

> ¿Cómo se muestra?

### Application

Se preocupa de:

> ¿Qué operación quiere realizar el usuario?

### Domain

Se preocupa de:

> ¿Qué significa que esa operación sea válida?

### Infrastructure

Se preocupa de:

> ¿Cómo guardamos los datos?

---

# 5. ¿Qué ganamos con esto?

Supongamos que mañana cambias:

```text
PostgreSQL
```

por otra tecnología.

Si tienes:

```text
UI
 ↓
Business logic
 ↓
Repository
 ↓
PostgreSQL
```

el impacto está concentrado.

Si tienes:

```text
UI
 ↓
Drizzle
 ↓
PostgreSQL
```

por todas partes, el cambio es mucho más costoso.

Esto se llama **reducir acoplamiento**.

---

# 6. Acoplamiento: uno de los conceptos más importantes

Dos componentes están acoplados cuando uno depende fuertemente de cómo funciona el otro.

Por ejemplo:

```typescript
const risks = await db
  .select()
  .from(risksTable)
  .where(...)
```

dentro de un componente.

Ese componente conoce:

* Drizzle;
* SQL;
* estructura de la tabla;
* filtros;
* base de datos.

Está muy acoplado.

Una alternativa:

```typescript
const risks = await getRisks(...)
```

y:

```text
getRisks()
   ↓
RiskRepository
   ↓
Drizzle
```

reduce el acoplamiento.

---

# 7. Pero cuidado: bajo acoplamiento no significa "poner interfaces para todo"

Este es un error muy común al aprender arquitectura.

Alguien descubre Dependency Inversion y crea:

```text
IRiskRepository
RiskRepository
RiskRepositoryFactory
RiskRepositoryProvider
RiskRepositoryAdapter
```

para una aplicación que tiene 20 usuarios.

Eso no necesariamente es buena arquitectura.

La pregunta correcta es:

> **¿Qué cambio futuro estoy aislando?**

Si no existe una razón concreta, probablemente estás agregando complejidad innecesaria.

Aquí entran:

**KISS**

> Keep It Simple.

**YAGNI**

> You Aren't Gonna Need It.

---

# 8. SOLID explicado con CyberStrategy

Aquí tienes un excelente campo de entrenamiento.

## S — Single Responsibility

Una cosa debería tener una responsabilidad clara.

No:

```text
DashboardService
```

que:

* consulta DB;
* calcula riesgos;
* calcula KPIs;
* verifica permisos;
* formatea HTML.

Mejor separar responsabilidades.

---

## O — Open/Closed

El sistema debería permitir extender comportamiento sin tener que modificar constantemente código existente.

Ejemplo:

```text
RiskCalculator
```

Si mañana agregas otra metodología de scoring, podrías extender el mecanismo.

Pero **no necesitas diseñar eso hoy** si solamente existe una metodología.

---

## L — Liskov Substitution

Es más relevante cuando tienes jerarquías o abstracciones.

Por ejemplo:

```text
Framework
 ├── NIST
 ├── ISO
 └── CIS
```

No significa automáticamente que debas crear:

```text
INistFramework
IIsoFramework
ICisFramework
```

El principio es importante, pero no necesariamente exige esa implementación.

---

## I — Interface Segregation

No crear interfaces gigantes.

Por ejemplo:

```text
UserService
```

con 30 operaciones puede ser una señal.

Mejor interfaces pequeñas cuando realmente sean necesarias.

---

## D — Dependency Inversion

Esencial para entender arquitectura.

La idea:

> Las reglas importantes del negocio no deberían depender directamente de detalles técnicos.

Conceptualmente:

```text
Business logic
      ↓
abstraction
      ↓
database implementation
```

en lugar de:

```text
Business logic
      ↓
Drizzle
      ↓
PostgreSQL
```

Pero nuevamente:

**no significa que debas crear una abstracción para cada tabla.**

---

# 9. Dominio: probablemente el concepto más importante para CyberStrategy

Arquitectura moderna no significa empezar por las tablas.

Primero debes entender:

> **¿Cuál es el dominio del sistema?**

En tu caso:

```text
Security Strategy Management
```

Y sus conceptos:

```text
Business Objective
Security Objective
Risk
Framework
Control
Assessment
Initiative
KPI
Roadmap
Cycle
```

Esto es el **modelo de dominio**.

---

# 10. Entidad vs. atributo vs. cálculo

Este es un ejercicio arquitectónico muy bueno.

Pregunta:

> ¿La brecha es una entidad o es un cálculo?

Probablemente:

```text
Current = 2
Target = 5

Gap = 3
```

Por tanto:

```text
Gap
```

puede ser un concepto del dominio, pero **no necesariamente una entidad persistente**.

Esto es importante porque:

> No todo concepto del negocio debe convertirse en una tabla.

---

# 11. Otro ejemplo: "atrasada"

Una iniciativa:

```text
dueDate = 2026-10-01
status = In Execution
```

Hoy:

```text
2026-10-05
```

Entonces:

```text
overdue = true
```

¿Necesitas guardar?

```text
isOverdue = true
```

Probablemente no.

Es un **dato derivado**.

Esto evita inconsistencias.

---

# 12. Fuente única de verdad

Otro concepto fundamental.

Si tienes:

```text
probability
impact
riskLevel
```

y `riskLevel` deriva de los otros dos, tienes dos posibilidades.

### Malo

Guardar todo:

```text
probability = 4
impact = 5
riskLevel = Critical
```

y permitir modificar cualquiera.

### Mejor

Guardar:

```text
probability = 4
impact = 5
```

y calcular:

```text
riskLevel = calculateRisk(4,5)
```

Entonces existe una sola fuente de verdad.

---

# 13. Esto también explica por qué me preocupa el histórico

Supongamos:

```text
Assessment 2026
Control A = 2
```

Luego:

```text
Assessment 2027
Control A = 4
```

Si modificas:

```text
Control.currentLevel
```

perdiste el pasado.

Por eso una evaluación debería contener una fotografía histórica:

```text
Assessment 2026
 └── AssessmentControl
       current = 2

Assessment 2027
 └── AssessmentControl
       current = 4
```

Esto es un concepto arquitectónico:

> **Temporalidad / historical state**

---

# 14. ¿Por qué propuse un "Cycle"?

Porque estás empezando a necesitar una unidad temporal de negocio.

Algo como:

```text
Strategy Cycle
     │
     ├── Assessment
     ├── Risk review
     ├── Initiatives
     └── KPI review
```

Eso permite posteriormente:

```text
Compare Cycle 2026 vs 2027
```

Y aquí aparece algo interesante:

**RQ-105 "Comparar ciclos" no debería ser una funcionalidad aislada.**

Debe apoyarse en un concepto de dominio que realmente represente el ciclo.

Eso es arquitectura: **hacer que el modelo represente correctamente el negocio.**

---

# 15. Estado vs. evento

Otro aprendizaje importante.

Tienes:

```text
Initiative = Completed
```

Eso es **estado**.

Pero también puede interesarte:

```text
2026-09-01 → Started
2026-09-15 → Paused
2026-09-20 → Resumed
2026-10-01 → Completed
```

Eso son **eventos/historial**.

No necesariamente necesitas Event Sourcing.

Pero sí debes reconocer la diferencia.

```text
Current state
      ≠
History of changes
```

---

# 16. Por qué NO recomiendo Event Sourcing ahora

Porque aprender arquitectura también significa aprender cuándo **no** usar una tecnología.

Event Sourcing podría representar:

```text
InitiativeCreated
InitiativeStarted
InitiativePaused
InitiativeResumed
InitiativeCompleted
```

y reconstruir:

```text
Current State
```

Es interesante.

Pero agrega mucha complejidad.

Para CyberStrategy hoy:

```text
CRUD + audit/history
```

probablemente es suficiente.

Esto es un ejemplo perfecto de:

> **La arquitectura correcta depende del problema.**

---

# 17. Monolito modular

Para tu aplicación, esta es una arquitectura que quiero que aprendas muy bien.

No:

```text
Microservice Risk
Microservice Assessment
Microservice KPI
Microservice Dashboard
```

sino:

```text
             CyberStrategy
                  │
        ┌─────────┼─────────┐
        │         │         │
      Risks   Assessments  KPIs
        │         │         │
        └─────────┼─────────┘
                  │
              PostgreSQL
```

Todo está en una aplicación.

Pero internamente existen **módulos con límites claros**.

Eso es un:

> **Modular Monolith**

---

# 18. ¿Por qué es tan interesante?

Porque obtienes muchas ventajas:

* despliegue simple;
* debugging sencillo;
* transacciones fáciles;
* menor infraestructura;
* desarrollo rápido;
* menor latencia entre módulos;
* menos DevOps;
* más fácil para Claude.

Y puedes evolucionar posteriormente si realmente aparece una razón.

---

# 19. La regla de oro del monolito modular

Que los módulos no se conviertan en:

```text
todos llaman a todos
```

Porque entonces tienes:

```text
Risks ↔ Assessments ↔ KPIs ↔ Initiatives ↔ Dashboard
       ↕           ↕          ↕
       todo conoce todo
```

Eso produce **acoplamiento global**.

Idealmente:

```text
Module A
    ↓
public API
    ↓
Module B
```

y no:

```text
Module A
    ↓
internals of B
```

---

# 20. Bounded Contexts

Aquí entramos en un concepto de DDD.

No necesariamente necesitas implementarlo formalmente, pero entenderlo es muy útil.

Podrías pensar en:

```text
Strategic Context
 ├── Business Objectives
 └── Security Objectives

Risk Context
 └── Risks

Assessment Context
 ├── Framework
 ├── Controls
 └── Assessments

Execution Context
 ├── Initiatives
 └── Roadmap

Measurement Context
 └── KPIs

Identity Context
 └── Users / Roles
```

Eso te ayuda a entender **qué pertenece a qué**.

---

# 21. ¿Por qué esto es importante para Claude?

Porque Claude tiende naturalmente a modificar lo que tiene cerca.

Si encuentra:

```text
app/risks/page.tsx
```

y ahí está todo:

```text
UI
SQL
validaciones
cálculos
autorización
```

Claude puede modificar todo ese archivo para resolver un cambio.

En cambio, si tienes límites:

```text
features/risks/
    domain/
    application/
    infrastructure/
    ui/
```

Claude tiene más contexto estructural.

No elimina los errores de IA, pero **reduce el espacio donde puede introducirlos**.

---

# 22. Esto conecta directamente con tu metodología OpenSpec

Aquí hay una conexión muy interesante con lo que estás haciendo.

OpenSpec define:

> qué cambio queremos hacer.

La arquitectura define:

> dónde debe vivir ese cambio.

Por ejemplo:

```text
RQ-112
"Generar iniciativas desde brechas"
```

Una IA mal guiada podría:

```text
AssessmentPage
    ↓
crear iniciativa directamente
```

Una arquitectura más limpia podría hacer:

```text
Assessment
   ↓
Gap calculation
   ↓
GenerateInitiatives use case
   ↓
Initiative
```

Entonces:

**OpenSpec controla el cambio.**

**Arquitectura controla dónde y cómo se implementa.**

---

# 23. Testing también es arquitectura

No pienses en testing solamente como:

> "probar que el botón funciona".

Las reglas críticas deberían poder probarse independientemente de React.

Por ejemplo:

```typescript
calculateRisk(5, 5)
```

debería poder probarse sin:

* navegador;
* Next.js;
* PostgreSQL;
* UI.

Eso produce tests rápidos y confiables.

---

# 24. La pirámide de testing

Para tu proyecto:

```text
             E2E
            /   \
           /     \
      Integration
        /       \
       /         \
      Unit tests
```

Muchos:

```text
Unit
```

menos:

```text
Integration
```

y pocos:

```text
E2E
```

Por ejemplo:

### Unit

```text
calculateRisk()
calculateGap()
calculateKpiStatus()
canTransition()
validateCascade()
```

### Integration

```text
createRisk()
completeInitiative()
closeAssessment()
```

### E2E

```text
CISO crea iniciativa
Responsable la completa
Dashboard la muestra
```

---

# 25. La gran ventaja: cambiar con confianza

Esta es probablemente la mayor ganancia de una buena arquitectura.

Supongamos que mañana dices:

> Cambiemos la fórmula de priorización.

Si tienes:

```text
Priority calculation
       ↓
unit tests
```

puedes modificarla con bastante seguridad.

Si está mezclada entre:

```text
React
SQL
Server Action
Dashboard
```

el cambio se vuelve mucho más riesgoso.

---

# 26. Arquitectura = costo de cambio

Una forma muy buena de pensar arquitectura es:

> **¿Cuánto cuesta cambiar algo?**

Por ejemplo:

### Mala arquitectura

Cambiar:

```text
KPI threshold
```

requiere modificar:

```text
KPI page
Dashboard
Reports
API
Tests
```

### Mejor arquitectura

```text
KpiStatusCalculator
```

cambia en un único lugar.

Entonces:

> **La arquitectura reduce el costo de cambio.**

Este es uno de los conceptos que más te recomiendo recordar.

---

# 27. Cohesión

Otro concepto fundamental.

**Alta cohesión** significa que las cosas que están juntas tienen razones relacionadas para estar juntas.

Por ejemplo:

```text
features/kpis/
```

contiene cosas relacionadas con KPI.

Eso es mejor que:

```text
components/
services/
utils/
helpers/
```

donde terminamos con cientos de archivos sin relación evidente.

---

# 28. Acoplamiento vs cohesión

Esta combinación es clásica:

> **Alta cohesión + bajo acoplamiento**

Idealmente:

```text
        ┌───────────┐
        │   Risks   │
        └─────┬─────┘
              │
        public interface
              │
        ┌─────▼─────┐
        │Assessment │
        └───────────┘
```

Cada módulo conoce lo que necesita y poco más.

---

# 29. ¿Y dónde entra DRY?

**Don't Repeat Yourself.**

Pero hay una sutileza muy importante.

DRY no significa:

> "Todo código parecido debe convertirse en una abstracción."

Si tienes:

```text
RiskForm
InitiativeForm
```

ambos tienen:

```text
<label>
<input>
```

no necesariamente necesitas crear:

```text
UniversalBusinessFormEngine
```

DRY busca evitar **duplicación de conocimiento**, no simplemente duplicación textual.

Esto es una distinción arquitectónica muy importante.

---

# 30. YAGNI es igualmente importante

Supongamos que piensas:

> "Quizás algún día tendremos múltiples bases de datos."

No construyas ahora:

```text
DatabaseProvider
PostgresProvider
MongoProvider
OracleProvider
```

si solamente tienes PostgreSQL.

YAGNI dice:

> **No implementes una necesidad hipotética.**

Cuando aparezca la necesidad, diseñas para ella.

---

# 31. KISS tampoco significa código mediocre

KISS no significa:

> "hazlo rápido y sin arquitectura".

Significa:

> **elige la solución más sencilla que resuelva correctamente el problema.**

Por ejemplo:

```text
Next.js
+
modular monolith
+
Drizzle
+
PostgreSQL
+
Server Actions
```

puede ser perfectamente una arquitectura profesional.

No necesitas Kubernetes para demostrarlo.

---

# 32. Y finalmente: YAGNI + SOLID + KISS juntos

Los tres pueden parecer contradictorios:

```text
SOLID → abstraer
KISS  → simplificar
YAGNI → no anticipar
```

Pero realmente se complementan:

> **Diseña bien lo que existe, mantén simples las soluciones y no diseñes problemas que todavía no existen.**

Esa frase resume bastante bien cómo abordaría CyberStrategy.

---

# 33. La arquitectura que yo enseñaría usando tu proyecto

Si quieres realmente aprender, yo estudiaría CyberStrategy en este orden:

```text
1. Domain modeling
        ↓
2. Separation of concerns
        ↓
3. Cohesion / coupling
        ↓
4. Application vs domain logic
        ↓
5. Dependency inversion
        ↓
6. Modular monolith
        ↓
7. Persistence / data modeling
        ↓
8. State machines
        ↓
9. Historical data / temporal modeling
        ↓
10. Authorization
        ↓
11. Testing architecture
        ↓
12. Observability
        ↓
13. Scalability
```

Y **después**, si realmente aparece la necesidad:

```text
DDD avanzado
CQRS
Event-driven architecture
Event sourcing
Microservices
Distributed systems
```

---

# 34. Lo más importante que quiero que aprendas

No quiero que termines sabiendo solamente:

> "Tengo que crear una carpeta `domain`."

Quiero que puedas mirar una funcionalidad y preguntarte:

### 1. ¿Qué concepto del negocio estoy modelando?

### 2. ¿Quién es responsable de esa decisión?

### 3. ¿Es estado o es un cálculo?

### 4. ¿Es persistente o derivado?

### 5. ¿Quién puede modificarlo?

### 6. ¿Qué otras partes dependen de él?

### 7. ¿Qué pasa si cambia mañana?

### 8. ¿Dónde debería vivir esa regla?

### 9. ¿Cómo la pruebo sin depender de toda la aplicación?

### 10. ¿Estoy resolviendo un problema real o anticipando uno?

Si empiezas a pensar así, **estás haciendo arquitectura**.

---

## Y CyberStrategy es un excelente proyecto para aprenderla

Porque tienes casi todos los ingredientes:

```text
                  ┌───────────────┐
                  │ Business      │
                  │ Domain        │
                  └───────┬───────┘
                          │
          ┌───────────────┼───────────────┐
          ▼               ▼               ▼
       Risks          Controls         Objectives
          │               │               │
          └───────────────┼───────────────┘
                          ▼
                      Assessment
                          │
                          ▼
                         Gap
                          │
                          ▼
                      Initiative
                          │
                          ▼
                       Roadmap
                          │
                          ▼
                        KPI
                          │
                          ▼
                      Dashboard
```

Y puedes aprender **arquitectura de verdad** haciendo evolucionar el sistema, en lugar de estudiar patrones aisladamente.

### Mi recomendación para tu aprendizaje

Cuando Claude te entregue el material que le pedimos, **no pasemos inmediatamente a "arreglarlo"**.

Primero hagamos una especie de *architecture walkthrough* de CyberStrategy:

1. **Mapa de arquitectura actual**
2. **Identificar módulos y responsabilidades**
3. **Seguir una operación completa**, por ejemplo `crear riesgo`
4. **Identificar dónde está cada regla**
5. **Detectar acoplamientos**
6. **Compararlo con SOLID/KISS/YAGNI**
7. **Entender qué está bien y por qué**
8. **Entender qué cambiaríamos y por qué**
9. **Solo después modificar el código**

Así cada cambio que hagamos te servirá para aprender **el principio arquitectónico detrás de la decisión**, que creo que es mucho más valioso para ti que simplemente conseguir que Claude produzca una arquitectura "bonita".



