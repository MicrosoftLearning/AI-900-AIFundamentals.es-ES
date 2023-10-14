---
lab:
  title: Explorar servicios de Azure AI
---

> **Importante**
> **El laboratorio de Anomaly Detector ha quedado en desuso y se ha reemplazado por la actualización siguiente.**

Los servicios de Azure AI ayudan a los usuarios a crear aplicaciones de IA con API y modelos precompilados y personalizados. En este ejercicio, echará un vistazo a uno de los servicios, Seguridad del contenido de Azure AI, en Content Safety Studio. 

Content Safety Studio permite explorar cómo se puede moderar el contenido de texto e imagen. Puede ejecutar pruebas en texto o imágenes de ejemplo y obtener una puntuación de gravedad que va de "segura" a "alta" para cada categoría. En este ejercicio de laboratorio, creará un recurso de servicio único en Content Safety Studio y probará sus funcionalidades. 

> **Nota** El objetivo de este ejercicio es obtener una idea general de cómo se aprovisionan y usan los servicios de Azure AI. Content Safety se usa como ejemplo, pero no se espera que obtenga un conocimiento completo de la seguridad del contenido en este ejercicio.

## Navegación por Content Safety Studio 

![Captura de pantalla de la página de aterrizaje de Content Safety Studio.](./media/content-safety/content-safety-getting-started.png)


1. Abra [Content Safety Studio](https://contentsafety.cognitive.azure.com?azure-portal=true). Si no ha iniciado sesión, deberá iniciar sesión. Seleccione **Iniciar sesión** en la parte superior derecha de la pantalla. Use el correo electrónico y la contraseña asociados a la suscripción de Azure para iniciar sesión. 

1. Content Safety Studio está configurado como muchos otros estudios para los servicios de Azure AI. En el menú de la parte superior de la pantalla, haga clic en el icono de la izquierda de *Azure AI*. Verá una lista desplegable de otros estudios diseñados para el desarrollo con servicios de Azure AI. Puede volver a hacer clic en el icono para ocultar la lista.

![Captura de pantalla del menú de Content Safety Studio con una selección de alternancia abierta para cambiar a otros estudios.](./media/content-safety/studio-toggle-icon.png)  

## Asociación de un recurso a Studio 

Antes de usar Studio, debe asociar un recurso de servicios de Azure AI con Studio. Dependiendo de Studio, puede encontrar que necesita un recurso de servicio único específico o puede usar un recurso general de varios servicios. En el caso de Content Safety Studio, puede usar el servicio mediante la creación de un recurso de *Content Safety* de un solo servicio o un recurso general de varios servicios de *Servicios de Azure AI*. En los pasos siguientes, crearemos un recurso Content Safety de un solo servicio. 

1. En la parte superior derecha de la pantalla, haga clic en el icono **Configuración**. 

![Captura de pantalla del icono de Configuración en la parte superior derecha de la pantalla, junto a la campana, el signo de interrogación y los iconos de sonrisa.](./media/content-safety/settings-toggle.png)

1. En la página **Configuración**, verá una pestaña *Directorio* y una pestaña *Recurso*. En la pestaña *Recurso*, seleccione **Crear un nuevo recurso**. Esto le llevará a la página para crear un recurso en Azure Portal.

> **Nota** La pestaña *Directorio* permite a los usuarios seleccionar directorios diferentes desde los que podrá crear recursos. No es necesario cambiar su configuración a menos que desee usar otro directorio. 

![Captura de pantalla de dónde seleccionar crear un nuevo recurso en la página de configuración de Content Safety Studio.](./media/content-safety/create-new-resource-from-studio.png)

1. En la página *Crear Content Safety* de [Azure Portal](https://portal.azure.com?auzre-portal=true), debe configurar varios detalles para crear el recurso. Configúrelo con los valores siguientes:
    - **Suscripción**: *su suscripción a Azure*.
    - **Grupo de recursos**: *cree o seleccione un grupo de recursos con un nombre único*.
    - **Región**: *elija cualquier región disponible*.
    - **Nombre**: *escriba un nombre único*.
    - **Plan de tarifa**: F0 gratis.

1. Seleccione **Revisar y crear** y revise la configuración. Seleccione **Crear**. La pantalla indicará cuándo se ha completado la implementación. 

*¡Felicidades! Acaba de crear o aprovisionar un recurso de servicios de Azure AI. El que ha aprovisionado en particular es un recurso de Content Safety de un solo servicio.*

1. Cuando finalice la implementación, abra una nueva pestaña y vuelva a [Content Safety Studio](https://contentsafety.cognitive.azure.com?azure-portal=true). 

1. Vuelva a seleccionar el icono **Configuración** en la parte superior derecha de la pantalla. Esta vez debería ver que el recurso recién creado se ha agregado a la lista.  

1. En la página Configuración de Content Safety Studio, seleccione el recurso del servicio de Azure AI que acaba de crear y haga clic en **Usar recurso** en la parte inferior de la pantalla. Volverá a la página principal de Studio. Ahora puede empezar a usar Studio con el recurso recién creado.

## Probar la moderación de texto en Content Safety Studio

1. En la página principal de Content Safety Studio, en *Ejecutar pruebas de moderación*, vaya al cuadro **Moderar contenido de texto**  y haga clic en **Probar**.
1. En Ejecutar una prueba sencilla, haga clic en **Contenido seguro**. Observe que el texto guardado se muestra en la lista a continuación. 
1. Haga clic en **Ejecutar prueba**. La ejecución de una prueba llama al modelo de aprendizaje profundo de Content Safety Service. El modelo de aprendizaje profundo ya se ha entrenado para reconocer contenido no seguro.
1. En el panel *Resultados*, inspeccione los resultados. Hay cuatro niveles de gravedad que van de "segura" a "alta" y cuatro tipos de contenido dañino. ¿El servicio de inteligencia artificial de Content Safety considera que este ejemplo es aceptable o no? Lo que es importante tener en cuenta es que los resultados están dentro de un intervalo de confianza. Un modelo bien entrenado, como uno de los modelos estándar de Azure AI, puede devolver resultados que tengan una alta probabilidad de coincidir con los resultados de etiquetas aplicadas por un humano. Cada vez que ejecute una prueba, vuelva a llamar al modelo. 
1. Ahora pruebe otro ejemplo. Seleccione el texto de Contenido violento con errores ortográficos. Compruebe que el contenido aparece en el cuadro siguiente.
1. Haga clic en **Ejecutar prueba** y vuelva a inspeccionar los resultados en el panel Resultados. 

Puede ejecutar pruebas en todos los ejemplos proporcionados y, a continuación, inspeccionar los resultados.

## Consultar las claves y el punto de conexión

Estas funcionalidades probadas se pueden programar en todo tipo de aplicaciones. Las claves y el punto de conexión que se usan para el desarrollo de aplicaciones se pueden encontrar tanto en Content Safety Studio como en Azure Portal. 

1. En Content Safety Studio, vuelva a la página **Configuración**, con la pestaña *Recursos* seleccionada. Busque el recurso que ha usado. Desplácese por aquí para ver el punto de conexión y la clave del recurso. 
1. En Azure Portal, verá que se trata del *mismo* punto de conexión y claves *diferentes* para el recurso. Para comprobarlo, vaya a [Azure Portal](https://portal.azure.com?auzre-portal=true). Busque *Content Safety* en la barra de búsqueda superior. Encuentre el recurso y haga clic en él. En el menú de la izquierda, busque en *Administración de recursos* para *Claves y puntos de conexión*. Seleccione **Claves y puntos de conexión** para ver el punto de conexión y las claves del recurso. 

Una vez que haya terminado, puede eliminar el recurso Content Safety de Azure Portal. Eliminar el recurso es una manera de reducir los costos que se acumulan cuando el recurso existe en la suscripción. Para ello, vaya a la página **Información general** del recurso Content Safety. En la parte superior de la pantalla, seleccione **Eliminar**. 
