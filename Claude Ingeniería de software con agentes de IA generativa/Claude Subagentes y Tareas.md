# Claude Subagentes y Tareas

Estado: Traducido automáticamente del Inglés

Traducido automáticamente del Inglés

Los subagentes son una de las características más potentes de Claude Code para manejar proyectos complejos y grandes bases de código. Le permiten ejecutar múltiples instancias especializadas de Claude en paralelo, cada una con su propio contexto y área de enfoque.

## ¿Qué son los subagentes?

Los subagentes son instancias ligeras de Claude Code que se ejecutan en una tarea a través de la Herramienta de Tareas. Cuando un subagente se está ejecutando, se puede ver que la salida dice "Task(Performing task X)".

Las principales ventajas de los subagentes son

- **Ventanas de contexto independientes**: Cada subagente mantiene su propio contexto, evitando el desbordamiento de información
    
- **Ejecución paralela**: Múltiples subagentes pueden trabajar simultáneamente en diferentes aspectos de su proyecto
    
- **Enfoque especializado**: A cada subagente se le puede asignar un dominio o tarea específicos en los que concentrarse
    

## Uso básico de subagentes

### Ejecución de dos subagentes especializados

He aquí un ejemplo práctico de ejecución de dos subagentes para analizar diferentes partes de su aplicación:

```
Can you run two subagents? 
One of them should analyze improvements to the back-end that could be made. 
One of them should analyze improvements that could be made to the front end.
Please run them in parallel. 
```

Cuando ejecute este comando, Claude Code creará dos tareas paralelas:

Cada subagente

- Se centrará exclusivamente en su dominio asignado (backend vs frontend)
    
- Utilizará su propia ventana contextual para profundizar en los archivos relevantes
    
- Proporcionará recomendaciones especializadas basadas en su área de experiencia
    

### Exploración de grandes bases de código

Un hecho interesante es que puede ejecutar múltiples subagentes en paralelo. Por ejemplo, puede lanzar 4 tareas paralelas con este aviso:

```
Explore the codebase using 4 tasks in parallel. 
Each agent should explore different directories.
```

Este enfoque es particularmente valioso para:

- **Grandes bases de código**: Cuando se necesita entender rápidamente múltiples componentes
    
- **Evaluación inicial del proyecto**: Obtener una visión global sin saturar una única ventana contextual
    
- **Separación de dominios**: Having different subagents focus on different architectural layers