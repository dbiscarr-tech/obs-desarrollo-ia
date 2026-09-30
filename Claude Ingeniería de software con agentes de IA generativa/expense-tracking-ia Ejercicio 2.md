# Ejercicio: Construir múltiples versiones de una función con el mejor de N

Estado: Traducido automáticamente del Inglés

Traducido automáticamente del Inglés

## Requisitos previos

- Completado el primer ejercicio - [NextJS aplicación de seguimiento de gastos](https://www.coursera.org/learn/claude-code/supplement/kSPiN/exercise-getting-started-with-claude-code-building-your-first-application)
    
- Conocimientos básicos de las ramas de Git.
    
    - Si necesitas ayuda, pide ayuda a Claude con "Teach me about Git branches. Let's do it interactively. Don't create an artifact. Explain things using concrete examples with example repositories with a handful of files."
        
- Código Claude instalado y proyecto de seguimiento de gastos en funcionamiento
    

## Parte 1: Entendiendo el patrón Best-of-N

### ¿Qué es Best-of-N?

El patrón Best-of-N aprovecha una ventaja única del trabajo de la IA:**es rápido, barato y sin ego**. A diferencia de los desarrolladores humanos, Claude no se frustra cuando le pides que deseche el trabajo y vuelva a intentarlo.

**Desarrollo tradicional**:

- Implementar una solución
    
- Seguir con ella (es demasiado caro reescribirla)
    
- Esperar que sea el mejor enfoque
    

**Desarrollo AI Labor**:

- Implementar 3-5 soluciones diferentes
    
- Compare y evalúe todas ellas
    
- Elegir la mejor o combinar elementos
    
- Tiempo total: Aún más rápido que una implementación tradicional
    

### Por qué funciona

1. **La IA es rápida**: Lo que a los humanos les lleva días, a la IA le lleva minutos
    
2. La**IA es barata**: Incluso el uso premium de la IA cuesta menos que el tiempo del desarrollador
    
3. La**IA es creativa**: Cada intento puede explorar enfoques radicalmente diferentes
    
4. La**IA no tiene ego**: No hay sentimientos heridos cuando se descarta su trabajo
    

## Parte 2: Configuración para múltiples implementaciones

### Paso 1: Prepare su repositorio

En primer lugar, asegúrese de que su rastreador de gastos está comprometido y funcionando:

cd expense-tracker-ai

git add .

git commit -m "Initial expense tracker implementation"

git status

### Paso 2: Lanzar Claude Code

claude

## Parte 3: Implementación de la versión 1 - Exportación simple a CSV

### Petición de características

Vamos a implementar la funcionalidad de exportación de datos de tres maneras diferentes. Comencemos con la Versión 1.

**Preguntar por la Versión 1:**

19

20

21

22

23

24

25

### Qué observar

Mientras Claude trabaja en la Versión 1, observe

- Cómo crea la rama
    
- El enfoque que adopta (probablemente usando APIs del navegador)
    
- La simplicidad de la implementación
    
- Dónde coloca el botón de exportación
    
- El formato de los datos CSV
    

### Versión de prueba 1

Una vez que Claude complete la implementación

1. Inicia el servidor de desarrollo: npm run dev
    
2. Añade algunos gastos de prueba
    
3. Haga clic en el botón Exportar
    
4. Comprueba que el archivo CSV se descarga y se abre correctamente
    

## Parte 4: Implementación de la Versión 2 - Exportación avanzada con opciones

### Reiniciar y crear la versión 2

Ahora le pediremos a Claude que implemente la misma función de manera completamente diferente.

**Solicitar la Versión 2:**

[[Version 2]]

## Parte 5: Implementación de la Versión 3 - Integración en la Nube

### Crear Versión 3

Exploremos un tercer enfoque completamente diferente.

**Pregunta para la Versión 3:*

[[Version 3]]

## Parte 6: Comparando sus Tres Versiones

### Cambiar entre ramas

Ahora puedes cambiar fácilmente entre las tres implementaciones:

```
# View all branches

git branch -a

  

# Switch to Version 1

git checkout feature-data-export-v1

npm run dev

# Test the simple CSV export

  

# Switch to Version 2

git checkout feature-data-export-v2

npm run dev

# Test the advanced modal with options

  

# Switch to Version 3

git checkout feature-data-export-v3

npm run dev

# Test the cloud integration features
```