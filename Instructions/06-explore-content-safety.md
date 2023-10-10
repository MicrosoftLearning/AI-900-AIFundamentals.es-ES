Content Safety Studio permite explorar cómo se puede moderar el contenido de texto e imagen. Puede ejecutar pruebas en texto o imágenes de ejemplo y obtener una puntuación de gravedad que va de "segura" a "alta" para cada categoría. Puede averiguar rápidamente cómo funciona el servicio de inteligencia artificial de Seguridad del contenido y de qué es capaz. 

En este ejercicio de laboratorio, creará un recurso de Servicios de Azure AI con varios servicios en Azure Portal e inspeccionará su punto de conexión y sus claves. A continuación, usará Content Safety Studio para explorar la funcionalidad del servicio de inteligencia artificial de Seguridad del contenido. 

## Creación de un recurso de Servicios de Azure AI

1.  En otra pestaña del explorador, abra Azure Portal en [https://portal.azure.com](https://portal.azure.com?azure-portal=true) e inicie sesión con su cuenta Microsoft.
1.  Haga clic en el botón **&#65291;Crear un recurso** y busque *Servicios de Azure AI*. Seleccione **Crear** un plan de **servicios de Azure AI**. Se le dirigirá a una página para crear un recurso de servicios de Azure AI. Configúrelo con los valores siguientes:
- **Suscripción**: Su suscripción de Azure.
- **Grupo de recursos**: seleccione o cree un grupo de recursos adecuado.
- **Región**: elija cualquier región disponible.
- **Nombre**: Escriba un nombre único.
- **Plan de tarifa**: F0. 
2.  Revise y cree el recurso y espere a que finalice la implementación. 

## Asociación del recurso a Content Safety Studio 
En una pestaña independiente del explorador, abra Content Safety Studio e inicie sesión. Se muestra la pantalla Introducción.

1.  Haga clic en el engranaje Configuración en el menú superior derecho.
2.  Seleccione el recurso del servicio de Azure AI que acaba de crear y, a continuación, haga clic en Usar recurso.
3.  Vea el punto de conexión y la clave del recurso, desplazándose si es necesario. Si realiza la comprobación en Azure Portal, verá que este es el punto de conexión y la clave del recurso que muestra que ha asociado correctamente el recurso con Content Safety Studio.
4.  Haga clic en Content Safety Studio para volver a la página principal. En Moderación del contenido de texto, haga clic en Probar.
5.  En Ejecutar una prueba sencilla, haga clic en Contenido seguro. Observe que el texto guardado se muestra en la lista a continuación. 
6.  Haga clic en Ejecutar prueba. 
7.  En el panel Resultados, inspeccione los resultados. Hay cuatro niveles de gravedad que van de "segura" a "alta" y cuatro tipos de contenido dañino. ¿El servicio de inteligencia artificial de Seguridad del contenido considera que este ejemplo es aceptable o no? 
8.  Ahora pruebe otro ejemplo. Seleccione el texto de Contenido violento con errores ortográficos. Compruebe que el contenido aparece en el cuadro siguiente.
9.  Haga clic en Ejecutar prueba e inspeccione los resultados en el panel Resultados a continuación. ¿Es aceptable este ejemplo? Si no es así, ¿por qué?

Puede ejecutar pruebas en todos los ejemplos proporcionados y, a continuación, inspeccionar los resultados.

Cuando haya terminado, elimine el recurso de Servicios de Azure AI en Azure Portal. 
