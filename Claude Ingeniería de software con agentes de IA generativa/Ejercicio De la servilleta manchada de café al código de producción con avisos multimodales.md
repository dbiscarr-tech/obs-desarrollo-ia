# Ejercicio: De la servilleta manchada de café al código de producción con avisos multimodales

Estado: Traducido automáticamente del Inglés

Traducido automáticamente del Inglés

_Estás sentado en una cafetería pensando en tu aplicación de seguimiento de gastos cuando te viene la inspiración. Coges una servilleta y empiezas a esbozar tus ideas, pero tu café con leche deja algunas manchas en tu obra maestra. No hay problema: Claude Code puede convertir incluso los bocetos más desordenados en código pulido y listo para producción._

## El escenario

Lleva unas semanas utilizando su rastreador de gastos básico, pero se da cuenta de que necesita una forma mejor de visualizar sus patrones de gasto. Durante la pausa para el café de la mañana, esbozas una idea para un panel de control "Expense Insights" en el único papel disponible: la servilleta de una cafetería.

El boceto incluye:

- Un encabezado que diga "Información mensual"
    
- Un Gráfico de anillos que muestre los gastos por categoría
    
- Una lista de las 3 principales categorías de gasto con sus importes
    
- Un indicador de "racha de gastos" (días sin gastar de más)
    
- Algunas manchas de anillos de café que tú decides que añaden carácter
    

**El boceto**

Has hecho un buen trabajo con tu boceto (bueno, en realidad Claude ha hecho un buen trabajo):

![](https://d3c33hcgiwev3.cloudfront.net/imageAssetProxy.v1/_bd339514467649dbb68bb1f3d9bbe845_Screenshot-2025-07-10-at-12.47.52-PM.png?expiry=1790251715832&hmac=pjUBxP4eaTXLDmNUR13cCH_xhDRcBWp68UTRsfca0Lo)

## Ejercicio: Transformar un Boceto en Código

### Paso 1: Configurar tu entorno

Asegúrate de estar en el directorio de tu proyecto de seguimiento de gastos e inicia Claude Code:

```
I sketched an expense insights dashboard on a napkin at a coffee shop. 
Please implement this as a React component called MonthlyInsights.

Looking at this sketch: /your/path/to/the/napkin-insights-mockup.png

Implement a new screen in the application that mirrors what you see in the mockup.
```

## Ejercicio: Transformando Sketch a Código

### Paso 1: Guardar el diseño de la servilleta

Primero, necesita guardar la maqueta de la servilleta como un archivo de imagen que pueda mostrar a Claude Code.

**Haga clic con el botón derecho en el diseño de la servilleta de arriba y seleccione "Guardar imagen como..." o "Guardar imagen como..."**

Guárdala como servilleta-insights-mockup.png en tu carpeta de proyecto del rastreador de gastos o en el Escritorio.

### Paso 3: Muéstrale a Claude tu boceto de "servilleta

Ahora llega el momento mágico - está a punto de mostrar a Claude Code su boceto y ver cómo se transforma en código listo para producción. Utilice este comando**(ACTUALICE LA RUTA A LA IMAGEN**):

1

2

3

4

5

6

(También puede arrastrar/soltar la imagen en el área de instrucciones de Claude Code)

# Equipo Whiteboard to Code: Construir mientras estás en el descanso

Imagínate esto: Acabas de terminar una intensa reunión de equipo donde todo el mundo estaba esbozando ideas en la pizarra. La pizarra está cubierta de flujos de usuarios, diseños de componentes y diagramas de arquitectura. En lugar de perder el impulso mientras el equipo se toma un café, puedes hacer fotos de la pizarra y pedir a Claude Code que implemente varias versiones de tus ideas, de modo que cuando vuelvas con el café recién hecho, tengas prototipos en funcionamiento para evaluar.

# Es más fácil mostrar que contar: la comunicación visual en el desarrollo

Cuando se trabaja con Claude Code, ciertos conceptos son mucho más fáciles de comunicar a través de imágenes que a través de descripciones de texto. Entender cuándo recurrir a una captura de pantalla, un boceto o un diagrama puede ahorrarte tiempo y conducir a resultados mucho mejores.

## Diseño y maquetación de IU/UX

### Relaciones espaciales

**Por qué gana lo visual:** Describir cómo deben colocarse los elementos entre sí es increíblemente prolijo y propenso a malinterpretaciones.

**Enfoque textual:**"La barra de búsqueda debe estar centrada horizontalmente en la parte superior, con el logotipo a 20px a la izquierda y el avatar del usuario a 15px a la derecha. Debajo, debe haber un menú de navegación con el mismo espacio entre los elementos, y el área de contenido principal debe empezar 40px por debajo de la navegación..."

**Enfoque visual:**_Muestra el wireframe o maqueta_"Implementa este diseño exactamente como se muestra."

### Esquemas de color y jerarquía visual

**Por qué gana lo visual:** Los colores, degradados, sombras y énfasis visual son casi imposibles de describir con precisión.

Enfoque**textual**: "Utiliza un azul que no sea demasiado oscuro pero tampoco demasiado claro, algo así como un azul profesional pero más cálido, con un degradado sutil que vaya de ligeramente más claro en la parte superior a ligeramente más oscuro en la parte inferior..."

**Enfoque visual:**_Muestra la paleta de colores o el diseño existente_"Haz coincidir estos colores exactos y el tratamiento visual."

### Comportamiento responsivo

**Por qué gana lo visual:** Cómo deben adaptarse los diseños a los distintos tamaños de pantalla implica un complejo razonamiento espacial.

Enfoque**textual**: "En el móvil, la barra lateral debe colapsarse en un menú de opciones, las tarjetas deben apilarse verticalmente en lugar de en una cuadrícula, y los tamaños de fuente deben reducirse proporcionalmente manteniendo la legibilidad..."

**Enfoque visual:**_Muestra capturas de pantalla de móvil y escritorio una al lado de la otra_"Hazlo responsive así"

## Visualización de datos y gráficos

### Tipos de gráficos y estilos

**Por qué gana lo visual:** La apariencia de los gráficos implica muchas decisiones de diseño sutiles que son difíciles de articular.

Enfoque**textual**: "Crea un Gráfico de anillos con el agujero aproximadamente al 40% del radio, utiliza estos cinco colores pero asegúrate de que sean visualmente distintos, añade etiquetas que no se solapen y coloca la leyenda a la derecha a menos que no haya espacio suficiente..."

**Enfoque visual:**_Muestra un gráfico de ejemplo o un esquema_"Crea un gráfico con este aspecto"

### Diseño de cuadros de mando

**Por qué gana lo visual:** Los cuadros de mando implican disposiciones complejas de múltiples visualizaciones de datos.

Enfoque**textual**: "Coloca las principales tarjetas de KPI en la parte superior en una fila de cuatro, debajo un gráfico grande que ocupe dos tercios de la anchura con un gráfico más pequeño a la derecha, y debajo una tabla que ocupe toda la anchura..."

**Enfoque visual:**_Muestra la maqueta del cuadro_ de mandos "Implementa este diseño de cuadro de mandos"

## Flujos de usuarios e interacciones

### Procesos de varios pasos

**Por qué gana lo visual:** Los flujos de usuarios implican cambios de estado, lógica condicional y múltiples pantallas que son difíciles de describir linealmente.

Enfoque**textual**: "Primero el usuario ve un formulario, después de enviarlo va a una página de confirmación, pero si hay un error se queda en el formulario con mensajes de error, y si no ha iniciado sesión va primero al inicio de sesión, a menos que sea un usuario invitado en cuyo caso..."

**Enfoque visual:**_Muestra el diagrama de flujo con flechas y puntos de decisión_"Implementa este flujo de usuarios."

### Animación y transiciones

**Por qué gana lo visual:** El movimiento y el tiempo son conceptos inherentemente visuales.

Enfoque**textual**: "Cuando se abra el modal, debe deslizarse desde abajo con una función de suavizado suave, no demasiado rápido pero tampoco demasiado lento, y el fondo debe oscurecerse pero no volverse completamente negro..."

**Enfoque visual:**_Muestra vídeo o GIF animado_"Haz que se anime así"

## Estados de error y Casos extremos

### Informes de errores visuales

**Por qué gana lo visual:** Los problemas de diseño, estilización y fallos visuales son inmediatamente obvios en las capturas de pantalla.

Enfoque**textual**: "El texto de los botones está cortado y hay un espaciado extraño alrededor de los campos del formulario, y los colores no se ven bien, y en el móvil la navegación se superpone al contenido..."

**Enfoque visual:**_Muestra una captura de pantalla del problema_"Arregla lo que está mal en esta captura de pantalla"

### Estados vacíos y estados de carga

**Por qué gana lo visual:** Estos estados implican mensajes visuales específicos y consideraciones de diseño.

Enfoque**textual**: "Cuando no haya datos, muestra un mensaje amigable con una ilustración, quizá algo sobre cómo empezar, y asegúrate de que esté centrado y no tenga un aspecto demasiado vacío..."

Enfoque**visual:**_Muestra ejemplos de buenos_ estados vacíos "Diseña estados vacíos como estos ejemplos"

## Arquitectura y diseño de sistemas

### Diagramas de arquitectura del sistema

**Por qué gana lo visual:** Las relaciones entre sistemas, el flujo de datos y las interacciones entre componentes son conceptos espaciales.

Enfoque**textual**: "El frontend habla con la pasarela API, que dirige a tres microservicios, cada uno con su propia base de datos, y hay una cola de mensajes entre el servicio de pedidos y el servicio de inventario..."

**Enfoque visual:**_Muestra el diagrama de arquitectura_"Implementa esta arquitectura de sistema"

### Relaciones de esquema de base de datos

**Por qué gana lo visual:** Las relaciones entre tablas, las claves foráneas y el modelado de datos son inherentemente visuales.

Enfoque**textual**: "Los usuarios tienen muchos gastos, los gastos pertenecen a categorías, las categorías pueden tener subcategorías y existe una relación de muchos a muchos entre los gastos y las etiquetas..."

Enfoque**visual:**_Muestra el diagrama entidad relación_"Crea este esquema de base de datos"

## Lógica empresarial compleja

### Árboles de decisiones y lógica condicional

**Por qué gana lo visual:** Los escenarios complejos de si/entonces son más fáciles de seguir visualmente.

Enfoque**textual**: "Si el usuario es premium y el gasto es superior a 100 $ pero inferior a 1.000 $ y está en la categoría de comida y es un día laborable, aplíquele el descuento premium, pero si es fin de semana o no es premium..."

**Enfoque visual:**_Muestra el diagrama de_ flujo de_decisiones_"Implementa este flujo de lógica de negocio"

### Diagramas de flujo de trabajo

**Por qué gana lo visual:** Los procesos de negocio implican múltiples actores, puntos de decisión y flujos paralelos.

Enfoque**textual**: "El gestor aprueba el gasto, luego pasa a contabilidad, pero si supera los 500 dólares necesita primero la aprobación del director, y si está relacionado con viajes también necesita la revisión de RR.HH..."

Enfoque**visual:**_Muestra el diagrama de flujo_ de trabajo "Construye este flujo de trabajo de aprobación"

## Cuestiones técnicas y de rendimiento

### Resultados del perfil de rendimiento

**Por qué gana lo visual:** Los datos de rendimiento implican gráficos, líneas de tiempo y patrones visuales.

Enfoque**textual**: "El subproceso principal se bloquea durante unos 200 ms durante la renderización, hay una fuga de memoria que aumenta gradualmente con el tiempo, y hay varias funciones que tardan más de lo esperado..."

Enfoque**visual:**_Muestra el perfil de rendimiento de DevTools_"Optimizar en base a este análisis de rendimiento."

### Arquitectura de red

**Por qué gana lo visual:** Las topologías de red, el balanceador de carga y la infraestructura son conceptos espaciales.

Enfoque**textual**: "Pon un balanceador de cargas delante de tres servidores de aplicaciones, con un clúster de bases de datos detrás, y CDN para activos estáticos, con monitorización en todo..."

**Enfoque visual:**_Muestra un diagrama de topología de red_"Configura esta infraestructura"

## Cuando la comunicación visual transforma la productividad

### Transferencias de diseño

En lugar de escribir especificaciones detalladas, los diseñadores pueden mostrar maquetas y Claude Code puede implementarlas directamente.

### Informes de errores

Las capturas de pantalla con anotaciones son infinitamente más útiles que las descripciones textuales de problemas visuales.

### Comunicación con las partes interesadas

Mostrar prototipos de trabajo basados en bocetos crea una comprensión y un feedback inmediatos.

### Revisiones de código

Las capturas de pantalla del antes y el después ayudan a los revisores a comprender el impacto de los cambios.

### Documentación

Los ejemplos visuales en la documentación son más eficaces que las explicaciones puramente textuales.