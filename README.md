# Redes neuronales - autoencoders convolucionales y transfer learning en Fashion MNIST

Este repositorio contiene el Trabajo Práctico Final realizado para la asignatura Redes Neuronales de la Facultad de Matemática, Astronomía, Física y Computación, Universidad Nacional de Córdoba. El proyecto se centra en la implementación de autoencoders convolucionales para la reconstrucción de imágenes del conjunto de datos Fashion MNIST y su posterior aplicación en tareas de clasificación mediante transfer learning.

El objetivo principal fue explorar diferentes arquitecturas y configuraciones de hiperparámetros y observar como afectan a la capacidad de una red para comprimir y reconstruir datos complejos como el dataset Fashion-MNIST, que contiene 70,000 imágenes de prendas de vestir de 28x28 píxeles.
Posteriormente, se reutilizó el encoder preentrenado para construir un clasificador, evaluando si el conocimiento adquirido en la tarea de reconstrucción mejora la precisión al etiquetar las imágenes.

Para este proyecto se utilizaron:
* Capas Convolucionales 2D: aplican filtros para capturar características relevantes. 
* Función de activación ReLU: introduce no linealidad en la red. 
* Max pooling: reduce la dimensión espacial manteniendo las características esenciales. 
* Dropout: capa para evitar el sobreajuste (overfitting). 
* Clasificador Convolucional: basado en la reutilización del encoder del autoencoder con una capa lineal adicional para la asignación de etiquetas.

### AUTOENCODER
Se utilizó un caso base ($n=64$, $p=0.2$, ADAM, $lr=10^{-3}$, batch size 100) y se realizaron variaciones de un único hiperparámetro a la vez: 
* Tamaño de capa lineal ($n$): cuando $n$ aumenta, la pérdida tiende a ser menor.
* Dropout ($p$): con valores más altos de $p$, la función de pérdida tiende a ser mayor, sin presencia de over shooting. 
* Optimizador: ADAM presentó una convergencia más rápida frente a SGD, que requirió más iteraciones. 
* Learning Rate ($lr$): con $lr=10^{-2}$ la pérdida osciló considerablemente sin disminuir de forma clara. 
* Batch Size: Con un tamaño de 50 se sugirió un posible sobreajuste, mientras que 100 y 150 mostraron una reducción más estable. 

Selección Final: Encoder con $n=64$, dropout $p=0.1$, batch size 100, $lr=10^{-3}$ y optimizador ADAM. 

### CLASIFICADOR
Se evaluó el desempeño del clasificador utilizando Cross Entropy Loss y Precisión. Para analizar el impacto del conocimiento previo, se compararon cuatro casos:
* Caso 1 (Preentrenado + Entrenamiento conjunto): Al utilizar el encoder preentrenado y entrenar ambos componentes, se alcanzó la mayor precisión.
* Caso 2 (Preentrenado + Solo clasificador): entrenando únicamente la capa de clasificación sobre el encoder preentrenado, se mantuvieron valores estables de precisión alrededor del 80%.
* Caso 3 (Sin preentrenar + Entrenamiento conjunto): buen desempeño con una precisión cercana al 88%, evidenciando que el entrenamiento conjunto es efectivo incluso desde cero.
* Caso 4 (Sin preentrenar + Solo clasificador): Fue el escenario con menor rendimiento, con una pérdida inicial muy alta y una precisión final inferior al 75%.

Las matrices de confusión confirmaron que el Caso 1 es el que presenta mayor coincidencia entre etiquetas predichas y verdaderas, mientras que el Caso 4 muestra la mayor confusión, aunque logra captar ciertas estructuras básicas de los datos.

