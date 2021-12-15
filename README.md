![head](https://github.com/manuelvasquezg/project_proveindustriales/blob/main/Imagenes/banner-intermedio-cientifico-datos.jpg "head")

# Algoritmos de machine learning para la predicción de órdenes rechazadas y de estados de cierre en transacciones de compra a la empresa Proveindustriales

## Identificación del problema

Tras realizar el levantamiento de información y requisitos a la empresa proveindustriales, y tras el análisis del contexto en el que se ubican los datos entregados por la misma, se identifica que la empresa no cuenta con un sistema de alertas tempranas que le permita preveer y tomar acción sobre la posibilidad de que una orden de compra sea rechazada. Como consecuencia, en caso de que la orden no sea rechazada, tampoco se cuenta con la capacidad de predecir la probabilidad de que una orden de compra llegue a ser recibida completa, recibida parcial, finalizada incompleto o cancelada. Por tanto, estas son las dos principales necesidades que se identifican y que pasan a conformar el objetivo de este proyecto.

## Propuesta de valor

Los resultados se dividen en dos modelos. Del primer modelo se espera que pueda clasificar y predecir las transacciones rechazadas, de tal manera que la empresa pueda tomar decisiones con base en esta información y así disminuir la cantidad de transacciones rechazadas, que redundaría en una disminución de costos de operación.

Del segundo modelo se espera que permita conocer la probabilidad de que una orden pueda sea recibida completa, recibida parcial o finalizada incompleto, permitiendo identificar la confiabilidad del proveedor y fortaleciendo las caracterisiticas que hacen que la etrega se lo mas optima posible.

## Linea de base

![lineadebase](https://github.com/manuelvasquezg/project_proveindustriales/blob/main/Imagenes/Screenshot%20from%202021-12-15%2014-29-19.png "lineadebase")

En la linea de base se enumeran las variables que fueron tomadas en cuenta para entrenar los modelos y al final de cada línea y con dichas variables, se enuncian los resultados esperados.

## Modelos

### 1. Modelo de clasificación de ordenes rechazadas

#### Detalles del modelo:

Tras el análisis exploratorio y extraer aquellas varibales que tenían una alta correlación entre sí, las variables independientes que quedaron fueron: cantidad pedida, precio unitario, total IVA, días transcurridos, categoría, proveedor y dirección de pedido.

Se seleccionaron ategorías con registros de mas del 15% del total de los datos, ya que las categorías de menos registros estaban generando overfitting. De esto, las categorías que se tomaron fueron:

![detallesmodelo](https://github.com/manuelvasquezg/project_proveindustriales/blob/main/Imagenes/detallesmodelo.png "detallesmodelo")

Tras esta depuración, la cantidad de datos que quedaron fue:
- Train set : 12499
- Test set : 3472
- Valid set: 1389

#### Selección del mejor modelo basado en F1-Score

Se probaron tres modelos que clasificación binaria que permitirían dentificar si na orden de compra es candidata a ser rechazada o no. Estos modelos fueron los de Random Forest, Decision Tree y LightGBM.

Basandose en el F1-Score como medida de evaluación, se decidió escoger el modelo de LightGBM, debido a que esta medida de evaluación pretende calificar la presición de la clasificación hecha por el modelo en cuestión. Y se le dió importancia principalmente por la característica de los datos, ya que se dentificó un desabalance que podría generar overfiting, por que era importante analizar, principalmente, la precisión con que el modelo hacía cada clasificación. En otras palabras, se le dió mas importancia al hecho de que las ordenes de compra que el modelo clasificara como recahazadas, efectivamente fueran rechazadas, minimizando la probabilidad de que el modelo incurriera en falsos positivos.

![seleccionmejormodelounotabla](https://github.com/manuelvasquezg/project_proveindustriales/blob/main/Imagenes/seleccionmejormodelounotabla.png "seleccionmejormodelounotabla") ![seleccionmejormodelounobarras](https://github.com/manuelvasquezg/project_proveindustriales/blob/main/Imagenes/seleccionmejormodelounobarras.png "seleccionmejormodelounobarras")

### 2. Modelos predictivo de los estados de cierre en las ordenes de compra aprobadas

#### Detalles del modelo

Tras los resultados del modelo anterior donde se buscaba clasificar las ordenes que serían rechazadas, se desarrolló el segundo modelo que pretende predecir el estado en el que finalizará una orden de compra que *no sea rechazada*. Dichos estados de cierre, potencialmente serían:

- Recibido
- Cancelado
- Finalizado incompleto
- Recibido parcial

Este segundo modelo se basó en los resultados del anterior, por lo que las medidas del dataset y el filtro de las categorías realizados al principio, aplican tambien para este modelo.

#### Selección del mejor modelo basado en F1-Score

Se probaron tres modelos de clasificación multinomial o multiclase que permitirían identificar a cual de los cuatro estados de cierre es candidata la orden de compra, es decir, si es mas probable que sea Recibida, Cancelada, Finalizado Incompleto o Recibido Parcial. Estos modelos fueron los de K Vecinos mas Cercanos (KNN), Decision Tree y LightGBM.

Nuevamete, basandose en el F1-Score como medida de evaluación, se decidió escoger el modelo de LightGBM, debido a que esta medida de evaluación pretende calificar la presición de la clasificación hecha por el modelo en cuestión. Y se le dió importancia, principalmente, por la característica de los datos, ya que se dentificó un desabalance que podría generar overfiting. Por esto era importante analizar, principalmente, la precisión con que el modelo hacía cada clasificación. En otras palabras, se le dió mas importancia al hecho de que las ordenes de compra no rechazadas que el modelo clasificara como Finalizada, Cancelada, Finalizada Incompleta o Recibida Parcial, efectivamente fueran pertenecieran ese estado de cierre, minimizando la probabilidad de que el modelo incurriera clasificara ordenes de compra en estados de cierre incorrectos.

![seleccionmejormodelodostabla](https://github.com/manuelvasquezg/project_proveindustriales/blob/main/Imagenes/seleccionmejormodelodostabla.png "seleccionmejormodelodostabla") ![seleccionmejormodelodosbarras](https://github.com/manuelvasquezg/project_proveindustriales/blob/main/Imagenes/seleccionmejormodelodosbarras.png "seleccionmejormodelodosbarras")

#### Ejemplo de resultados

Tras ser entrenado y probado, el modelo es sometido a una última prueba con un mini set de validación con datos desconocidos. Esto permite explorar la confiabilidad de los resultados. Como se muestra en la primera linea del código, primero se ingresa el set de validación, del cual, para observar el resultado, se toma como muestra su primer registro. En la segunda linea del código, al extraer el primer registro, el modelo arroja una serie de valores porcentuales que corresponden a la probabilidad de que una orden de compra quede entre alguno de los estados de cierre, así que cada valor porcentual está representando un estado de cierre.

- 0 = Cancelado
- 1 = Finalizado Incompleto
- 2 = Recibido
- 3 = Recibido parcial

Entonces, el primer valor porcentual de la fila representa el estado de cierre "Cancelado", o sea 0; el segundo valor porcentual representa el estado de cierre "Finalizado Incompleto", o sea 1; y así sucesivamente. De estos valores porcentuales arrojados por el modelo se toma el mas alto, que para este ejemplo corresponde al primero de la fila, lo que significaría, según el modelo, que el registro seleccionado quedaría en estado de cierre "Cancelado".

En la tercera linea se verifica el estado de cierre real del registro usado para el ejemplo y se verifica que, efectivamente, su estado de cierre es 0, o sea, "Cancelado", con lo cual se puede comprobar que el porcentaje mas alto lo predijo bien el modelo.

![ejemploresuldatos](https://github.com/manuelvasquezg/project_proveindustriales/blob/main/Imagenes/ejemploresuldatos.png "ejemploresuldatos")

## Créditos

- Daniela Sofía Medina Diaz
- David Alberto Lopez Torres
- Edison Javier Yepes Sanchez
- Manuel Vásquez Garcia
- Rubiel Antonio Muñoz Jurado
