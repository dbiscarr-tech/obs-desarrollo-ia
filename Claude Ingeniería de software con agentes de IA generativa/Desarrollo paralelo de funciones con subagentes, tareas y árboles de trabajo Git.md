# Desarrollo paralelo de funciones con subagentes, tareas y árboles de trabajo Git

Estado: Traducido automáticamente del Inglés

Traducido automáticamente del Inglés

Este tutorial muestra cómo utilizar el sistema de subagentes de Claude Code para implementar características en distintos árboles de trabajo simultáneamente, todo desde una única sesión de Claude Code. Queremos que Claude Code haga todo el trabajo de configurar los árboles de trabajo, prepararlos para el desarrollo y generar subtareas para trabajar en las características de cada árbol de trabajo.

Para que todo esto funcione, es fundamental que tenga la estructura de directorios correcta Y que ejecute Claude Code en la carpeta correcta. Ejecutaremos Claude Code desde una ubicación diferente a la que lo hicimos en el pasado.

```
/parent-directory/                    # <- Run Claude Code from HERE
├── expense-tracker-ai/               # Main project
```

**Inicie Claude Code en el directorio principal**

```
# Navigate to the parent directory containing the main project

cd /parent-directory-of-expense-tracker-ai

  

# Start Claude Code here

claude
```

**Cree el Comando de Agentes Paralelos**

Ahora, vamos a crear un comando reutilizable para trabajar en características en paralelo utilizando subagentes y árboles de trabajo. Este comando funciona de forma más fiable cuando le indicamos la carpeta específica en la que se encuentra el archivo

Guarda esto como .claude/commands/parallel-agents.md

```
I want to develop features in parallel using Git worktrees and subagents: $ARGUMENTS

You are in the parent folder of the main repo. You will need to change to the main repo
folder to create the worktrees.

Please execute this complete workflow:

PHASE 1 - SETUP WORKTREES:
For each feature mentioned:
1. Create a worktree at ../expense-tracker-[feature-name] with branch feature/[feature-name]
2. Set up the development environment in each worktree (if needed)
3. List all worktrees created

PHASE 2 - SPAWN SUBAGENTS:
For each feature, run a subagent in parallel with these instructions:
- You are working in the expense-tracker-[feature-name] worktree directory
- This is a completely isolated development environment
- Implement the [feature-name] feature with full functionality
- Include proper testing and error handling
- Compile and run tests, but don't attempt to run the application (e.g., don't do "npm run" or "npm run dev &", etc.) 
- When complete, write a detailed summary in [feature-name].work.txt in the main expense-tracker-ai directory
- The summary should include: what was implemented, files created/modified, dependencies added, testing approach, and integration notes

PHASE 3 - COORDINATION:
- Monitor all subagents working in parallel
- Ensure each subagent completes their feature implementation
- Verify each subagent creates their work summary file

PHASE 4 - FINAL SUMMARY:
After all subagents complete:
1. Read all the .work.txt files created by subagents
2. Provide a comprehensive summary of what was accomplished
3. List all features implemented and their status
4. Provide next steps for integration

Execute this complete parallel development workflow.
```

## Asómbrate

Vamos a invocar nuestro comando parallel-agents con algunas características para implementar.

```
/parallel-agents Implement a simple top expense categories page and a simple top vendors page
```

Deberías ver algún trabajo asombroso por parte de Claude Code que se parece algo a esto:

```
 Let me check if this is a git repository first:

⏺ Bash(git status)
  ⎿  On branch feature_export_system                             
     Changes not staged for commit:
       (use "git add <file>..." to update what will be committed)
     … +13 lines (ctrl+r to expand)

⏺ Good, we're already in the git repository. Now let me create the worktrees:

⏺ Bash(git worktree add ../expense-tracker-top-expense-categories -b feature/top-expense-categories)
  ⎿  HEAD is now at 6637792 Merge branch 'feature/analytics-dashboard' into integration/export-analytics
     Preparing worktree (new branch 'feature/top-expense-categories')

⏺ Bash(git worktree add ../expense-tracker-top-vendors -b feature/top-vendors)
  ⎿  HEAD is now at 6637792 Merge branch 'feature/analytics-dashboard' into integration/export-analytics
     Preparing worktree (new branch 'feature/top-vendors')

⏺ Update Todos
  ⎿  ☒ Change to main repo directory and set up worktrees
     ☒ Create worktree for top expense categories feature
     ☒ Create worktree for top vendors feature
     ☐ Spawn parallel subagents for both features
     ☐ Monitor subagent progress and collect summaries
     ☐ Provide final comprehensive summary

⏺ Let me verify the worktrees were created and install dependencies:

⏺ Bash(git worktree list)
  ⎿  /Users/juleswhite/Downloads/ai-work/expense-tracker-ai                      6637792 [feature_export_system]         
     /Users/juleswhite/Downloads/ai-work/expense-tracker-top-expense-categories  6637792 [feature/top-expense-categories]
     /Users/juleswhite/Downloads/ai-work/expense-tracker-top-vendors             6637792 [feature/top-vendors]

⏺ Bash(cd ../expense-tracker-top-expense-categories && npm install)
  ⎿  added 340 packages, and audited 341 packages in 5s

     137 packages are looking for funding
     … +3 lines (ctrl+r to expand)

⏺ Bash(cd ../expense-tracker-top-vendors && npm install)
  ⎿  added 340 packages, and audited 341 packages in 5s

     137 packages are looking for funding
     … +3 lines (ctrl+r to expand)

⏺ Perfect! Now I'll spawn subagents to work on both features in parallel:

⏺ Task(Implement top expense categories)
  ⎿  > expense-tracker-ai@0.1.0 build
     > next build

     … +26 lines (ctrl+r to expand)
     Bash(npm run build)
     Waiting…
     +11 more tool uses

⏺ Task(Implement top vendors page)
  ⎿  Updated ../expense-tracker-top-vendors/app/components/Navigation.tsx with 1 addition
     Bash(npm run build)
     Waiting…
     +11 more tool uses
```


Al final, deberías ver algo asombroso como esto:

```
FINAL SUMMARY - Parallel Feature Development Complete

  Features Successfully Implemented:

  1. Top Expense Categories Page (feature/top-expense-categories)

  - Location: /app/categories/page.tsx
  - Features:
    - Categories sorted by total spending with visual bar charts
    - Shows spending amounts, percentages, and transaction counts
    - Color-coded categories with emojis (Food 🍕, Transportation 🚗, etc.)
    - Summary statistics including total spending and top category
    - Real-time data synchronization across tabs
    - Loading and empty states

  2. Top Vendors Page (feature/top-vendors)

  - Location: /app/vendors/page.tsx
  - Features:
    - Vendors sorted by total spending (top 10)
    - Shows spending amounts, transaction counts, and last transaction dates
    - Dynamic color-coded bar charts for visual comparison
    - Summary cards with total vendors, spending, and top vendor
    - Aggregates expenses by description field (vendor name)
    - Responsive design with loading/empty states

  Worktrees Created:

  - ../expense-tracker-top-expense-categories (branch: feature/top-expense-categories)
  - ../expense-tracker-top-vendors (branch: feature/top-vendors)

  Files Modified in Both Features:

  - /app/components/Navigation.tsx - Added links for both new pages

  Next Steps for Integration:

  1. Merge feature/top-expense-categories into your integration branch
  2. Merge feature/top-vendors into your integration branch
  3. Resolve any navigation conflicts (both features modify Navigation.tsx)
  4. Test both features together in the integrated application
  5. Consider adding links from dashboard to these new analytics pages
```

