---
title: Instrucciones hospedadas en línea
permalink: index.html
layout: home
---

# Ejercicios de Fundamentos de Azure AI

Este repositorio contiene los ejercicios de laboratorios para el curso [*Fundamentos de Microsoft Azure Artificial Intelligence* (AI-900)](https://docs.microsoft.com/es-es/learn/certifications/courses/ai-900t00) y los módulos de aprendizaje autodirigido equivalentes de Microsoft Learn [Introducción a la inteligencia artificial en Azure](https://docs.microsoft.com/learn/paths/get-started-with-artificial-intelligence-on-azure/), [Crear modelos predictivos sin código con Azure Machine Learning](https://docs.microsoft.com/es-es/learn/paths/create-no-code-predictive-models-azure-machine-learning/),  [Explorar Computer Vision en Microsoft Azure](https://docs.microsoft.com/learn/paths/explore-computer-vision-microsoft-azure/), [Explorar el procesamiento del lenguaje natural](https://docs.microsoft.com/learn/paths/explore-natural-language-processing/) y [Explorar la IA de conversación](https://docs.microsoft.com/learn/paths/explore-conversational-ai/). Los ejercicios están pensados para complementar los materiales de aprendizaje y permiten practicar el uso de las tecnologías descritas en estos materiales. 

Para completar estos ejercicios, necesitará una suscripción a Microsoft Azure. Si su instructor no le ha proporcionado una suscripción, puede registrarse y recibir una versión de prueba gratuita en [https://azure.microsoft.com](https://azure.microsoft.com).

{% assign labs = site.pages | where_exp:"page", "page.url contains '/instructions'" %}
| Ejercicios |
| ------- | 
{% for activity in labs  %}| [{{ activity.lab.title }}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
