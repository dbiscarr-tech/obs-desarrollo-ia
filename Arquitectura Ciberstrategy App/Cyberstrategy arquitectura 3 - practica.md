
# 8. Trazando una operación completa: «Completar una iniciativa»

Supongamos que en CyberStrategy un usuario Responsable entra a una iniciativa y cambia:

> `In Execution → Completed`

A primera vista parece trivial. Pero arquitectónicamente es una operación interesante porque atraviesa prácticamente todas las capas.

El flujo conceptual sería:

```text
Usuario
   │
   ▼
UI / React
   │
   ▼
Server Action
   │
   ├──► Autorización
   │
   ▼
Use Case
   │
   ├──► Regla de negocio
   │
   ├──► Validaciones
   │
   ▼
Transacción
   │
   ├──► Initiative
   ├──► relaciones
   └──► Audit Log (si existe)
   │
   ▼
PostgreSQL / Drizzle
   │
   ▼
Resultado
   │
   ▼
UI actualizada
```

La pregunta importante no es **"¿cómo hago el update?"**.

Es:

> **¿Dónde debe vivir cada decisión?**

---

# 9. Primera frontera: UI ≠ negocio

Imagina que Claude genera esto:

```typescript
async function completeInitiative(id: string) {
  await db
    .update(initiatives)
    .set({ status: "COMPLETED" })
    .where(eq(initiatives.id, id))
}
```

Funciona.

Pero hay un problema arquitectónico:

**¿quién decidió que esa iniciativa puede pasar a `COMPLETED`?**

La UI no debería decidirlo.

Tampoco debería decidirlo directamente la base de datos.

Y tampoco queremos que cada página tenga su propia versión de esa regla.

Por eso aparece una primera idea fundamental:

> **Una operación de negocio debe tener un lugar reconocible donde vive su significado.**

Por ejemplo:

```typescript
completeInitiative(initiative)
```

es mucho más expresivo que:

```typescript
updateInitiative({ status: "COMPLETED" })
```

La segunda es una modificación de datos.

La primera representa una **acción del dominio**.

---

# 10. Estado no es lo mismo que transición

Esta distinción es muy importante.

Tenemos:

```text
PLANNED
IN_EXECUTION
COMPLETED
PAUSED
CANCELLED
```

Una implementación ingenua permitiría:

```typescript
updateInitiative({
  status: "COMPLETED"
})
```

desde cualquier estado.

Entonces podrías terminar con:

```text
CANCELLED → COMPLETED
```

o:

```text
COMPLETED → IN_EXECUTION
```

sin que nadie lo haya pensado.

En cambio, si modelamos la transición:

```typescript
completeInitiative(initiative)
```

podemos decir:

```text
PLANNED ───────► IN_EXECUTION ───────► COMPLETED
                    │
                    ▼
                  PAUSED
```

Y definir explícitamente cuáles transiciones son válidas.

Por ejemplo:

```typescript
function completeInitiative(initiative) {
  if (initiative.status !== "IN_EXECUTION") {
    throw new Error(
      "Only initiatives in execution can be completed"
    )
  }

  return {
    ...initiative,
    status: "COMPLETED",
  }
}
```

Ahora existe una **regla de negocio explícita**.

---

# 11. ¿Por qué no poner esa regla en React?

Podrías hacer:

```typescript
if (initiative.status === "IN_EXECUTION") {
  showCompleteButton()
}
```

Perfecto para la interfaz.

Pero eso **no es seguridad ni integridad de negocio**.

Un usuario podría:

* llamar directamente al Server Action;
* manipular una petición;
* utilizar otra interfaz;
* ejecutar otra operación que modifique la iniciativa.

Por eso:

```text
UI
 │
 └── "te muestro el botón"
```

mientras que:

```text
Domain
 │
 └── "determino si la operación es válida"
```

Son responsabilidades diferentes.

Esta separación es uno de los conceptos más importantes de arquitectura.

---

# 12. Y aquí aparece autorización

Supongamos que tenemos:

```text
Admin
CISO
Responsible
Reader
```

Un Responsable puede actualizar una iniciativa.

Un Reader no.

Entonces hay **dos preguntas distintas**:

### Pregunta 1

> ¿Este usuario tiene permiso?

```typescript
can(user, "initiative.complete")
```

### Pregunta 2

> ¿La iniciativa puede completarse?

```typescript
canCompleteInitiative(initiative)
```

Son cosas diferentes.

Podríamos tener:

```text
                    ┌──────────────────────┐
Usuario ───────────►│ ¿Tiene autorización? │
                    └──────────┬───────────┘
                               │ sí
                               ▼
                    ┌──────────────────────┐
                    │ ¿Transición válida? │
                    └──────────┬───────────┘
                               │ sí
                               ▼
                         COMPLETAR
```

Esto es:

**Authorization ≠ Business Rule**

No conviene mezclarlos.

---

# 13. Ahora aparece el Use Case

Aquí entra un concepto que probablemente encontrarás muchísimo cuando estudies arquitectura:

> **Use Case / Application Service**

Por ejemplo:

```typescript
completeInitiative(userId, initiativeId)
```

Ese caso de uso puede coordinar:

1. obtener usuario;
2. verificar autorización;
3. obtener iniciativa;
4. verificar transición;
5. modificarla;
6. registrar auditoría;
7. devolver resultado.

Conceptualmente:

```typescript
async function completeInitiative(
  userId,
  initiativeId
) {
  const user = await getUser(userId)

  authorize(user, "initiative.complete")

  const initiative =
    await getInitiative(initiativeId)

  const completed =
    completeInitiativeDomain(initiative)

  await saveInitiative(completed)

  await audit(...)

  return completed
}
```

Observa algo interesante.

El Use Case **coordina**.

No necesariamente contiene toda la lógica.

---

# 14. Una regla para recordar

Puedes pensar así:

### UI

> "¿Qué le muestro al usuario?"

### Application / Use Case

> "¿Qué operación necesita ejecutarse?"

### Domain

> "¿Qué significa que esta operación sea válida?"

### Infrastructure

> "¿Cómo guardo o recupero la información?"

Esta separación es tremendamente útil.

---

# 15. ¿Dónde debería estar Drizzle?

Aquí aparece otra decisión.

No queremos que una regla como:

```typescript
if (status !== "IN_EXECUTION")
```

dependa de Drizzle.

Idealmente:

```text
Domain
   │
   │ no sabe
   ▼
Drizzle
```

El dominio debería poder entender:

```typescript
initiative.status
```

sin saber si los datos vienen de:

* PostgreSQL;
* Drizzle;
* memoria;
* una API;
* un test.

Eso es **desacoplamiento**.

Pero atención:

## Esto NO significa que debamos crear 40 interfaces.

Un error muy común es:

```text
InitiativeRepository
InitiativeRepositoryInterface
InitiativeRepositoryImpl
PostgresInitiativeRepository
DrizzleInitiativeRepository
```

para una aplicación pequeña.

Eso puede ser más arquitectura que problema.

Aquí aparece nuevamente:

> **KISS + YAGNI**

Si no existe una necesidad concreta de sustituir Drizzle/Postgres, probablemente no necesitas una abstracción artificial.

---

# 16. Entonces, ¿cómo lo haría en CyberStrategy?

Para tu proyecto, yo pensaría inicialmente en algo así:

```text
app/
  initiatives/
    page.tsx

    actions.ts

domain/
  initiatives/
    initiative.ts
    rules.ts

application/
  initiatives/
    complete-initiative.ts

lib/
  db/
    schema.ts
    index.ts
```

No porque esta estructura sea "la correcta universal".

Sino porque hace visibles las responsabilidades.

Por ejemplo:

```text
app/
    interfaz y entrada

application/
    casos de uso

domain/
    reglas de negocio

lib/db/
    persistencia
```

Y eso es mucho más importante que el nombre exacto de las carpetas.

---

# 17. ¿Qué pasa con la transacción?

Supongamos que completar una iniciativa hace tres cosas:

```text
1. actualizar initiative
2. actualizar relación
3. crear audit log
```

Tenemos un problema.

¿Qué pasa si ocurre esto?

```text
1. UPDATE initiative       ✓
2. UPDATE relation        ✓
3. INSERT audit log       ✗
```

Ahora tenemos un estado parcialmente actualizado.

Por eso necesitamos una transacción:

```text
BEGIN

UPDATE initiative
UPDATE relation
INSERT audit

COMMIT
```

Si algo falla:

```text
ROLLBACK
```

Y volvemos al estado anterior.

Aquí aparece **ACID**, pero no necesitas memorizarlo como teoría académica.

Para tu aprendizaje basta inicialmente con:

> **Una operación que conceptualmente debe ocurrir completa, debería ejecutarse atómicamente.**

---

# 18. ¿Y el dashboard?

Aquí aparece otro concepto muy importante.

Después de completar la iniciativa, el dashboard puede mostrar:

```text
Iniciativas completadas: 24
```

Pero **no debería tener una segunda implementación de la regla**.

Por ejemplo, sería mala idea que el dashboard diga:

```typescript
if (
  initiative.status === "COMPLETED" &&
  initiative.progress === 100 &&
  initiative.endDate < today
) {
   ...
}
```

mientras otra pantalla utiliza otra definición.

Esto genera el clásico problema:

> **"La pantalla A dice una cosa y el dashboard otra."**

El dashboard debería consumir el estado y cálculos definidos por el modelo de dominio/query.

---

# 19. Una distinción todavía más importante

Volvamos a CyberStrategy.

Cuando completamos una iniciativa:

```text
Initiative = COMPLETED
```

¿significa automáticamente:

```text
Control = mejoró
```

No.

Y tampoco:

```text
Risk = disminuyó
```

Esta es una decisión arquitectónica excelente que ya detectamos en tu modelo.

Porque estamos hablando de **tres hechos diferentes**:

```text
Ejecución
   │
   └── iniciativa completada

Resultado
   │
   └── control efectivamente mejorado

Impacto
   │
   └── riesgo residual modificado
```

Podrían estar relacionados, pero no son equivalentes.

---

# 20. Ejemplo

Supongamos:

> Iniciativa: implementar MFA.

La iniciativa pasa a:

```text
COMPLETED
```

Pero después descubrimos que:

* solo el 80% de los usuarios tiene MFA;
* existen cuentas técnicas sin MFA;
* algunos sistemas legacy no lo soportan.

Entonces:

```text
Initiative
COMPLETED
       │
       ├──── Control
       │      mejora parcial
       │
       └──── Risk
              puede no cambiar
```

Esto demuestra algo fundamental:

> **Un estado operativo no necesariamente demuestra un resultado de negocio.**

Por eso tu decisión actual de **no actualizar automáticamente madurez o riesgo al cerrar iniciativas** es coherente con un modelo que quiere distinguir ejecución de impacto.

---

# 21. Esto nos lleva a un concepto mayor: hechos vs interpretaciones

En CyberStrategy podríamos tener:

### Hechos

```text
Initiative.status = COMPLETED
Assessment.controlLevel = 3
Risk.residualProbability = 2
```

### Cálculos

```text
riskLevel = probability × impact
gap = target - current
kpiStatus = calculate(...)
```

### Interpretaciones

```text
"El riesgo mejoró"
"El programa está funcionando"
"La estrategia está alineada"
```

Las dos primeras categorías pueden ser modeladas directamente.

La tercera requiere contexto y puede depender de múltiples indicadores.

Esto es particularmente importante cuando construyas el dashboard ejecutivo.

No quieres que el dashboard transforme automáticamente unos pocos números en conclusiones que el modelo no puede justificar.

---

# 22. La arquitectura empieza a aparecer

Si juntamos todo:

```text
                    ┌───────────────┐
                    │     React     │
                    │      UI       │
                    └───────┬───────┘
                            │
                            ▼
                    ┌───────────────┐
                    │ Server Action │
                    └───────┬───────┘
                            │
                            ▼
                    ┌───────────────┐
                    │   Use Case    │
                    │               │
                    │ autorización  │
                    │ coordinación  │
                    └───────┬───────┘
                            │
                            ▼
                    ┌───────────────┐
                    │    Domain     │
                    │               │
                    │ reglas        │
                    │ invariantes   │
                    │ transiciones  │
                    └───────┬───────┘
                            │
                            ▼
                    ┌───────────────┐
                    │ Drizzle / DB  │
                    └───────────────┘
```

Y ahora puedes evaluar cualquier código que Claude produzca preguntándote:

> **¿En qué caja debería estar esto?**

Esa pregunta vale muchísimo más que memorizar patrones de arquitectura.

---

# 23. El ejercicio para ti

Ahora te propongo algo que va a acelerar mucho tu aprendizaje.

En tu código real de CyberStrategy, busca cómo está implementado actualmente:

> **"Completar una iniciativa".**

No cambies nada todavía.

Pídele a Claude algo como:

```text
Quiero usar esta operación para aprender arquitectura.

Analiza cómo está implementado actualmente el caso de uso
"Completar una iniciativa".

No modifiques código.

Muéstrame el recorrido real desde:

Usuario
→ UI
→ Server Component/Client Component
→ Server Action/API
→ autorización
→ lógica de negocio
→ acceso a datos
→ Drizzle
→ base de datos
→ resultado
→ actualización de UI.

Para cada paso indica:

1. archivo
2. función
3. responsabilidad
4. qué decisión de negocio toma
5. dependencias
6. si está acoplado a otra capa
7. qué principio arquitectónico aplica
8. qué problema existiría si esta responsabilidad estuviera en otra capa.

Después identifica:

- reglas de negocio duplicadas
- reglas en la UI que deberían estar en servidor
- lógica de negocio dentro de queries
- lógica de persistencia dentro del dominio
- validaciones duplicadas
- problemas de autorización
- posibles transacciones
- oportunidades de simplificación

No propongas refactor todavía.

Primero quiero entender la arquitectura actual.
```

Ese último punto es deliberado:

> **Primero entender. Después juzgar. Finalmente cambiar.**

Es exactamente el enfoque que quiero que desarrolles como arquitecto.

Y cuando tengas la respuesta de Claude, tráemela aquí. Podemos hacer una **revisión arquitectónica línea por línea**, pero orientada a que entiendas el razonamiento detrás de cada decisión, no simplemente a decirte qué código cambiar.
