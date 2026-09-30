# Permitir que el código Claude trabaje en paralelo con Git Worktrees

Estado: Traducido automáticamente del Inglés

Traducido automáticamente del Inglés

# Git Worktrees con Claude Code: Desarrollo paralelo en aislamiento

Los worktrees de Git combinados con Claude Code crean un potente flujo de trabajo para el desarrollo paralelo. En lugar de hacer malabares con múltiples ramas en un único directorio de trabajo, puedes tener múltiples entornos aislados donde diferentes instancias de Claude Code trabajan simultáneamente en características separadas sin ningún conflicto.

## ¿Qué son los árboles de trabajo de Git?

Los árboles de trabajo de Git le permiten obtener múltiples ramas del mismo repositorio en directorios separados. Cada árbol de trabajo tiene su propio directorio de trabajo con archivos aislados, a la vez que comparte el mismo historial Git y reflog.

Piénsalo de esta manera

- **Flujo de trabajo tradicional**: Un directorio de trabajo, cambiar entre ramas
    
- **Flujo de trabajo Worktree**: Múltiples directorios de trabajo, cada uno con su propia rama
    

## Configuración de entornos de desarrollo paralelos

### Creando tu primer Worktree

Digamos que estás trabajando en tu aplicación de seguimiento de gastos y necesitas desarrollar dos características simultáneamente: un sistema de exportación de datos y un panel de analítica.

## Paso 1: Pedir a Claude Code que configure Worktrees

```
I want to develop two features in parallel for my expense tracker app using Git worktrees:

  

1. Data export system (CSV, PDF, JSON exports)

2. Analytics dashboard (charts, insights, trends)

  

Please help me set up the worktree environment:

1. Create a worktree for the export feature at ../expense-tracker-export with branch

feature/data-export

2. Create a worktree for the analytics feature at ../expense-tracker-analytics with branch

feature/analytics-dashboard

3. List all worktrees to confirm they were created

4. Explain what each worktree will contain and how they're isolated

  

I want to be able to work on both features simultaneously without conflicts.
```

Su estructura de directorios tiene ahora un aspecto similar:

/expense-tracker-ai/ # Main worktree (main branch)

/expense-tracker-export/ # Export worktree (feature/data-export)

/expense-tracker-analytics/ # Analytics worktree (feature/analytics-dashboard)

### Iniciando Claude Code en cada árbol de trabajo

Cada árbol de trabajo es un entorno de desarrollo completo e independiente. Ahora, abra dos ventanas de terminal diferentes e inicie Claude Code en cada uno de los árboles de trabajo:

**Terminal 1 - Función de Exportación:**

```
cd ../expense-tracker-export

claude
```

**Terminal 2 - Función analítica:**

```
cd ../expense-tracker-analytics

claude
```

**Ahora puede ejecutar Claude Code de forma segura en paralelo para desarrollar múltiples funciones de seguimiento de gastos simultáneamente.** Cada árbol de trabajo funciona de forma completamente aislada - los cambios realizados en un árbol de trabajo no afectarán a los otros, y puede probar cada función de forma independiente sin ningún conflicto. Esto significa que puede tener a Claude trabajando en su sistema de exportación en un terminal mientras desarrolla simultáneamente el panel de analítica en otro, todo ello sin riesgo de romper la funcionalidad existente o interferir en el trabajo de los demás. El enfoque de desarrollo en paralelo le permite progresar rápidamente en múltiples frentes mientras mantiene un código limpio y libre de conflictos.

# Fusión de funciones de Worktree Volver a la página principal

## Requisitos previos

- Ha completado funciones en árboles de trabajo separados
    
- Las funciones están confirmadas y funcionando en sus respectivas ramas
    
- Se encuentra en el directorio principal del proyecto, en la rama original a partir de la cual creó los árboles de trabajo
    
- Detenga Claude Code en cada uno de los terminales separados para no confundirse
    
- Ejecute Claude Code en el directorio principal del proyecto en la rama original
    

## Paso 1: Pida a Claude Code que se encargue de la integración

```
I have two features developed in parallel worktrees that I need to integrate:

- feature/data-export (export functionality)

- feature/analytics-dashboard (analytics functionality)

  

Please help me integrate these features:

1. Create a new integration branch called "integration/export-analytics"

2. Merge feature/data-export into the integration branch

3. Merge feature/analytics-dashboard into the integration branch

4. Resolve any merge conflicts that arise

5. Test that both features work together

6. Run all tests to ensure nothing is broken

  

I want to integrate these safely before merging to main.
```

## Resultado

Sus funciones paralelas están ahora integradas en su rama principal, y todos los árboles de trabajo están limpios. Pruebe su aplicación para asegurarse de que todo funciona correctamente

# ¡Haga estos comandos reutilizables!

## Comando de trabajo paralelo

Guarda esto como .claude/commands/parallel-work.md

```
I want to develop features in parallel for my expense tracker app using Git worktrees: $ARGUMENTS

Think about how to divide the work up into separate features unless this has been 
clearly explained already. 

Please help me set up the worktree environment:
1. For each feature mentioned, create a worktree at ../expense-tracker-[feature-name] 
with branch feature/[feature-name]
2. Set up the development environment in each worktree
3. List all worktrees to confirm they were created
4. Explain what each worktree will contain and how they're isolated

I want to be able to work on all features simultaneously without conflicts.
```

# Comando de Integración

## Comando Integrar Trabajo Paralelo

Guardar como .claude/commands/integrate-parallel-work.md

```
I have features developed in parallel worktrees that I need to integrate: $ARGUMENTS

Please help me integrate these features:
1. Create a new integration branch called "integration/parallel-features"
2. For each feature name provided, merge the branch feature/[feature-name] into the integration branch
3. Resolve any merge conflicts that arise
4. Test that all features work together
5. Run all tests to ensure nothing is broken
6. Once integration is successful, merge to main and clean up branches

I want to integrate these safely before merging to main.
```

Ejemplo de uso del comando: /integrate-parallel-work budget-tracking notifications user-settings