## Desarrollo local 

Si trabaja desde su equipo local, puede seguir estos pasos para configurar su entorno para utilizar los laboratorios.  

### Paquete redistribuible de C++ 
1. Descargue e instale el [Paquete redistribuible de Visual C++ (x64)](https://aka.ms/vs/16/release/vc_redist.x64.exe) 

### Python y paquetes necesarios 
1. Instale [Python 3.6.1](https://www.python.org/downloads/release/python-361/)  
   - **Importante**: Seleccione las opciones para agregar Python a la variable PATH y para registrarla como el entorno de Python predeterminado 
2. Después de la instalación, abra el *Símbolo del sistema* y escriba el siguiente comando para instalar los paquetes necesarios: 

> pip install ipython jupyter matplotlib pillow requests azure-cognitiveservices-vision-computervision azure-cognitiveservices-vision-customvision azure-cognitiveservices-vision-face azure-cognitiveservices-language-textanalytics azure.cognitiveservices.speech azure_ai_formrecognizer 

### Visual Studio Code 
1. Si no tiene instalado Visual Studio Code, [descárguelo aquí](https://code.visualstudio.com/Download). Después de instalar Visual Studio Code, ábralo y, en la pestaña Extensions (Ctrl + Mayús + X), busque e instale la extensión **Python** de Microsoft.

2. En Visual Studio Code, abra un nuevo terminal, escriba **git clone https://github.com/MicrosoftLearning/AI-900ES-Microsoft-Azure-AI-Fundamentals** y pulse *Entrar*. 

