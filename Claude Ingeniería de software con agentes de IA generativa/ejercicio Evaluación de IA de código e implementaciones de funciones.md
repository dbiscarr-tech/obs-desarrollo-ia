ejercicio: Evaluación de IA de código e implementaciones de funciones

Estado: Traducido automáticamente del Inglés

Traducido automáticamente del Inglés

## Requisitos previos

- Tutorial 2.2 completado (implementación del patrón Best-of-N)
    
- Tres implementaciones diferentes de la función de exportación de datos en ramas de Git separadas
    
- Comprensión básica de los principios de evaluación de código
    
- Código Claude instalado y en funcionamiento
    

## Parte 1 Por qué la evaluación del código es importante para el desarrollo de la IA

### El reto de la evaluación

Cuando se trabaja con IA, a menudo se tienen varias soluciones entre las que elegir. A diferencia del desarrollo tradicional, en el que normalmente sólo se construye una versión, la velocidad de la IA permite explorar múltiples enfoques. Pero esto crea un nuevo reto:**¿Cómo evaluar y comparar sistemáticamente las diferentes implementaciones?**

### Evaluación del desarrollo tradicional frente al de IA

**Desarrollo tradicional:**

- Evaluar una solución frente a alternativas teóricas
    
- Tomar decisiones basadas en la experiencia y la intuición
    
- Capacidad limitada para probar diferentes enfoques
    
- Costes elevados para cambiar de dirección
    

**Desarrollo de IA:**

- Evaluar múltiples implementaciones operativas
    
- Comparar código real, no enfoques teóricos
    
- Probar experiencias del usuario reales en paralelo
    
- Toma de decisiones basada en datos sobre la arquitectura
    

### Qué estamos evaluando

En este tutorial, evaluaremos tres implementaciones diferentes de la misma función en múltiples dimensiones.

## Parte 2: Preparación para una evaluación sistemática

### Paso 1: Verifique sus implementaciones

Primero, asegúrese de que tiene las tres versiones del Tutorial 2.2:

```
cd expense-tracker-ai

git branch -a
```


Debería ver

- main
    
- feature-data-export-v1 (exportación CSV simple)
    
- feature-data-export-v2 (Exportación avanzada con opciones)
    
- feature-data-export-v3 (Funciones de integración en la nube)
    

### Paso 2: Lanzar Claude Code

claude

### Paso 3: Crear y ejecutar la evaluación

Ahora hagamos que Claude examine sistemáticamente cada implementación cambiando entre ramas y analizando la arquitectura del código, el diseño de la interfaz de usuario y los enfoques técnicos utilizados en cada versión. Esta revisión sistemática del código nos ayudará a comprender no sólo lo que hace cada versión, sino también cómo logra sus objetivos y qué concesiones se hicieron en la implementación.

Documentaremos nuestros hallazgos en un formato estructurado que cubra la arquitectura técnica, la organización del código, las bibliotecas utilizadas y los patrones de implementación de cada versión. Este análisis sentará las bases para tomar decisiones informadas sobre qué enfoque adoptar o cómo combinar los mejores elementos de varias versiones.

**Sugerencia para la evaluación:**

```
I have three different implementations of data export functionality across three git branches in my expense tracker application. I want to create a systematic evaluation framework to compare them thoroughly.

BACKGROUND:
- feature-data-export-v1: Simple CSV export (one-button approach)
- feature-data-export-v2: Advanced export with multiple formats and filtering options
- feature-data-export-v3: Cloud integration with sharing and collaboration features

Now I want you to systematically analyze each of three features implementations by switching between branches and examining the code, architecture, and implementation details.

ANALYSIS PROCESS:
For each branch (v1, v2, v3), please:

1. Switch to the branch
2. Examine all the files that were created or modified
3. Analyze the code architecture and patterns used
4. Look at component structure and organization
5. Review the user interface implementation
6. Check for error handling and edge cases
7. Assess the technical approach and libraries used

DOCUMENTATION:
Create a file called "code-analysis.md" with detailed findings for each version:

**For Each Version, Document:**
- Files created/modified (list them)
- Code architecture overview (how is it organized?)
- Key components and their responsibilities
- Libraries and dependencies used
- Implementation patterns and approaches
- Code complexity assessment
- Error handling approach
- Security considerations
- Performance implications
- Extensibility and maintainability factors

**Technical Deep Dive:**
- How does the export functionality work technically?
- What file generation approach is used?
- How is user interaction handled?
- What state management patterns are used?
- How are edge cases handled?

Be thorough and technical - this analysis will inform our decision about which approach to adopt or how to combine them.
```


## Qué buscar después del análisis inicial de Claude

### Paso 3: Revisar el análisis de código de Claude

Una vez que Claude complete el análisis sistemático del código, revise el archivo generado **"code-analysis.md** " para obtener una visión de alto nivel de lo dramáticamente diferentes que son realmente los tres enfoques. Busque las grandes diferencias en arquitectura, complejidad y filosofía de implementación - esto le ayudará a entender si realmente exploró diferentes espacios de solución o simplemente creó variaciones del mismo enfoque básico.

### Construir su marco de evaluación personal

Aunque el enfoque de evaluación predeterminado de Claude es exhaustivo, es posible que tenga criterios específicos que sean los más importantes para sus proyectos, equipo o sector. Crear una plantilla de evaluación personalizada garantiza que cada comparación Best-of-N se centre en lo que realmente es importante para usted. Piense en lo que realmente importa para sus proyectos y cree un archivo llamado my-evaluation-template.md. A continuación, dígale que lea y aplique esa evaluación la próxima vez que le pida que implemente varias versiones de una función.