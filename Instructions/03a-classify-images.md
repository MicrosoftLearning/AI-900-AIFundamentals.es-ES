---
lab:
  title: Exploración de la clasificación de imágenes
---

# Exploración de la clasificación de imágenes

El servicio cognitivo *Computer Vision* proporciona modelos útiles creados previamente para trabajar con imágenes, pero a menudo necesitará entrenar su propio modelo para visión informática. Por ejemplo, supongamos que una organización de conservación de la fauna silvestre quiere realizar un seguimiento del avistamiento de animales mediante cámaras sensibles al movimiento. Las imágenes capturadas por las cámaras podrían utilizarse para verificar la presencia de especies particulares en una zona determinada y ayudar con los esfuerzos de conservación de las especies en peligro de extinción. Para ello, la organización se beneficiaría de un modelo de *clasificación de imágenes* entrenado para identificar diferentes especies de animales en las fotografías capturadas.

En Azure, puede usar el servicio cognitivo ***Custom Vision*** para entrenar un modelo de clasificación de imágenes basado en imágenes existentes. Hay dos elementos para crear una solución de clasificación de imágenes. En primer lugar, debe entrenar un modelo para reconocer clases diferentes mediante imágenes existentes. Después, una vez entrenado el modelo, debe publicarlo como un servicio que pueden consumir las aplicaciones.

Para probar las capacidades del servicio Custom Vision, usaremos una aplicación de línea de comandos sencilla que se ejecuta en Cloud Shell. Los mismos principios y funcionalidad se aplican en soluciones reales, como sitios web o aplicaciones móviles.

## Antes de comenzar

Para completar este laboratorio, necesitará una [suscripción de Azure](https://azure.microsoft.com/free?azure-portal=true) en la que tenga acceso administrativo.

## Creación de un recurso de *Cognitive Services*

Para usar el servicio Computer Vision, puede crear un recurso de **Computer Vision** o un recurso de **Cognitive Services**.

>**Nota** No todos los recursos están disponibles en todas las regiones. Tanto si crea un recurso de Custom Vision como de Cognitive Services, para acceder a los servicios de Custom Vision, solo se pueden usar los recursos creados en [determinadas regiones](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services). Para simplificar el proceso, se selecciona previamente una región en las instrucciones de configuración siguientes.

Cree un recurso de **Cognitive Services** en la suscripción de Azure.

1. Abra Azure Portal en [https://portal.azure.com](https://portal.azure.com?azure-portal=true) e inicie sesión con su cuenta de Microsoft.

1. Haga clic en el botón **&amp;#65291;Crear un recurso**, busque *Cognitive Services* y cree un recurso de **Cognitive Services** con la siguiente configuración:
    - **Suscripción**: *su suscripción a Azure*.
    - **Grupo de recursos**: *cree o seleccione un grupo de recursos con un nombre único*.
    - **Región**: Este de EE. UU.
    - **Nombre**: *escriba un nombre único*.
    - **Plan de tarifa**: estándar S0
    - **Al marcar esta casilla, confirmo que he leído y comprendido todos los términos que aparecen a continuación**: Seleccionado.

1. Revise y cree el recurso y espere a que finalice la implementación. A continuación, vaya al recurso implementado.

1. Vea la página **Claves y punto de conexión** del recurso de Cognitive Services. Necesitará el punto de conexión y las claves para conectarse desde las aplicaciones cliente.

## Creación de un proyecto de Custom Vision

Para entrenar un modelo de detección de objetos, debe crear un proyecto de Custom Vision basado en el recurso de entrenamiento. Para hacerlo, debe usar el portal de Custom Vision.

1. Descargue y extraiga las imágenes de entrenamiento de [https://aka.ms/animal-images](https://aka.ms/animal-images). Estas imágenes se proporcionan en una carpeta comprimida, que cuando se extrae contiene subcarpetas denominadas **elefante**, **jirafa** y **león**.

1. En una nueva pestaña del explorador, examine el portal de Custom Vision en [https://customvision.ai](https://customvision.ai?azure-portal=true). Si se le solicita, inicie sesión con la cuenta de Microsoft asociada a su suscripción de Azure y acepte las Condiciones del servicio

1. En el portal de Custom Vision, cree un nuevo proyecto con la siguiente configuración:

    - **Nombre**: Identificación de animales
    - **Descripción**: Clasificación de imágenes de animales
    - **Recurso**: *El recurso de Cognitive Services o Custom Vision que creó anteriormente*
    - **Tipos de proyecto**: Clasificación
    - **Tipos de clasificación**: multiclase (etiqueta única por imagen)
    - **Dominios**: General \[A2]

1. Haga clic en **Agregar imágenes** y seleccione todos los archivos de la carpeta **elefante** extraídos anteriormente. Después, cargue los archivos de imágenes e incluya la etiqueta *elefante*, como en este ejemplo:

    ![Captura de pantalla de la interfaz de carga de imágenes.](media/create-image-classification-system/upload-elephants.png)

1. Use el botón **Agregar imágenes** ([+]) para cargar las imágenes en la carpeta **jirafa** con la etiqueta *jirafa* y las imágenes de la carpeta **león** con la etiqueta *león*.

1. Explore las imágenes cargadas en el proyecto de Custom Vision, debería tener 17 imágenes de cada clase, como en este ejemplo:

    ![Captura de pantalla de las imágenes de entrenamiento etiquetadas en el portal de Custom Vision.](media/create-image-classification-system/animal-training-images.png)

1. En el proyecto de Custom Vision, sobre las imágenes, haga clic en **Train** para entrenar un modelo de clasificación con las imágenes etiquetadas. Seleccione la opción **Entrenamiento rápido** y, después, espere a que se complete la iteración de entrenamiento.

    > **Sugerencia**: El entrenamiento puede tardar unos minutos. Mientras espera, consulte [Cómo los selfis de leopardo de las nieves y la inteligencia artificial pueden ayudar a salvar a la especie de la extinción](https://news.microsoft.com/transform/snow-leopard-selfies-ai-save-species/), donde se describe un proyecto real que usa Computer Vision para realizar un seguimiento de los animales en peligro de extinción en su hábitat natural.

1. Una vez entrenado el modelo, compruebe las métricas de rendimiento *Precision*, *Recall* y *AP* (miden la precisión de la predicción del modelo de clasificación y sus valores deberían ser altos).

## Prueba del modelo

Antes de publicar esta iteración del modelo para que las aplicaciones lo usen, debe probarlo.

1. Sobre las métricas de rendimiento, haga clic en **Quick Test**.

1. En el cuadro **Dirección URL de imagen**, escriba `https://aka.ms/giraffe` y haga clic en el botón **imagen de prueba rápida (&#10132;)** .

1. Consulte las predicciones obtenidas del modelo, la puntuación de probabilidad de *jirafa* debería ser la más alta, como en este ejemplo:

    ![Captura de pantalla de la interfaz de prueba rápida.](media/create-image-classification-system/quick-test.png)

1. Cierre la ventana **Quick Test**.

## Publicación del modelo de clasificación de imágenes

Ahora está listo para publicar el modelo entrenado y usarlo desde una aplicación cliente.

1. Haga clic en **&#128504; Publicar** para publicar el modelo entrenado con la configuración siguiente:
    - **Nombre del modelo**: animales
    - **Recurso de predicción**: *El recurso de predicción de Cognitive Services o Custom Vision que creó anteriormente*.

1. Después de la publicación, haga clic en *Dirección URL de predicción* (&#127760;) para ver la información necesaria para usar el modelo publicado.

    ![Captura de pantalla de la URL predictiva.](media/create-image-classification-system/prediction-url.png)

Más adelante, necesitará la dirección URL y los valores Prediction-Key adecuados para obtener una predicción de una dirección URL de imagen, por lo que debe mantener este cuadro de diálogo abierto y continuar con la siguiente tarea.

## Preparación de una aplicación cliente

Para probar las funcionalidades del servicio Custom Vision, usaremos una sencilla aplicación de línea de comandos que se ejecuta en Cloud Shell en Azure.

1. Vuelva a la pestaña del explorador que contiene Azure Portal y seleccione el botón **Cloud Shell** ( **[>_]** ) situado en la parte superior de la página a la derecha del cuadro de búsqueda. Se abre un panel de Cloud Shell en la parte inferior del portal.

    La primera vez que abra Cloud Shell, es posible que se le pida que elija el tipo de shell que desea usar (*Bash* o *PowerShell*). En su caso, seleccione **PowerShell**.

    Si se le pide que cree almacenamiento para Cloud Shell, asegúrese de que se haya seleccionado su suscripción y seleccione **Crear almacenamiento**. A continuación, espere un minuto más o menos a que se cree el almacenamiento.

    Cuando Cloud Shell esté listo, debería tener un aspecto similar al siguiente:
    
    ![Captura de pantalla de Cloud Shell en Azure Portal.](media/create-image-classification-system/cloud-shell.png)

    > *Sugerencia*: Asegúrese de que el tipo de shell indicado en la parte superior izquierda del panel de Cloud Shell es **PowerShell**. Si es *Bash*, cambie a *PowerShell* mediante el menú desplegable.

    Tenga en cuenta que puede cambiar el tamaño de Cloud Shell arrastrando la barra de separación en la parte superior del panel, o usando los iconos **&#8212;** , **&#9723;** y **X** en la parte superior derecha para minimizar, maximizar y cerrar el panel. Para obtener más información sobre el uso de Azure Cloud Shell, consulte la [documentación de Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

2. En el shell de comandos, escriba los siguientes comandos para descargar los archivos de este ejercicio y guárdelos en una carpeta denominada **ai-900** (después de quitar esa carpeta si ya existe).

    ```PowerShell
    rm -r ai-900 -f
    git clone https://github.com/MicrosoftLearning/AI-900-AIFundamentals ai-900
    ```

3. Una vez descargados los archivos, escriba los siguientes comandos para cambiar al directorio **ai-900** y edite el archivo de código para este ejercicio:

    ```PowerShell
    cd ai-900
    code classify-image.ps1
    ```

    Observe cómo se abre un editor como el de la imagen siguiente:

     ![Captura de pantalla del editor de código en Cloud Shell.](media/create-image-classification-system/code-editor.png)

     > **Sugerencia**: Puede usar la barra de separación entre la línea de comandos de Cloud Shell y el editor de código para cambiar el tamaño de los paneles.

4. No se preocupe demasiado por los detalles del código. Lo importante es que comience con algún código para especificar la dirección URL de predicción y la clave del modelo de Custom Vision. Deberá actualizar para que el resto del código use el modelo.

    Obtenga la *Dirección URL de predicción* y la *Clave de predicción* del cuadro de diálogo que dejó abierto en la pestaña del explorador del proyecto de Custom Vision. **Necesita las versiones que debe usar *si tiene una dirección URL de la imagen*.**

    Use estos valores para reemplazar los marcadores de posición **YOUR_PREDICTION_URL** y **YOUR_PREDICTION_KEY** en el archivo de código.

    Después de pegar los valores de dirección URL de predicción y clave de predicción, las dos primeras líneas de código deben tener un aspecto similar al siguiente:

    ```PowerShell
    $predictionUrl="https..."
    $predictionKey ="1a2b3c4d5e6f7g8h9i0j...."
    ```

5. Después de realizar los cambios en las variables del código, presione **CTRL+S** para guardar el archivo. Después, presione **Ctrl+Q** para cerrar el editor de código.

## Prueba de la aplicación cliente

Ahora puede usar la aplicación cliente de ejemplo para clasificar imágenes en función del animal que contienen.

1. En el panel de PowerShell, escriba el siguiente comando para ejecutar el código:

    ```PowerShell
    ./classify-image.ps1 1
    ```

    Este código usa el modelo para clasificar la siguiente imagen:

    ![Fotografía de una jirafa.](media/create-image-classification-system/animal-1.jpg)

1. Revise la predicción, que debería ser **jirafa**.

1. Ahora vamos a probar otra imagen. Ejecute este comando:

    ```PowerShell
    ./classify-image.ps1 2
    ```

    Esta vez se clasifica la siguiente imagen:

    ![Fotografía de un elefante.](media/create-image-classification-system/animal-2.jpg)

1. Compruebe que el modelo clasifica esta imagen como **elefante**.

1. Vamos a probar con otra más. Ejecute este comando:

    ```PowerShell
    ./classify-image.ps1 3
    ```

    La imagen final tiene este aspecto:

    ![Fotografía de un león.](media/create-image-classification-system/animal-3.jpg)

1. Compruebe que el modelo clasifica esta imagen como **león**.

Esperamos que el modelo de clasificación de imágenes clasifique correctamente las tres imágenes.

## Más información

En este ejercicio se muestran solo algunas de las funcionalidades del servicio Custom Vision. Para más información sobre lo que puede hacer con este servicio, consulte la [página de Custom Vision](https://azure.microsoft.com/services/cognitive-services/custom-vision-service/).
