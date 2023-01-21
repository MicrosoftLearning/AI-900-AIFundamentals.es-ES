---
lab:
  title: Exploración de la clasificación de imágenes
---

# <a name="explore-image-classification"></a>Exploración de la clasificación de imágenes

> **Nota** Para completar este laboratorio, necesitará una [suscripción de Azure](https://azure.microsoft.com/free?azure-portal=true) en la que tenga acceso de administrador.

El servicio cognitivo *Computer Vision* proporciona modelos útiles creados previamente para trabajar con imágenes, pero a menudo necesitará entrenar su propio modelo para visión informática. Por ejemplo, supongamos que la empresa minorista Northwind Traders quiere crear un sistema de finalización de la compra automatizado que identifique los artículos que los clientes quieren comprar en función de una imagen tomada por una cámara en el momento de la finalización de la compra. Para ello, deberá entrenar un modelo de clasificación que pueda clasificar las imágenes a fin de identificar el artículo que se va a comprar.

En Azure, puede usar el servicio cognitivo ***Custom Vision*** para entrenar un modelo de clasificación de imágenes basado en imágenes existentes. Hay dos elementos para crear una solución de clasificación de imágenes. En primer lugar, debe entrenar un modelo para reconocer clases diferentes mediante imágenes existentes. Después, una vez entrenado el modelo, debe publicarlo como un servicio que pueden consumir las aplicaciones.

Para probar las capacidades del servicio Custom Vision, usaremos una aplicación de línea de comandos sencilla que se ejecuta en Cloud Shell. Los mismos principios y funcionalidad se aplican en soluciones reales, como sitios web o aplicaciones de teléfono.

## <a name="create-a-cognitive-services-resource"></a>Creación de un recurso de *Cognitive Services*

Para usar el servicio Computer Vision, puede crear un recurso de **Computer Vision** o un recurso de **Cognitive Services**.

>**Nota** No todos los recursos están disponibles en todas las regiones. Tanto si crea un recurso de Custom Vision como de Cognitive Services, para acceder a los servicios de Custom Vision, solo se pueden usar los recursos creados en [determinadas regiones](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services). Para simplificar el proceso, se selecciona previamente una región en las instrucciones de configuración siguientes.

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

1. Descargue y extraiga las imágenes de entrenamiento de https://aka.ms/fruit-images. Estas imágenes se proporcionan en una carpeta comprimida, que cuando se extrae contiene subcarpetas denominadas **apple**, **banana** y **orange**.

1. En otra pestaña del explorador, abra el portal de Custom Vision en [https://customvision.ai](https://customvision.ai?azure-portal=true). Si se le solicita, inicie sesión con la cuenta de Microsoft asociada a su suscripción de Azure y acepte las Condiciones del servicio

1. En el portal de Custom Vision, cree un nuevo proyecto con la siguiente configuración:

    - **Nombre**: finalización de la compra de alimentos
    - **Descripción** clasificación de imágenes de alimentos
    - **Recurso**: *el recurso de Custom Vision que ha creado anteriormente*
    - **Tipos de proyecto**: Clasificación
    - **Tipos de clasificación**: multiclase (etiqueta única por imagen)
    - **Dominios**: comida

1. Haga clic en **Añadir imágenes** y seleccione todos los archivos de la carpeta **apple** extraídos anteriormente. Después, cargue los archivos de imágenes e incluya la etiqueta *apple*, como en este ejemplo:

    ![Cargar la manzana con la etiqueta apple](media/create-image-classification-system/upload-apples.jpg)

1. Repita el paso anterior para cargar las imágenes de la carpeta **banana** con la etiqueta *banana* y las imágenes de la carpeta **orange** con la etiqueta *orange*.

1. Explore las imágenes cargadas en el proyecto de Custom Vision, debería tener 15 imágenes de cada clase, como en este ejemplo:

    ![Imágenes de frutas etiquetadas: 15 manzanas, 15 bananas y 15 naranjas](media/create-image-classification-system/fruit.jpg)

1. En el proyecto de Custom Vision, sobre las imágenes, haga clic en **Train** para entrenar un modelo de clasificación con las imágenes etiquetadas. Seleccione la opción **Quick Training** y, después, espere a que se complete el entrenamiento (puede tardar alrededor de un minuto).

1. Una vez entrenado el modelo, compruebe las métricas de rendimiento *Precision*, *Recall* y *AP* (miden la precisión de la predicción del modelo de clasificación y sus valores deberían ser altos).

## <a name="test-the-model"></a>Prueba del modelo

Antes de publicar esta iteración del modelo para que las aplicaciones lo usen, debe probarlo.

1. Sobre las métricas de rendimiento, haga clic en **Quick Test**.

1. En el cuadro **URL de la imagen**, escriba `https://aka.ms/apple-image` y haga clic en ➔

1. Consulte las predicciones obtenidas del modelo, la puntuación de probabilidad de *apple* debería ser la más alta, como en este ejemplo:

    ![Una imagen con una predicción de clase de una manzana](media/create-image-classification-system/test-apple.jpg)

1. Cierre la ventana **Quick Test**.

## <a name="publish-the-image-classification-model"></a>Publicación del modelo de clasificación de imágenes

Ahora está listo para publicar el modelo entrenado y usarlo desde una aplicación cliente.

1. Haga clic en **&#128504; Publicar** para publicar el modelo entrenado con la configuración siguiente:
    - **Nombre del modelo**: alimentos
    - **Recurso de predicción**: *el recurso de predicción creado anteriormente*.

1. Después de la publicación, haga clic en *Dirección URL de predicción* (&#127760;) para ver la información necesaria para usar el modelo publicado. Más adelante, necesitará la dirección URL y los valores Prediction-Key adecuados para obtener una predicción de una dirección URL de imagen, por lo que debe mantener este cuadro de diálogo abierto y continuar con la siguiente tarea. 

## <a name="run-cloud-shell"></a>Ejecución de Cloud Shell

Para probar las funcionalidades del servicio Custom Vision, usaremos una sencilla aplicación de línea de comandos que se ejecuta en Cloud Shell en Azure.

1. En Azure Portal, seleccione el botón **[>_]** (*Cloud Shell*) situado en la parte superior de la página, a la derecha del cuadro de búsqueda. Se abre un panel de Cloud Shell en la parte inferior del portal. 

    ![Inicio de Cloud Shell haciendo clic en el icono situado a la derecha del cuadro de búsqueda superior](media/create-image-classification-system/powershell-portal-guide-1.png)

1. La primera vez que abra Cloud Shell, es posible que se le pida que elija el tipo de shell que desea usar (*Bash* o *PowerShell*). Seleccione **PowerShell**. Si no ve esta opción, omita el paso.  

1. Si se le pide que cree almacenamiento para Cloud Shell, asegúrese de que se haya especificado su suscripción y seleccione **Crear almacenamiento**. A continuación, espere un minuto más o menos a que se cree el almacenamiento.

    [![Para crear el almacenamiento, haga clic en Confirmar.](media/create-image-classification-system/powershell-portal-guide-2.png)](media/create-image-classification-system/powershell-portal-guide-2.png#lightbox)

1. Asegúrese de que el tipo de shell indicado en la parte superior izquierda del panel de Cloud Shell se cambia a *PowerShell*. Si es *Bash*, cambie a *PowerShell* mediante el menú desplegable.

    ![Cómo encontrar el menú desplegable de la izquierda para cambiar a PowerShell](media/create-image-classification-system/powershell-portal-guide-3.png)

1. Espere a que se inicie PowerShell. Debería ver la siguiente pantalla en Azure Portal:  

    ![Espere a que se inicie PowerShell.](media/create-image-classification-system/powershell-prompt.png)

## <a name="configure-and-run-a-client-application"></a>Configuración y ejecución de una aplicación cliente

Ahora que tiene un entorno de Cloud Shell, puede ejecutar una aplicación sencilla que use el servicio Custom Vision para analizar una imagen.

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

    ![Editor de código.](media/create-image-classification-system/powershell-portal-guide-4.png)

1. En el panel **Archivos** de la izquierda, expanda **ai-900** y seleccione **classify-image.ps1**. Este archivo contiene código que usa el modelo de Custom Vision para analizar una imagen, tal como se muestra aquí:

     ![Editor que contiene código para clasificar una imagen](media/create-image-classification-system/classify-image-code.png)

1. No se preocupe demasiado por los detalles del código, lo importante es que necesita la dirección URL de predicción y la clave para el modelo de Custom Vision al usar una dirección URL de imagen. 

   Obtenga la *URL predictiva* en el cuadro de diálogo de su proyecto Custom Vision. 

   >**Nota** Recuerde que ha revisado la *URL predictiva* tras publicar el modelo de clasificación de imágenes. Para encontrar la *URL predictiva*, diríjase a la pestaña **Rendimiento** de su proyecto y haga clic en **URL predictiva** (si la pantalla está comprimida, puede que solo aparezca un icono de globo). Aparece un cuadro de diálogo. Copie la URL de **Si tiene una URL de imagen**. Péguela en el editor de código, reemplazando **YOUR_PREDICTION_URL**.

    Con el mismo cuadro de diálogo, obtenga la *clave de predicción*. Copie la clave de predicción que se muestra después de *establecer Prediction-Key encabezado en*. Péguela en el editor de código, reemplazando el valor de marcador de posición **YOUR_PREDICTION_URL**.

    ![Captura de pantalla de la URL predictiva.](media/create-image-classification-system/find-prediction-url.png)

    Después de pegar los valores de dirección URL de predicción y clave de predicción, las dos primeras líneas de código deben tener un aspecto similar al siguiente:

    ```PowerShell
    $predictionUrl="https..."
    $predictionKey ="1a2b3c4d5e6f7g8h9i0j...."
    ```

1. En la parte superior derecha del panel del editor, use el botón **...** para abrir el menú y seleccione **Guardar** para guardar los cambios. A continuación, vuelva a abrir el menú y seleccione **Cerrar editor**.

    Usará la aplicación cliente de ejemplo para clasificar varias imágenes en la categoría manzana, plátano o naranja.

1. Clasificaremos esta imagen:

    ![Imagen de una manzana](media/create-image-classification-system/fruit-1.jpg)

    En el panel de PowerShell, escriba los siguientes comandos para ejecutar el código:

    ```PowerShell
    cd ai-900
    ./classify-image.ps1 1
    ```

1. Revise la predicción, que debería ser **manzana**.

1. Ahora vamos a probar otra imagen:

    ![Imagen de un plátano](media/create-image-classification-system/fruit-2.jpg)

    Ejecute este comando:

    ```PowerShell
    ./classify-image.ps1 2
    ```

1. Compruebe que el modelo clasifica esta imagen como **plátano**.

1. Por último, probemos la tercera imagen de prueba:

    ![Imagen de una naranja](media/create-image-classification-system/fruit-3.jpg)

    Ejecute este comando:

    ```PowerShell
    ./classify-image.ps1 3
    ```

1. Compruebe que el modelo clasifica esta imagen como **naranja**.

## <a name="learn-more"></a>Más información

Esta sencilla aplicación muestra solo algunas de las funcionalidades del servicio Custom Vision. Para más información sobre lo que puede hacer con este servicio, consulte la [página de Custom Vision](https://azure.microsoft.com/services/cognitive-services/custom-vision-service/).
