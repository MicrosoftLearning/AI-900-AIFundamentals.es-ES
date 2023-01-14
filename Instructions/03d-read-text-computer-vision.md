---
lab:
  title: Exploración del reconocimiento óptico de caracteres
---

# <a name="explore-optical-character-recognition"></a>Exploración del reconocimiento óptico de caracteres

> **Nota** Para completar este laboratorio, necesitará una [suscripción de Azure](https://azure.microsoft.com/free?azure-portal=true) en la que tenga acceso de administrador.

Uno de los retos comunes de Computer Vision es detectar e interpretar texto de una imagen. Este tipo de procesamiento se conoce a menudo como *reconocimiento óptico de caracteres* (OCR). Read API de Microsoft proporciona acceso a las funcionalidades de OCR. 

Para probar las funcionalidades de Read API, usaremos una sencilla aplicación de línea de comandos que se ejecuta en Cloud Shell. Los mismos principios y funcionalidad se aplican en soluciones del mundo real, como sitios web o aplicaciones de teléfono.

## <a name="use-the-computer-vision-service-to-read-text-in-an-image"></a>Uso del servicio Computer Vision para leer texto de una imagen

El servicio cognitivo **Computer Vision** proporciona compatibilidad con tareas de OCR, como por ejemplo:

- Una API **Read** optimizada para documentos más grandes. Esta API se usa de forma asincrónica y se puede emplear con texto impreso y manuscrito.

## <a name="create-a-cognitive-services-resource"></a>Creación de un recurso *Cognitive Services*

Para aprovisionar el servicio Computer Vision, puede crear un recurso de **Computer Vision** o un recurso de **Cognitive Services**.

Si aún no lo ha hecho, cree un recurso de **Cognitive Services** en la suscripción de Azure.

1. En otra pestaña del explorador, abra Azure Portal en [https://portal.azure.com](https://portal.azure.com?azure-portal=true) e inicie sesión con su cuenta Microsoft.

1. Haga clic en el botón **&amp;#65291;Crear un recurso**, busque *Cognitive Services* y cree un recurso de **Cognitive Services** con la siguiente configuración:
    - **Suscripción**: *su suscripción a Azure*.
    - **Grupo de recursos**: *cree o seleccione un grupo de recursos con un nombre único*.
    - **Región**: *elija cualquier región disponible*.
    - **Nombre**: *escriba un nombre único*.
    - **Plan de tarifa**: estándar S0
    - **Al marcar esta casilla, confirmo que he leído y comprendido todos los términos que aparecen a continuación**: Seleccionado.

1. Revise y cree el recurso y espere a que finalice la implementación. A continuación, vaya al recurso implementado.

1. Vea la página **Claves y punto de conexión** del recurso de Cognitive Services. Necesitará el punto de conexión y las claves para conectarse desde las aplicaciones cliente.

## <a name="run-cloud-shell"></a>Ejecución de Cloud Shell

Para probar las funcionalidades del servicio Custom Vision, usaremos una sencilla aplicación de línea de comandos que se ejecuta en Cloud Shell en Azure.

1. En Azure Portal, seleccione el botón **[>_]** (*Cloud Shell*) situado en la parte superior de la página, a la derecha del cuadro de búsqueda. Se abre un panel de Cloud Shell en la parte inferior del portal. 

    ![Inicio de Cloud Shell haciendo clic en el icono situado a la derecha del cuadro de búsqueda superior](media/read-text-computer-vision/powershell-portal-guide-1.png)

1. La primera vez que abra Cloud Shell, es posible que se le pida que elija el tipo de shell que desea usar (*Bash* o *PowerShell*). Seleccione **PowerShell**. Si no ve esta opción, omita el paso.  

1. Si se le pide que cree almacenamiento para Cloud Shell, asegúrese de que se haya especificado su suscripción y seleccione **Crear almacenamiento**. A continuación, espere un minuto más o menos a que se cree el almacenamiento.

    ![Para crear el almacenamiento, haga clic en Confirmar.](media/read-text-computer-vision/powershell-portal-guide-2.png)

1. Asegúrese de que el tipo de shell indicado en la parte superior izquierda del panel de Cloud Shell se cambia a *PowerShell*. Si es *Bash*, cambie a *PowerShell* mediante el menú desplegable.

    ![Cómo encontrar el menú desplegable de la izquierda para cambiar a PowerShell](media/read-text-computer-vision/powershell-portal-guide-3.png) 

1. Espere a que se inicie PowerShell. Debería ver la siguiente pantalla en Azure Portal:  

    ![Espere a que se inicie PowerShell.](media/read-text-computer-vision/powershell-prompt.png) 

## <a name="configure-and-run-a-client-application"></a>Configuración y ejecución de una aplicación cliente

Ahora que tiene un modelo personalizado, puede ejecutar una sencilla aplicación cliente que use el servicio OCR.

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

    ![Editor de código.](media/read-text-computer-vision/powershell-portal-guide-4.png)

1. En el panel **Archivos** de la izquierda, expanda **ai-900** y seleccione **ocr.ps1**. Este archivo contiene código que usa el servicio Computer Vision para detectar y analizar el texto de una imagen, como se muestra aquí:

    ![Editor que contiene código para analizar el texto de las imágenes.](media/read-text-computer-vision/ocr-code.png)

1. No se preocupe demasiado por los detalles del código, lo importante es que necesita la dirección URL del punto de conexión y cualquiera de las claves para el recurso Cognitive Services. Cópielos desde la página **Claves y puntos de conexión** del recurso en Azure Portal y péguelos en el editor de código, pero reemplace los valores de marcador de posición **YOUR_KEY** y **YOUR_ENDPOINT**, respectivamente.

    > **Consejo** Es posible que tenga que usar la barra de separación para ajustar el área de pantalla mientras trabaja con los paneles **Claves y punto de conexión** y **Editor**.

    Después de pegar los valores de clave y punto de conexión, las dos primeras líneas de código deben tener un aspecto similar al siguiente:

    ```PowerShell
    $key="1a2b3c4d5e6f7g8h9i0j...."    
    $endpoint="https..."
    ```

1. En la parte superior derecha del panel del editor, use el botón **...** para abrir el menú y seleccione **Guardar** para guardar los cambios. A continuación, vuelva a abrir el menú y seleccione **Cerrar editor**. Ahora que ha configurado la clave y el punto de conexión, puede usar el recurso Cognitive Services para extraer texto de una imagen.

    Uso de **Read** API. En este caso, tiene una imagen de publicidad para la empresa minorista ficticia Northwind Traders que incluye texto.

    La aplicación cliente de ejemplo analizará la siguiente imagen:

    ![Imagen de un anuncio de la tienda de comestibles Northwind Traders.](media/read-text-computer-vision/advert.jpg)

1. En el panel de PowerShell, escriba los siguientes comandos para ejecutar el código para leer el texto:

    ```PowerShell
    cd ai-900
    ./ocr.ps1 advert.jpg
    ```

1. Revise los detalles que se encuentran en la imagen. El texto que se encuentra en la imagen está organizado en una estructura jerárquica de regiones, líneas y palabras, y el código las lee para recuperar los resultados.

    Observe que la ubicación del texto está indicada por las coordenadas de la parte superior izquierda, y el ancho y el alto de un *cuadro de límite*, como se muestra aquí:

    ![Imagen del texto de la imagen descrita](media/read-text-computer-vision/lab-05-bounding-boxes.png)

1. Ahora vamos a probar otra imagen:

    ![Imagen de una carta mecanografiada.](media/read-text-computer-vision/letter.jpg)

    Para analizar la segunda imagen, escriba el siguiente comando:

    ```PowerShell
    ./ocr.ps1 letter.jpg
    ```

1. Revise los resultados del análisis de la segunda imagen. También debe devolver el texto y los cuadros de límite del texto.

## <a name="learn-more"></a>Más información

Esta sencilla aplicación muestra solo algunas de las funcionalidades OCR del servicio Computer Vision. Para más información sobre lo que puede hacer con este servicio, consulte la [página de OCR](https://docs.microsoft.com/azure/cognitive-services/computer-vision/overview-ocr).
