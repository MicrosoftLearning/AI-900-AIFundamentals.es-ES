---
lab:
  title: Exploración de la detección de objetos
---

# <a name="explore-object-detection"></a>Exploración de la detección de objetos

> **Nota** Para completar este laboratorio, necesitará una [suscripción de Azure](https://azure.microsoft.com/free?azure-portal=true) en la que tenga acceso de administrador.

La *detección de objetos* es una forma de visión informática en la que se entrena un modelo de Machine Learning para clasificar instancias individuales de objetos en una imagen e indicar un *cuadro de límite* que marque su ubicación. Puede pensar en esto como una progresión de la *clasificación de imágenes* (en la que el modelo responde a la pregunta "¿de qué es esto una imagen?") para la compilación de soluciones en las que podemos preguntar al modelo "¿qué objetos hay en esta imagen y dónde están?".

Por ejemplo, una tienda de comestibles podría usar un modelo de detección de objetos para implementar un sistema de finalización de la compra automatizado que examine una cinta transportadora mediante una cámara y puede identificar elementos específicos sin necesidad de colocar cada artículo en la cinta y examinarlos individualmente.

El servicio cognitivo **Custom Vision** de Microsoft Azure proporciona una solución basada en la nube para la creación y publicación de modelos de detección de objetos personalizados. En Azure, puede usar el servicio Custom Vision para entrenar un modelo de clasificación de imágenes basado en imágenes existentes. Hay dos elementos para crear una solución de clasificación de imágenes. En primer lugar, debe entrenar un modelo para reconocer clases diferentes mediante imágenes existentes. Después, una vez entrenado el modelo, debe publicarlo como un servicio que pueden consumir las aplicaciones.

Para probar las funcionalidades del servicio Custom Vision para detectar objetos en imágenes, usaremos una sencilla aplicación de línea de comandos que se ejecuta en Cloud Shell. Los mismos principios y funcionalidad se aplican en soluciones reales, como sitios web o aplicaciones de teléfono.

## <a name="create-a-cognitive-services-resource"></a>Creación de un recurso de *Cognitive Services*

Para usar el servicio Computer Vision, puede crear un recurso de **Computer Vision** o un recurso de **Cognitive Services**.

> **Nota** No todos los recursos están disponibles en todas las regiones. Tanto si crea un recurso de Custom Vision como de Cognitive Services, para acceder a los servicios de Custom Vision, solo se pueden usar los recursos creados en [determinadas regiones](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services). Para simplificar el proceso, se selecciona previamente una región en las instrucciones de configuración siguientes.

Cree un recurso de **Cognitive Services** en la suscripción de Azure.

1. En otra pestaña del explorador, abra Azure Portal en [https://portal.azure.com](https://portal.azure.com?azure-portal=true) e inicie sesión con su cuenta Microsoft.

1. Haga clic en el botón **&amp;#65291;Crear un recurso**, busque *Cognitive Services* y cree un recurso de **Cognitive Services** con la siguiente configuración:
    - **Suscripción**: *su suscripción a Azure*.
    - **Grupo de recursos**: *cree o seleccione un grupo de recursos con un nombre único*.
    - **Región**: Este de EE. UU.
    - **Nombre**: *escriba un nombre único*.
    - **Plan de tarifa**: estándar S0
    - **Al marcar esta casilla, confirmo que he leído y comprendido todos los términos que aparecen a continuación**: Seleccionado.

1. Revise y cree el recurso y espere a que finalice la implementación. A continuación, vaya al recurso implementado.

1. Vea la página **Claves y punto de conexión** del recurso de Cognitive Services. Necesitará el punto de conexión y las claves para conectarse desde las aplicaciones cliente.

## <a name="create-a-custom-vision-project"></a>Creación de un proyecto de Custom Vision

Para entrenar un modelo de detección de objetos, debe crear un proyecto de Custom Vision basado en el recurso de entrenamiento. Para hacerlo, debe usar el portal de Custom Vision.

1. En una nueva pestaña del explorador, abra el portal de Custom Vision en [https://customvision.ai](https://customvision.ai?azure-portal=true) e inicie sesión con la cuenta de Microsoft asociada a su suscripción de Azure.

1. Cree un proyecto con la siguiente configuración:
    - **Nombre**: Detección de comestibles
    - **Descripción**: detección de objetos de comestibles.
    - **Recurso**: *el recurso creado anteriormente*
    - **Tipos de proyecto**: detección de objetos
    - **Dominios**: General

1. Espere a que el proyecto se cree y abra en el explorador.

## <a name="add-and-tag-images"></a>Adición y etiquetado de imágenes

Para entrenar un modelo de detección de objetos, debe cargar imágenes que contengan las clases con las que desea que se identifique el modelo y etiquetarlas para indicar cuadros de límite para cada instancia de objeto.

1. Descargue y extraiga las imágenes de entrenamiento de https://aka.ms/fruit-objects. La carpeta extraída contiene una colección de imágenes de fruta.

1. En el portal de Custom Vision [https://customvision.ai](https://customvision.ai?azure-portal=true), asegúrese de que trabaja en el proyecto de detección de objetos _Detección de comestibles_. A continuación, seleccione **Agregar imágenes** y cargue todas las imágenes de la carpeta extraída.

    ![Cargue imágenes descargadas haciendo clic en Agregar imágenes.](media/create-object-detection-solution/fruit-upload.jpg)

1. Una vez cargadas las imágenes, seleccione la primera para abrirla.

1. Mantenga presionado el mouse sobre cualquier objeto de la imagen hasta que se muestre una región detectada automáticamente como la imagen siguiente. A continuación, seleccione el objeto y, si es necesario, cambie el tamaño de la región para rodearlo.

    ![Región predeterminada de un objeto](media/create-object-detection-solution/object-region.jpg)

    Como alternativa, simplemente puede arrastrar alrededor del objeto para crear una región.

1. Cuando la región rodea el objeto, agregue una nueva etiqueta con el tipo de objeto adecuado (*manzana*, *plátano* o *naranja*) como se muestra aquí:

    ![Objeto etiquetado en una imagen](media/create-object-detection-solution/object-tag.jpg)

1. Seleccione y etiquete el objeto de las otras en la imagen, cambiando el tamaño de las regiones y agregando nuevas etiquetas según sea necesario.

    ![Dos objetos etiquetados en una imagen](media/create-object-detection-solution/object-tags.jpg)

1. Use el vínculo **>** de la derecha para pasar a la siguiente imagen y etiquetar sus objetos. A continuación, siga trabajando en toda la colección de imágenes, etiquetando cada manzana, plátano y naranja.

1. Cuando haya terminado de etiquetar la última imagen, cierre el editor **Detalles de la imagen** y, en la página **Imágenes de entrenamiento**, en **Etiquetas**, seleccione **Etiquetado** para ver todas sus imágenes etiquetadas:

    ![Imágenes etiquetadas en un proyecto](media/create-object-detection-solution/tagged-images.jpg)

## <a name="train-and-test-a-model"></a>Entrenamiento y prueba de un modelo

Ahora que ha etiquetado las imágenes del proyecto, está listo para entrenar un modelo.

1. En el proyecto de Custom Vision, haga clic en **Entrenar** para entrenar un modelo de detección de objetos mediante las imágenes etiquetadas. Seleccione la opción **Entrenamiento rápido**.

1. Espere a que se complete el entrenamiento (puede tardar unos diez minutos) y, después, revise las métricas de rendimiento *Precisión*, *Coincidencia* y *Asignación*, que miden la corrección de la precisión del modelo de detección de objetos y deben ser todas altas.

1. En la esquina superior derecha de la página, haga clic en **Prueba rápida** y, después, en el cuadro **Dirección URL de imagen**, escriba `https://aka.ms/apple-orange` y vea la predicción que se genera. A continuación, cierre la ventana **Prueba rápida**.

## <a name="publish-the-object-detection-model"></a>Publicación del modelo de detección de objetos

Ahora está listo para publicar el modelo entrenado y usarlo desde una aplicación cliente.

1. Haga clic en **&#128504; Publicar** para publicar el modelo entrenado con la configuración siguiente:
    - **Nombre del modelo**: detect-produce
    - **Recurso de predicción**: *el recurso creado anteriormente*.

1. Después de la publicación, haga clic en *Dirección URL de predicción* (&#127760;) para ver la información necesaria para usar el modelo publicado. Más adelante, necesitará la dirección URL y los valores Prediction-Key adecuados para obtener una predicción de una dirección URL de imagen, por lo que debe mantener este cuadro de diálogo abierto y continuar con la siguiente tarea.

## <a name="run-cloud-shell"></a>Ejecución de Cloud Shell

Para probar las funcionalidades del servicio Custom Vision, usaremos una sencilla aplicación de línea de comandos que se ejecuta en Cloud Shell en Azure.

1. En Azure Portal, seleccione el botón **[>_]** (*Cloud Shell*) situado en la parte superior de la página, a la derecha del cuadro de búsqueda. Se abre un panel de Cloud Shell en la parte inferior del portal. 

    ![Inicio de Cloud Shell haciendo clic en el icono situado a la derecha del cuadro de búsqueda superior](media/create-object-detection-solution/powershell-portal-guide-1.png)

1. La primera vez que abra Cloud Shell, es posible que se le pida que elija el tipo de shell que desea usar (*Bash* o *PowerShell*). Seleccione **PowerShell**. Si no ve esta opción, omita el paso.  

1. Si se le pide que cree almacenamiento para Cloud Shell, asegúrese de que se haya especificado su suscripción y seleccione **Crear almacenamiento**. A continuación, espere un minuto más o menos a que se cree el almacenamiento.

    ![Para crear el almacenamiento, haga clic en Confirmar.](media/create-object-detection-solution/powershell-portal-guide-2.png)

1. Asegúrese de que el tipo de shell indicado en la parte superior izquierda del panel de Cloud Shell se cambia a *PowerShell*. Si es *Bash*, cambie a *PowerShell* mediante el menú desplegable.

    ![Cómo encontrar el menú desplegable de la izquierda para cambiar a PowerShell](media/create-object-detection-solution/powershell-portal-guide-3.png) 

1. Espere a que se inicie PowerShell. Debería ver la siguiente pantalla en Azure Portal:  

    ![Espere a que se inicie PowerShell.](media/create-object-detection-solution/powershell-prompt.png) 

## <a name="configure-and-run-a-client-application"></a>Configuración y ejecución de una aplicación cliente

Ahora que tiene un modelo personalizado, puede ejecutar una sencilla aplicación cliente que use el servicio Custom Vision para detectar objetos en una imagen.

1. En el shell de comandos, escriba el comando siguiente para descargar la aplicación de ejemplo y guárdela en una carpeta llamada ai-900.

    ```PowerShell
    git clone https://github.com/MicrosoftLearning/AI-900-AIFundamentals ai-900
    ```

    >**Nota** Si ya usó este comando en otro laboratorio para clonar el repositorio *ai-900*, puede omitir este paso.

1. Los archivos se descargan en una carpeta llamada **ai-900**. Ahora queremos ver todos los archivos del almacenamiento de Cloud Shell y trabajar con ellos. Escriba el siguiente comando en el shell:

    ```PowerShell
    code .
    ```

    Observe cómo se abre un editor como el de la imagen siguiente: 

    ![Editor de código.](media/create-object-detection-solution/powershell-portal-guide-4.png)

1. En el panel **Archivos** de la izquierda, expanda **ai-900** y seleccione **detect-objects.ps1**. Este archivo contiene algún código que usa el servicio Custom Vision para detectar objetos en una imagen, como se muestra aquí:

    ![Editor que contiene código para detectar elementos en una imagen](media/create-object-detection-solution/detect-image-code.png)

1. No se preocupe demasiado por los detalles del código, lo importante es que necesita la dirección URL de predicción y la clave para el modelo de Custom Vision al usar una dirección URL de imagen. 

    Obtenga la *URL predictiva* en el cuadro de diálogo de su proyecto Custom Vision. 

    >**Nota** Recuerde que ha revisado la *URL predictiva* tras publicar el modelo de clasificación de imágenes. Para encontrar la *URL predictiva*, diríjase a la pestaña **Rendimiento** de su proyecto y haga clic en **URL predictiva** (si la pantalla está comprimida, puede que solo aparezca un icono de globo). Aparecerá un cuadro de diálogo. Copie la URL de **Si tiene una URL de imagen**. Péguela en el editor de código, reemplazando **YOUR_PREDICTION_URL**. 

    Con el mismo cuadro de diálogo, obtenga la *clave de predicción*. Copie la clave de predicción que se muestra después de *establecer Prediction-Key encabezado en*. Péguela en el editor de código, reemplazando el valor de marcador de posición **YOUR_PREDICTION_URL**. 

    ![Captura de pantalla de la URL predictiva.](media/create-object-detection-solution/find-prediction-url.png)

    Después de pegar los valores de dirección URL de predicción y clave de predicción, las dos primeras líneas de código deben tener un aspecto similar al siguiente:

    ```PowerShell
    $predictionUrl="https..."
    $predictionKey ="1a2b3c4d5e6f7g8h9i0j...."
    ```

1. En la parte superior derecha del panel del editor, use el botón **...** para abrir el menú y seleccione **Guardar** para guardar los cambios. A continuación, vuelva a abrir el menú y seleccione **Cerrar editor**.

    Usará la aplicación cliente de ejemplo para detectar objetos en esta imagen:

    ![Imagen de una fruta](media/create-object-detection-solution/produce.jpg)

1. En el panel de PowerShell, escriba el siguiente comando para ejecutar el código:

    ```PowerShell
    cd ai-900
    ./detect-objects.ps1 
    ```

1. Revise la predicción, que debería ser *manzana naranja plátano*.

## <a name="learn-more"></a>Más información

Esta sencilla aplicación muestra solo algunas de las funcionalidades del servicio Custom Vision. Para más información sobre lo que puede hacer con este servicio, consulte la [página de Custom Vision](https://azure.microsoft.com/services/cognitive-services/custom-vision-service/).
