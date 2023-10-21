---
lab:
  title: Exploración del reconocimiento de formularios
---

# Exploración del reconocimiento de formularios

> **Nota** Para completar este laboratorio, necesitará una [suscripción de Azure](https://azure.microsoft.com/free?azure-portal=true) en la que tenga acceso de administrador.

En el campo de inteligencia artificial (IA) de Computer Vision, el reconocimiento óptico de caracteres (OCR) se usa normalmente para leer documentos impresos o manuscritos. A menudo, el texto simplemente se extrae de los documentos en un formato que se puede usar para su posterior procesamiento o análisis.

Un escenario de OCR más avanzado es la extracción de información de formularios, como pedidos de compra o facturas, con un conocimiento semántico de lo que representan los campos del formulario. El servicio **Form Recognizer** está diseñado específicamente para este tipo de problema de inteligencia artificial.

Form Recognizer modelos de Machine Learning entrenados para extraer texto de imágenes de facturas, recibos, etc. Aunque otros modelos de Computer Vision pueden capturar texto, Form Recognizer también captura la estructura del texto, como pares clave-valor e información de tablas. De este modo, en lugar de tener que escribir manualmente entradas de un formulario en una base de datos, puede capturar automáticamente las relaciones entre el texto del archivo original. 

Para probar las funcionalidades del servicio Form Recognizer, usaremos una sencilla aplicación de línea de comandos que se ejecuta en Cloud Shell. Los mismos principios y funcionalidad se aplican en soluciones reales, como sitios web o aplicaciones de teléfono.

## Creación de un grupo de recursos de *servicios de Azure AI*

Para aprovisionar el servicio Form Recognizer, puede crear un recurso de **Form Recognizer** o un recurso de **servicios de Azure AI**.

Si aún no lo ha hecho, cree un recurso de **servicios de Azure AI** en la suscripción de Azure.

1. En otra pestaña del explorador, abra Azure Portal en [https://portal.azure.com](https://portal.azure.com?azure-portal=true) e inicie sesión con su cuenta Microsoft.

1. Haga clic en el botón **&#65291;Crear un recurso** y busque *Servicios de Azure AI*. Seleccione **Crear** un plan de **servicios de Azure AI**. Se le dirigirá a una página para crear un recurso de servicios de Azure AI. Configúrelo con los valores siguientes:
    - **Suscripción**: *su suscripción a Azure*.
    - **Grupo de recursos**: *cree o seleccione un grupo de recursos con un nombre único*.
    - **Región**: *elija cualquier región disponible*.
    - **Nombre**: *escriba un nombre único*.
    - **Plan de tarifa**: estándar S0
    - **Al marcar esta casilla, confirmo que he leído y comprendido todos los términos que aparecen a continuación**: Seleccionado.

1. Revise y cree el recurso y espere a que finalice la implementación. A continuación, vaya al recurso implementado.

1. Vea la página **Claves y punto de conexión** del recurso de servicios de Azure AI. Necesitará el punto de conexión y las claves para conectarse desde las aplicaciones cliente.

## Ejecución de Cloud Shell

Para probar las funcionalidades del servicio Form Recognizer, usaremos una sencilla aplicación de línea de comandos que se ejecuta en Cloud Shell en Azure. 

1. En Azure Portal, seleccione el botón **[>_]** (*Cloud Shell*) situado en la parte superior de la página, a la derecha del cuadro de búsqueda. Se abre un panel de Cloud Shell en la parte inferior del portal. 

    ![Inicio de Cloud Shell haciendo clic en el icono situado a la derecha del cuadro de búsqueda superior](media/analyze-receipts/powershell-portal-guide-1.png)

1. La primera vez que abra Cloud Shell, es posible que se le pida que elija el tipo de shell que desea usar (*Bash* o *PowerShell*). Seleccione **PowerShell**. Si no ve esta opción, omita el paso.  

1. Si se le pide que cree almacenamiento para Cloud Shell, asegúrese de que se haya especificado su suscripción y seleccione **Crear almacenamiento**. A continuación, espere un minuto más o menos a que se cree el almacenamiento.

    ![Para crear el almacenamiento, haga clic en Confirmar.](media/analyze-receipts/powershell-portal-guide-2.png)

1. Asegúrese de que el tipo de shell indicado en la parte superior izquierda del panel de Cloud Shell se cambia a *PowerShell*. Si es *Bash*, cambie a *PowerShell* mediante el menú desplegable.

    ![Cómo encontrar el menú desplegable de la izquierda para cambiar a PowerShell](media/analyze-receipts/powershell-portal-guide-3.png) 

1. Espere a que se inicie PowerShell. Debería ver la siguiente pantalla en Azure Portal:  

    ![Espere a que se inicie PowerShell.](media/analyze-receipts/powershell-prompt.png) 

## Configuración y ejecución de una aplicación cliente

Ahora que tiene un modelo personalizado, puede ejecutar una sencilla aplicación cliente que use el servicio Form Recognizer.

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

    ![Editor de código.](media/analyze-receipts/powershell-portal-guide-4.png)

1. En el panel **Archivos** de la izquierda, expanda **ai-900** y seleccione **form-recognizer.ps1**. Este archivo contiene código que usa el servicio Form Recognizer para analizar los campos de un recibo, como se muestra aquí:

    ![Editor que contiene código para analizar los campos de un recibo.](media/analyze-receipts/recognize-receipt-code.png)

1. No se preocupe demasiado por los detalles del código, lo importante es que necesita la dirección URL del punto de conexión y cualquiera de las claves para el recurso de servicios de Azure AI. Cópielos desde la página **Claves y puntos de conexión** del recurso en Azure Portal y péguelos en el editor de código, pero reemplace los valores de marcador de posición **YOUR_KEY** y **YOUR_ENDPOINT**, respectivamente.

    > **Consejo** Es posible que tenga que usar la barra de separación para ajustar el área de pantalla mientras trabaja con los paneles **Claves y punto de conexión** y **Editor**.

    Después de pegar los valores de clave y punto de conexión, las dos primeras líneas de código deben tener un aspecto similar al siguiente:

    ```PowerShell
    $key="1a2b3c4d5e6f7g8h9i0j...."    
    $endpoint="https..."
    ```

1. En la parte superior derecha del panel del editor, use el botón **...** para abrir el menú y seleccione **Guardar** para guardar los cambios. A continuación, vuelva a abrir el menú y seleccione **Cerrar editor**. Ahora que ha configurado la clave y el punto de conexión, puede usar el recurso para analizar los campos de un recibo. En este caso, usará el modelo integrado de Form Recognizer a fin de analizar un recibo para la empresa minorista Northwind Traders ficticia.

    La aplicación cliente de ejemplo analizará la siguiente imagen:

    ![Se trata de una imagen de un recibo.](media/analyze-receipts/receipt.jpg)

1. En el panel de PowerShell, escriba los siguientes comandos a fin de ejecutar el código para leer el texto:

    ```PowerShell
    cd ai-900
    ./form-recognizer.ps1
    ```

1. Revise los resultados devueltos. Vea que Form Recognizer puede interpretar los datos en el formulario, identificando correctamente la dirección y el número de teléfono del comerciante, así como la fecha y hora de la transacción, y los elementos de línea, el subtotal, los impuestos y los importes totales.

## Más información

Esta sencilla aplicación muestra solo algunas de las funcionalidades de Form Recognizer del servicio Computer Vision. Para obtener más información sobre lo que puede hacer con este servicio, consulte la [página de Form Recognizer](https://docs.microsoft.com/azure/applied-ai-services/form-recognizer/overview).

