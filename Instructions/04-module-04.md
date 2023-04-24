---
lab:
  title: Exploración del análisis de texto
  module: Module 4 - Natural Language Processing (NLP)
---

# <a name="explore-text-analytics"></a>Exploración del análisis de texto

> **Nota** Para completar este laboratorio, necesitará una [suscripción de Azure](https://azure.microsoft.com/free?azure-portal=true) en la que tenga acceso de administrador.

El procesamiento de lenguaje natural (NLP) es una rama de la inteligencia artificial (IA) que se ocupa del lenguaje escrito y hablado. Puede usar NLP para crear soluciones que extraigan el significado semántico del texto o la voz, o que formule respuestas significativas en lenguaje natural.

Microsoft Azure *Cognitive Services* incluye las funcionalidades de análisis de texto del servicio *Language*, que proporciona algunas funcionalidades de NLP integradas, incluida la identificación de frases clave en el texto y la clasificación del texto en función de las opiniones.

Por ejemplo, supongamos que la organización ficticia *Margie's Travel* anima a los clientes a enviar reseñas sobre sus estancias en hoteles. Puede usar el servicio Language para resumir las reseñas mediante la extracción de frases clave, determinar qué revisiones son positivas y cuáles negativas, o analizar el texto de reseña en busca de menciones de entidades conocidas como ubicaciones o personas.

Para probar las funcionalidades del servicio Language, usaremos una sencilla aplicación de línea de comandos que se ejecuta en Cloud Shell. Los mismos principios y funcionalidad se aplican en soluciones reales, como sitios web o aplicaciones de teléfono.

## <a name="create-a-cognitive-services-resource"></a>Creación de un recurso de *Cognitive Services*

Puede usar el servicio Language creando un recurso de **Language ** o un recurso de **Cognitive Services**.

Si aún no lo ha hecho, cree un recurso de **Cognitive Services** en la suscripción de Azure.

1. En otra pestaña del explorador, abra Azure Portal en [https://portal.azure.com](https://portal.azure.com?azure-portal=true) e inicie sesión con su cuenta Microsoft.

1. Seleccione el botón **&#65291;Crear un recurso**, busque *Cognitive Services* y cree un recurso de **Cognitive Services** con la siguiente configuración:
    - **Suscripción**: *su suscripción a Azure*.
    - **Grupo de recursos**: *cree o seleccione un grupo de recursos con un nombre único*.
    - **Región**: *elija cualquier región disponible*.
    - **Nombre**: *escriba un nombre único*.
    - **Plan de tarifa**: estándar S0
    - **Al marcar esta casilla, confirmo que he leído y comprendido todos los términos que aparecen a continuación**: Seleccionado.

1. Revise y cree el recurso.

### <a name="get-the-key-and-endpoint-for-your-cognitive-services-resource"></a>Obtención de la clave y la ubicación del recurso de Cognitive Services

1. Espere a que la implementación finalice. Después, vaya al recurso de Cognitive Services y, en la página **Información general**, seleccione el vínculo para administrar las claves del servicio. Necesitará el punto de conexión y las claves para conectarse al recurso de Cognitive Services desde las aplicaciones cliente.

1. Vea la página **Claves y punto de conexión** del recurso. Necesitará la **clave** y el **punto de conexión** para conectarse desde las aplicaciones cliente.

## <a name="run-cloud-shell"></a>Ejecución de Cloud Shell

Para probar las funcionalidades de análisis de texto del servicio Language, usaremos una sencilla aplicación de línea de comandos que se ejecuta en Cloud Shell en Azure.

1. En Azure Portal, seleccione el botón **[>_]** (*Cloud Shell*) situado en la parte superior de la página, a la derecha del cuadro de búsqueda. Se abre un panel de Cloud Shell en la parte inferior del portal.

    ![Inicio de Cloud Shell haciendo clic en el icono situado a la derecha del cuadro de búsqueda superior](media/analyze-text-language-service/powershell-portal-guide-1.png)

1. La primera vez que abra Cloud Shell, es posible que se le pida que elija el tipo de shell que desea usar (*Bash* o *PowerShell*). Seleccione **PowerShell**. Si no ve esta opción, omita el paso.  

1. Si se le pide que cree almacenamiento para Cloud Shell, asegúrese de que se haya especificado su suscripción y seleccione **Crear almacenamiento**. A continuación, espere un minuto más o menos a que se cree el almacenamiento.

    ![Para crear el almacenamiento, haga clic en Confirmar.](media/analyze-text-language-service/powershell-portal-guide-2.png)

1. Asegúrese de que el tipo de shell indicado en la parte superior izquierda del panel de Cloud Shell se cambia a *PowerShell*. Si es *Bash*, cambie a *PowerShell* mediante el menú desplegable.

    ![Cómo encontrar el menú desplegable de la izquierda para cambiar a PowerShell](media/analyze-text-language-service/powershell-portal-guide-3.png)

1. Espere a que se inicie PowerShell. Debería ver la siguiente pantalla en Azure Portal:  

    ![Espere a que se inicie PowerShell.](media/analyze-text-language-service/powershell-prompt.png)

## <a name="configure-and-run-a-client-application"></a>Configuración y ejecución de una aplicación cliente

Ahora que tiene un modelo personalizado, puede ejecutar una sencilla aplicación cliente que use el servicio Language.

1. En el shell de comandos, escriba el siguiente comando para descargar la aplicación de ejemplo y guárdela en una carpeta llamada ai-900.

    ```PowerShell
    git clone https://github.com/MicrosoftLearning/AI-900-AIFundamentals ai-900
    ```

    >**Consejo** Si ya usó este comando en otro laboratorio para clonar el repositorio *ai-900*, puede omitir este paso.

1. Los archivos se descargan en una carpeta llamada **ai-900**. Ahora queremos ver todos los archivos del almacenamiento de Cloud Shell y trabajar con ellos. Escriba el siguiente comando en el shell:

     ```PowerShell
    code .
    ```

    Observe cómo se abre un editor como el de la imagen siguiente:

    ![Editor de código.](media/analyze-text-language-service/powershell-portal-guide-4.png)

1. En el panel **Archivos** de la izquierda, expanda **ai-900** y seleccione **analyze-text.ps1**. Este archivo contiene código que usa el servicio Language:

    ![Editor que contiene código para usar el servicio Language](media/analyze-text-language-service/analyze-text-code.png)

1. No se preocupe demasiado por los detalles del código. En Azure Portal, vaya al recurso de Cognitive Services. Luego, seleccione la página **Claves y puntos de conexión** en el panel izquierdo. Copie la clave y el punto de conexión de la página y péguelos en el editor de código, pero reemplace los valores de marcador de posición **YOUR_KEY** y **YOUR_ENDPOINT**, respectivamente.

    > **Consejo** Es posible que tenga que usar la barra de separación para ajustar el área de pantalla mientras trabaja con los paneles **Claves y punto de conexión** y **Editor**.

    ![Busque la pestaña clave y punto de conexión en el panel izquierdo del recurso de Cognitive Services.](media/analyze-text-language-service/key-endpoint-support.png)

    Después de reemplazar los valores de la clave y punto de conexión, las primeras líneas del código deben tener un aspecto similar al siguiente:

    ```PowerShell
    $key="1a2b3c4d5e6f7g8h9i0j...."
    $endpoint="https..."
    ```

1. En la parte superior derecha del panel del editor, use el botón **...** para abrir el menú y seleccione **Guardar** para guardar los cambios. A continuación, vuelva a abrir el menú y seleccione **Cerrar editor**.

    La aplicación cliente de ejemplo utilizará el servicio Language de Cognitive Services para detectar el lenguaje, extraer frases clave, determinar opiniones y extraer entidades conocidas para las reseñas.

1. En Cloud Shell, escriba el siguiente comando para ejecutar el código:

    ```PowerShell
    cd ai-900
    ./analyze-text.ps1 review1.txt
    ```

    Revisará este texto:

    >Buen hotel y personal The Royal Hotel, Londres, Reino Unido 2/3/2018 Habitaciones limpias, buen servicio, excelente ubicación cerca del Palacio de Buckingham y la Abadía de Westminster, etc. Hemos disfrutado mucho de nuestra estancia. El patio es muy tranquilo y fuimos a un restaurante que forma parte del mismo grupo y es indio (costa oeste, así que mucho pescado) con una estrella Michelin. Pedimos el menú de degustación, que era fabuloso. Las habitaciones estaban muy bien equipadas con cocina, salón, dormitorio y un enorme baño. Muy recomendable.

1. Revise el resultado.

1. En el panel de PowerShell, escriba el siguiente comando para ejecutar el código:

    ```PowerShell
    ./analyze-text.ps1 review2.txt
    ```

    Revisará este texto:

    >Hotel destartalado con servicios deficientes The Royal Hotel, Londres, Reino Unido 6/5/2018 Hotel antiguo (lleva funcionando desde la década de 1950) con mobiliario normalito en las habitaciones; empieza a quedarse un poco antiguo y requiere un cambio. Internet no funcionaba y tuve que ir a una de sus oficinas para facturar el vuelo de regreso. La página web dice que está cerca del British Museum, pero está demasiado lejos para ir andando.

1. Revise el resultado.

1. En el panel de PowerShell, escriba el siguiente comando para ejecutar el código:

    ```PowerShell
    ./analyze-text.ps1 review3.txt
    ```

    Revisará este texto:

    >Buena ubicación y personal servicial, pero en una carretera muy transitada.
    The Lombard Hotel, San Francisco, EE. UU. 16/08/2018 Nos alojamos aquí en agosto después de leer las reseñas. Estuvimos muy contentos con la ubicación, justo detrás de Chestnut Street, una zona cosmopolita y de moda con muchos restaurantes para elegir. El distrito de la Marina es encantador para pasear, casas muy interesantes. Es muy recomendable caminar hasta el Museo de Bellas Artes de San Francisco y el puerto deportivo para disfrutar de una buena vista del puente Golden Gate y de la ciudad. En una ruta de autobús y fácil de llegar al centro. Las habitaciones estaban limpias con mucho espacio y el personal era amable y servicial. El único aspecto negativo fue el ruido de la calle Lombard, así que pide una habitación lo más alejada posible del ruido del tráfico.

1. Revise el resultado.

1. En el panel de PowerShell, escriba el siguiente comando para ejecutar el código:

    ```PowerShell
    ./analyze-text.ps1 review4.txt
    ```

    Revisará este texto:

    >Muy ruidoso y las habitaciones son diminutas The Lombard Hotel, San Francisco, USA 5/9/2018 El hotel está situado en la calle Lombard, que es una calle de SEIS carriles muy transitada directamente desde el puente Golden Gate. Tráfico desde la mañana hasta altas horas de la noche, especialmente los fines de semana. El ruido no sería tan malo si las habitaciones estuvieran mejor aisladas, pero no lo están. Tuve que ponerme algodones en los oídos para poder dormir; estaba demasiado cansado para disfrutar de la ciudad al día siguiente. Las habitaciones son DIMINUTAS. Escogí la habitación porque tenía dos camas queen size, pero la habitación apenas tenía espacio para acomodarlas. Somos una familia de cuatro personas y nos sentimos muy apretados. Con todo esto, las habitaciones están limpias y han hecho un esfuerzo por actualizarlas. El hotel está en el distrito de Marina con muchos buenos lugares para comer, a poca distancia de Presidio. Puede ser un buen hotel para adultos jóvenes que se acuestan tarde y disponen de un buen presupuesto.

1. Revise el resultado.

## <a name="learn-more"></a>Más información

Esta sencilla aplicación muestra solo algunas de las funcionalidades del servicio Language. Para más información sobre lo que puede hacer con este servicio, consulte la [página del servicio Language](https://azure.microsoft.com/services/cognitive-services/language-service/).
