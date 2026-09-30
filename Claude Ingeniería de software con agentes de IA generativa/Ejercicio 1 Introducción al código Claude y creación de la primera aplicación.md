# Ejercicio: Introducción al código Claude y creación de la primera aplicación

Estado: Traducido automáticamente del Inglés

Traducido automáticamente del Inglés

Información:

Este elemento incluye contenido que aún no se tradujo a tu idioma preferido.

## Requisitos previos

- Familiaridad básica con la línea de comandos/terminal
    
    - Si necesita ayuda, abra [Claude](https://claude.ai/) e indíquelo con: "Teach me how to use the command line on my system. Ask me one question at a time or tell me one step at a time what to do. What is the first question or step?"
        
- Node.js instalado en tu sistema[(https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)
    
    - Si necesitas ayuda, abre [Claude](https://claude.ai/) e indícale: "Help me install NodeJS and NPM. Ask me one question at a time or tell me one step at a time what to do. What is the first question or step?"
        
- Git instalado y configurado
    
    - Si necesitas ayuda, abre [Claude](https://claude.ai/) y pregúntale: "Help me install the Git CLI. Ask me one question at a time or tell me one step at a time what to do. What is the first question or step?"
        

## Parte 1: Instalando Claude Code

### Paso 1: Instalación

Para obtener acceso a Claude Code:

1. Obtenga acceso: Visita[claude.ai/code](https://claude.ai/code) para suscribirte a una suscripción que incluya Claude Code u obtén una clave API para pagar por el uso
    
2. Instale a través de npm (una vez que tenga acceso): npm install -g @anthropic-ai/claude-code
    
3. Verifique la instalación:Claude --version
    

### Paso 2: Primer lanzamiento

1. Cree un nuevo directorio de proyecto: mkdir expense-tracker-ai
    
2. Cambie al directorio del proyecto: cd expense-tracker-ai
    
3. Lanzar Claude Code:Claude
    
4. Configurar por primera vez: Claude le pedirá que se autentique y configure los permisos básicos.
    

## Parte 2: Comprender la interfaz

### Elementos clave de la interfaz

Cuando inicie Claude Code, verá:

- Prompt input: Donde escribirá sus instrucciones
    
- Solicitudes de permiso de herramientas: Claude preguntará antes de ejecutar comandos
    
- Operaciones con archivos: Verás la creación y edición de archivos en tiempo real
    
- Actualizaciones de todo: Claude explica lo que está haciendo mientras trabaja
    

### Comandos esenciales

- **/init**: Inicializa un archivo CLAUDE.md para tu proyecto (cuando hay un proyecto existente)
    
- **/permissions**: Gestiona los permisos de la herramienta
    
- **/clear**: Borra el historial de conversaciones
    
- **/help**: Mostrar los comandos disponibles
    
- **Comandos de barra oblicua**: Escribe / para ver todos los comandos disponibles
    

## Parte 3: Su Primer Big Prompt - Construyendo un Rastreador de Gastos

### Paso 1: Inicializar el Proyecto con el Enfoque Big Prompt

En lugar de pedirle a Claude que "cree un componente" o "añada una función", vamos a utilizar una**gran instrucción** que describa toda nuestra visión. Esta es la clave para aprovechar la IA como mano de obra y no sólo como asistente de codificación.

```
I want you to create a modern, professional NextJS expense tracking application. Here's my vision:

APPLICATION OVERVIEW:
Build a complete security streategy tracking web app that campany can manage their helps users manage their personal finances. The app should feel modern, intuitive, and professional.

CORE FEATURES:
- Add expenses with date, amount, category, and description
- View expenses in a clean, organized list
- Filter expenses by date range and category
- Dashboard with spending summaries and basic analytics
- Categories: Food, Transportation, Entertainment, Shopping, Bills, Other
- Data persistence using localStorage for this demo

TECHNICAL REQUIREMENTS:
- NextJS 14 with App Router
- TypeScript for type safety
- Tailwind CSS for styling with a modern, clean design
- Responsive design that works on desktop and mobile
- Use React hooks for state management
- Form validation for expense inputs
- Date picker for expense dates
- Currency formatting for amounts

DESIGN REQUIREMENTS:
- Clean, modern interface with a professional color scheme
- Intuitive navigation and user experience
- Visual feedback for user actions
- Loading states and error handling
- Mobile-responsive design
- Multi language, need to support at lease: spanish and english.

SPECIFIC FUNCTIONALITY:
- Expense form with validation
- Expense list with search and filter capabilities
- Summary cards showing total spending, monthly spending, top categories
- Basic charts or visual representations of spending patterns
- Export functionality (at least CSV)
- Delete and edit existing expenses

Please create this as a complete, production-ready application. Set up the project structure, implement all features, and make sure everything works together seamlessly. Focus on creating something that looks professional and that I could actually use to track my expenses.

When you're done, provide instructions on how to run the application and test all features.
```

### Paso 3: Ver trabajar a Claude

Después de enviar este mensaje, Claude

1. Planificará el enfoque: Esbozará cómo construirá la aplicación
    
2. Creará la estructura del proyecto: Configurará NextJS con TypeScript y Tailwind
    
3. Construir componentes: Creará todos los componentes React necesarios
    
4. Implementar funcionalidades: Añadir todas las funcionalidades solicitadas
    
5. **Probar y verificar**: Asegurarse de que todo funciona junto (puede que se olvide de hacer esto...si es así, recuérdaselo con algo como "por favor, compila y asegúrate de que la aplicación se ejecuta...")
    

**Importante**: Claude pedirá permisos para:

- Ejecutar comandos de terminal (npm create-next-app, npm install, etc.)
    
- Crear y editar archivos
    
- Instalar dependencias
    

Revisa siempre lo que te pide hacer y aprueba las operaciones seguras.

### Paso 4: Entender la salida

Mientras Claude trabaja, presta atención a

- La estructura de los archivos: Cómo organiza el proyecto
    
- Arquitectura de componentes: Cómo descompone la aplicación
    
- Gestión de estados: Cómo gestiona el flujo de datos
    
- Enfoque de estilo: Cómo implementa el diseño
    

### Paso 5: Prueba de la aplicación

Una vez que Claude termine, te dará instrucciones para ejecutar la aplicación:

npm run dev

Abre tu navegador a http://localhost:3000 y prueba.

Detener la aplicación: Cuando termines de probar, asegúrate de detener el servidor de desarrollo para liberar el puerto.En tu terminal, presiona Ctrl + C para terminar el proceso.Esto asegura que el puerto 3000 no quede corriendo en segundo plano.