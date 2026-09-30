# Creación de comandos Claude

Estado: Traducido automáticamente del Inglés

Traducido automáticamente del Inglés

## ¿Qué son los comandos de Claude Code?

Mientras que claude.md proporciona un contexto global para cada interacción, los comandos proporcionan**un contexto y un proceso específicos para tareas concretas y repetibles**. Piense en los comandos como conjuntos de instrucciones especializadas que proporcionan a Claude Code exactamente lo que necesita saber para determinados flujos de trabajo, sin abrumarlo con información irrelevante.

## El marco TARGETED para el diseño de comandos

Utilice este acrónimo para crear comandos eficaces:

- Instrucciones**Específicas de Tarea**
    
- **Argumentos**y marcadores de posición
    
- Pasos de proceso**reutilizables**
    
- Ejemplos**guiados**y referencias
    
- Requisitos de salida**explícitos**
    
- Nomenclatura**basada en plantillas**
    
- Tratamiento de**errores**y Casos extremos
    
- **Documentación**y contexto
    

## Principios básicos

### 1. Contexto adecuado en el momento adecuado

Los comandos resuelven el problema del "manual de 400 páginas" proporcionando sólo el contexto relevante necesario para una tarea específica, evitando la sobrecarga de información.

### 2. Coherencia reutilizable

Los comandos garantizan que se siga el mismo proceso de alta calidad cada vez que se realiza una tarea, escalando sus mejores prácticas a través de su trabajo de IA.

### 3. Automatización basada en plantillas

Utilice marcadores de posición y plantillas para flexibilizar los comandos al tiempo que mantiene la estructura y las convenciones de nomenclatura.

## Estructura y ubicación de comandos

Los comandos se almacenan como archivos markdown en el directorio .claude/commands/:

- **Específicos del proyecto**: .claude/commands/ (versionados con su proyecto)
    
- **Comandos globales**: ~/.claude/commands/ (disponibles en todos los proyectos)
    

# Consejos para la gestión de comandos

## Estrategias de organización

- **Por función**: plan-feature.md, impl-api.md, test-unit.md, deploy-prod.md, ...
    
- **Por dominio**: auth-login.md, user-profile.md, payment-process.md, order-create.md, ...
    
- **Por función**: dev-review.md, qa-automation.md, ops-deploy.md, pm-requirements.md, ...
    

## Control de versión

- Almacene los comandos del proyecto en .claude/commands/ para compartirlos en equipo
    
- Utilice mensajes de confirmación descriptivos al actualizar los comandos
    
- Revise los cambios en los comandos como parte del proceso de revisión del código
    

## Evolución de comandos

- Revise y actualice periódicamente los comandos basándose en los comentarios del equipo
    
- Archivar los comandos obsoletos en lugar de eliminarlos
    
- Documentar los cambios de comandos en el registro de cambios del proyecto
    

Los comandos transforman a Claude Code de un asistente general en un miembro especializado del equipo con un profundo conocimiento de sus flujos de trabajo específicos. Garantizan la ejecución coherente y de alta calidad de tareas repetitivas, al tiempo que proporcionan el contexto específico necesario para operaciones complejas.

# Ejemplos de comandos

Estos comandos no deben utilizarse "tal cual". Debe tomarlos y adaptarlos a su proyecto. El objetivo es servir de inspiración sobre el aspecto que podría tener un comando, no sobre cómo debería ser. Los mejores comandos se adaptarán a su proyecto específico.

## Ejemplo 1: Comando de revisión de código

**Archivo**: .claude/commands/code-review.md

```
# Code Review Command

Carefully perform a comprehensive code review of $ARGUMENTS.

## Review Standards
Examples of excellent code that you should match the design/style/conventions of:
- `src/components/UserProfile/UserProfile.tsx` (React components)
- `src/utils/dataValidation.ts` (utility functions)
- `src/hooks/useUserData.ts` (custom hooks)

## Process
1. **First**: Read the example files above to understand our design patterns, naming conventions, and code style
2. **Second**: Analyze $ARGUMENTS against these standards
3. **Third**: Create detailed critique covering:
   - Code structure and organization
   - Adherence to established patterns
   - Performance considerations
   - Security implications
   - Maintainability concerns
   - Test coverage gaps

## Output Requirements
- Save review as `ai-code-reviews/{filename}.review.md` for each file reviewed
- Include specific line references for issues
- Provide concrete suggestions for improvements
- Rate overall quality: Excellent/Good/Needs Improvement/Poor
- Estimate refactoring effort: Low/Medium/High

## Review Checklist
- Follows project naming conventions
- Proper error handling implemented
- No hardcoded values, secrets, or magic numbers
- Appropriate comments and documentation
- Follows existing design principles and consistent with exemplars
- No obvious security vulnerabilities
- Performance optimizations considered
```


## Ejemplo 2: Prueba de API

**Archivo**: .claude/commands/api-test

```
# API Testing Command

Create comprehensive API tests for: $ARGUMENTS

## Testing Strategy
Test the following API endpoints and scenarios based on $ARGUMENTS:

1. **Happy Path Testing**:
   - Valid request formats
   - Expected response structures
   - Proper HTTP status codes

2. **Error Handling Testing**:
   - Invalid request payloads
   - Authentication failures
   - Authorization edge cases
   - Rate limiting scenarios

3. **Edge Cases**:
   - Boundary value testing
   - Large payload handling
   - Concurrent request handling
   - Network timeout scenarios

## Test Structure Template
Create tests in `/tests/api/{endpoint-name}.test.ts`:

```typescript
describe('{Endpoint Name} API', () => {
  describe('POST /{endpoint}', () => {
    it('should create {resource} with valid data', async () => {
      // Test implementation
    });
    
    it('should return 400 for invalid data', async () => {
      // Test implementation
    });
    
    it('should require authentication', async () => {
      // Test implementation
    });
  });
  
  describe('GET /{endpoint}', () => {
    // Additional test cases
  });
});
```