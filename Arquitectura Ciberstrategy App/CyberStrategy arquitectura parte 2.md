
---

# 35. Primera pregunta de un arquitecto: ¿cuál es el problema que estamos resolviendo?

Antes de hablar de Next.js, Drizzle, PostgreSQL, React o carpetas, hay que entender el **problema de negocio**.

En CyberStrategy, simplificando:

> Ayudar a una organización a definir, evaluar, priorizar, ejecutar y medir su estrategia de ciberseguridad.

Eso nos permite identificar capacidades:

```text
Definir
   ↓
Evaluar
   ↓
Priorizar
   ↓
Ejecutar
   ↓
Medir
   ↓
Reevaluar
```

Esto es más importante que las páginas:

```text
/context
/risks
/frameworks
/assessments
/initiatives
/kpis
/dashboard
```

Porque las páginas pueden cambiar.

El negocio no necesariamente cambia.

---

# 36. Una aplicación no es una colección de pantallas

Este es un cambio de mentalidad importante.

Un desarrollador puede pensar:

> "Tengo que construir `/risks`."

Un arquitecto piensa:

> "Tengo que modelar el concepto `Risk` y las operaciones que se pueden realizar sobre él."

Entonces:

```text
Pantalla
   ↓
representa
   ↓
concepto de dominio
```

La pantalla es una consecuencia.

---

# 37. ¿Qué es realmente un Risk?

Aquí empieza el diseño de dominio.

Un riesgo tiene:

```text
Risk
 ├── code
 ├── category
 ├── probability
 ├── impact
 ├── treatment
 ├── owner
 ├── reviewDate
 └── residualProbability
     residualImpact
```

Pero además tiene **comportamiento**.

Por ejemplo:

```text
calculateInherentLevel()
calculateResidualLevel()
isOverdue()
canClose()
```

Esta es una idea importante:

> **Un objeto de dominio no es solamente datos. También representa reglas y comportamiento.**

En un sistema pequeño, eso puede implementarse mediante funciones de dominio en lugar de clases.

No hay que caer en:

```text
class Risk
```

solo porque estamos hablando de dominio.

---

# 38. Datos vs comportamiento

Supongamos:

```typescript id="f9n2z1"
const risk = {
  probability: 4,
  impact: 5
}
```

Y después:

```typescript id="a5xj5j"
const level = calculateRiskLevel(
  risk.probability,
  risk.impact
)
```

Aquí el comportamiento está separado.

Otra posibilidad sería:

```typescript id="h5p8h6"
risk.calculateLevel()
```

Ambas pueden ser válidas.

La arquitectura no consiste en elegir automáticamente una.

La pregunta es:

> ¿Qué forma hace más claro el dominio y más fácil de mantener?

---

# 39. El concepto de Aggregate

Aquí aparece otro concepto de DDD que te será útil.

Un **Aggregate** es un conjunto de objetos que deben mantenerse consistentes como una unidad.

Por ejemplo, podríamos pensar:

```text
Assessment
   │
   ├── AssessmentControl
   ├── AssessmentControl
   ├── AssessmentControl
   └── ...
```

El `Assessment` podría ser la raíz.

Entonces:

```text
Assessment
```

controla operaciones como:

```text
start()
close()
reopen()
```

y no queremos que cualquier parte del sistema haga arbitrariamente:

```text
assessment.status = "Closed"
```

Esto ayuda a mantener invariantes.

---

# 40. ¿Qué es un invariant?

Una condición que debe mantenerse siempre verdadera.

Por ejemplo:

```text
Assessment cerrado
→ no debería poder modificarse un control
```

Otro:

```text
Initiative completed
→ progress debería ser 100%
```

Otro:

```text
KPI measurement
→ debe pertenecer al KPI correspondiente
```

Otro:

```text
Risk probability
→ debe estar entre 1 y 5
```

La arquitectura debe ayudar a que esas reglas sean difíciles de violar.

---

# 41. Aquí aparece el concepto de "invariante de negocio"

Esto es mucho más importante que tener una clase bonita.

Por ejemplo:

```text
Assessment = Closed
```

no es solamente un valor.

Significa:

> El conjunto de controles evaluados ya no debería modificarse.

Entonces la regla debe vivir en un lugar que controle esa operación.

---

# 42. ¿Por qué no confiar solamente en la UI?

Porque esto:

```text
if (assessment.closed) {
  disableButton()
}
```

solo protege al usuario normal.

No protege:

* Server Action;
* API;
* scripts;
* otro componente;
* Claude generando código;
* futuros clientes.

La verdadera regla debe estar en servidor/dominio.

Por eso tu decisión actual:

> "Los permisos se validan en el servidor"

es arquitectónicamente correcta.

---

# 43. Authentication ≠ Authorization

Otra distinción que quiero que tengas clara.

### Authentication

> ¿Quién eres?

```text
Diego
```

### Authorization

> ¿Qué puedes hacer?

```text
CISO
→ puede cerrar assessment

Responsable
→ puede actualizar controles

Lector
→ puede visualizar
```

Son problemas diferentes.

Arquitectónicamente:

```text
Authentication
      ↓
Identity
      ↓
Authorization
      ↓
Business operation
```

---

# 44. Y después aparece una tercera cosa: Audit

Supongamos que alguien cambia:

```text
Risk
5 × 5 → 3 × 4
```

Puedes querer saber:

```text
Quién
Cuándo
Qué cambió
Valor anterior
Valor nuevo
```

Eso es **auditoría**.

No es lo mismo que autorización.

Tenemos:

```text
Authentication → quién eres
Authorization  → qué puedes hacer
Audit          → qué hiciste
```

Son tres preocupaciones distintas.

---

# 45. ¿Necesitas audit log ahora?

Probablemente sí conviene considerarlo para una aplicación de ciberseguridad, pero no necesariamente construir un sistema gigantesco.

Una tabla sencilla podría registrar:

```text
AuditLog
 ├── userId
 ├── action
 ├── entity
 ├── entityId
 ├── timestamp
 └── metadata
```

Pero nuevamente:

**primero entender el requerimiento.**

Si el producto apunta a organizaciones donde la trazabilidad es importante, probablemente tendrá bastante valor.

---

# 46. Ahora pensemos en transacciones

Imagina:

> Crear una iniciativa y asociarla a tres riesgos.

Tienes:

```text
INSERT initiative
INSERT initiative_risk
INSERT initiative_risk
INSERT initiative_risk
```

¿Qué pasa si:

```text
INSERT #1 → OK
INSERT #2 → OK
INSERT #3 → ERROR
```

Podrías quedar con datos parcialmente creados.

Entonces necesitas una **transacción**.

Conceptualmente:

```text
BEGIN

crear iniciativa
crear relaciones
crear auditoría

COMMIT
```

Si algo falla:

```text
ROLLBACK
```

Esto es arquitectura de datos.

---

# 47. ACID

PostgreSQL te proporciona garantías transaccionales importantes:

**A — Atomicity**

Todo o nada.

**C — Consistency**

El sistema pasa de un estado válido a otro válido.

**I — Isolation**

Operaciones concurrentes no deberían interferirse incorrectamente.

**D — Durability**

Una vez confirmado, el cambio persiste.

No necesitas convertirte en experto en bases de datos para diseñar CyberStrategy, pero entender esto te ayuda a decidir **qué operaciones deben ser atómicas**.

---

# 48. El modelo relacional también es arquitectura

Por ejemplo:

```text
Initiative
    │
    ├── initiative_risk
    │          ↓
    │        Risk
    │
    ├── initiative_control
    │          ↓
    │       Control
    │
    └── initiative_objective
               ↓
         SecurityObjective
```

Eso es mucho mejor que:

```text
Initiative
 ├── riskIds = "1,2,5"
 ├── controlIds = "3,4,7"
 └── objectiveIds = "2,8"
```

Porque las relaciones N:N deben representarse adecuadamente.

---

# 49. Foreign Keys no son burocracia

Una FK:

```text
initiative_risk.risk_id
       ↓
risk.id
```

garantiza integridad.

Sin ella puedes terminar con:

```text
initiative → risk 9999
```

cuando ese riesgo ya no existe.

La base de datos debe proteger invariantes que le corresponden.

Esto lleva a una idea importante:

> **La integridad no debería depender exclusivamente de la aplicación.**

---

# 50. Validación en dos niveles

Por ejemplo:

```text
probability = 7
```

La aplicación debería rechazarlo.

Pero también conviene que la base de datos tenga constraints cuando tenga sentido.

Tenemos:

```text
UI validation
      ↓
Application validation
      ↓
Database constraints
```

Cada capa protege algo distinto.

No significa duplicar todas las reglas tres veces.

---

# 51. ¿Dónde debe vivir cada regla?

Esta pregunta es fundamental.

### Regla de presentación

> Mostrar "5 — Crítico" en rojo.

UI.

### Regla de negocio

> 5 × 5 produce determinado nivel.

Domain/application.

### Regla de integridad

> probability debe estar entre 1 y 5.

Database + application.

### Regla de autorización

> Responsable solo puede modificar sus controles.

Authorization/server.

Aprender a clasificar reglas así es una habilidad arquitectónica muy valiosa.

---

# 52. Ahora llegamos a Dependency Injection

No necesitas un framework de DI para entender el concepto.

Supongamos:

```text
InitiativeService
```

necesita enviar notificaciones.

Mala arquitectura:

```typescript id="gqj5h3"
import { sendEmail } from "gmail-library"
```

La lógica queda acoplada directamente a Gmail.

Mejor conceptualmente:

```text
InitiativeService
       ↓
Notification interface
       ↓
EmailNotification
```

Entonces mañana puedes tener:

```text
Email
Teams
Slack
Webhook
```

Pero otra vez:

> **No crees la abstracción hasta que exista una razón real para necesitarla.**

---

# 53. Esto es Dependency Inversion

La lógica importante dice:

> "Necesito notificar."

No debería tener que decir:

> "Necesito Gmail."

Eso es una diferencia fundamental.

---

# 54. ¿Por qué un monolito modular es ideal para aprender?

Porque puedes observar todas estas cosas sin la complejidad de un sistema distribuido.

Puedes aprender:

* dominio;
* módulos;
* APIs;
* persistencia;
* transacciones;
* autorización;
* testing;
* observabilidad.

sin tener además:

```text
network partitions
distributed transactions
service discovery
message brokers
eventual consistency
```

Es un excelente laboratorio arquitectónico.

---

# 55. Y aquí viene una de las trampas más comunes

La gente aprende arquitectura así:

```text
SOLID
↓
Clean Architecture
↓
DDD
↓
Microservices
```

y termina creyendo:

> "Mientras más capas y servicios tenga, mejor arquitectura."

Es exactamente al revés.

Una arquitectura madura es aquella donde puedes explicar:

> **por qué cada frontera existe.**

---

# 56. La pregunta arquitectónica correcta

En vez de:

> "¿Debo crear un Repository?"

pregunta:

> "¿Qué problema resuelve un Repository aquí?"

En vez de:

> "¿Debo usar CQRS?"

pregunta:

> "¿Tengo diferentes necesidades de lectura/escritura que justifican separar modelos?"

En vez de:

> "¿Debo usar microservicios?"

pregunta:

> "¿Tengo límites organizacionales, escalabilidad o independencia de despliegue que lo justifiquen?"

Ese cambio de pensamiento es enorme.

---

# 57. Arquitectura evolutiva

No tienes que descubrir la arquitectura perfecta hoy.

Puedes tener:

```text
v1
Monolith
   ↓
Modular Monolith
   ↓
más usuarios
   ↓
separar un módulo
   ↓
eventos
   ↓
servicio independiente
```

Si alguna vez ocurre.

La arquitectura debe poder **evolucionar**.

No necesitas anticipar todo.

---

# 58. La arquitectura como gestión de riesgos

Esto es especialmente interesante para ti.

Una decisión arquitectónica tiene:

```text
Beneficio
Costo
Riesgo
```

Por ejemplo:

### Repository abstraction

Beneficio:

```text
↓ acoplamiento
```

Costo:

```text
↑ código
↑ abstracciones
```

Riesgo si no lo haces:

```text
medio
```

Entonces quizás:

> No hacerlo todavía.

---

### Microservicios

Beneficio potencial:

```text
independencia de despliegue
escalabilidad independiente
```

Costo:

```text
enorme
```

Riesgo actual:

```text
bajo
```

Entonces:

> No hacerlo.

Eso es arquitectura racional.

---

# 59. ¿Qué quiero que empieces a hacer cuando Claude programe?

Cada vez que Claude proponga algo, pregúntale:

> **¿Qué problema arquitectónico resuelve esto?**

Después:

> **¿Qué principio estamos aplicando?**

Después:

> **¿Qué complejidad agregamos?**

Después:

> **¿Qué alternativa más simple existe?**

Y finalmente:

> **¿Cómo lo probamos?**

Eso te va a hacer progresar muchísimo.

---

# 60. Aplicado a tu próximo cambio

Imaginemos:

> "RQ-118: generar roadmap desde priorización."

Antes de dejar que Claude programe, tú deberías poder pensar:

```text
Prioritization
      ↓
Initiatives
      ↓
Roadmap
```

Y preguntarte:

### Dominio

¿Qué significa "priorizar"?

### Datos

¿Qué se persiste?

### Cálculo

¿Qué se deriva?

### Estado

¿Una iniciativa puede pasar automáticamente al roadmap?

### Seguridad

¿Quién puede hacer esto?

### Consistencia

¿Qué pasa si una iniciativa ya está en el roadmap?

### Histórico

¿La priorización cambia entre ciclos?

### Testing

¿Qué escenarios debemos probar?

Eso **es hacer arquitectura antes de programar**.

---

## El siguiente nivel de aprendizaje

Cuando Claude te entregue el material que le pedimos, yo haría algo distinto a una simple revisión.

Tomaría **una operación real de CyberStrategy**, por ejemplo:

> **"Completar una iniciativa."**

Y la recorreríamos desde el navegador hasta PostgreSQL:

```text
Usuario
  ↓
React
  ↓
Server Action
  ↓
Authorization
  ↓
Use Case
  ↓
Domain rules
  ↓
Transaction
  ↓
Drizzle
  ↓
PostgreSQL
  ↓
Result
  ↓
UI
```

Y en **cada flecha** te explicaría:

* qué responsabilidad existe;
* qué principio arquitectónico aplica;
* qué acoplamiento existe;
* qué alternativa hay;
* qué ganamos;
* qué costo introducimos;
* cómo se testea;
* y **por qué no conviene complicarlo más**.

Después hacemos lo mismo con `Risk`, `Assessment`, `Initiative` y `Dashboard`.

Ese ejercicio te enseñaría bastante más arquitectura que leer 20 patrones de diseño aislados.
