![Pandas](https://img.shields.io/badge/-Pandas-333333?style=flat&logo=pandas)
![Numpy](https://img.shields.io/badge/-Numpy-333333?style=flat&logo=numpy)
![Matplotlib](https://img.shields.io/badge/-Matplotlib-333333?style=flat&logo=matplotlib)
![Seaborn](https://img.shields.io/badge/-Seaborn-333333?style=flat&logo=seaborn)
![Scikitlearn](https://img.shields.io/badge/-Scikitlearn-333333?style=flat&logo=scikitlearn)
![FastAPI](https://img.shields.io/badge/-FastAPI-333333?style=flat&logo=fastapi)
![Render](https://img.shields.io/badge/-Render-333333?style=flat&logo=render)

# <h1 align=center>**`PI01_MLOps_SteamGames`**</h1>
# <h1 align=center>**`Machine Learning Operations (MLOps)`**</h1>

<p align=center><img src=https://user-images.githubusercontent.com/67664604/217914153-1eb00e25-ac08-4dfa-aaf8-53c09038f082.png><p>

## **Introducción**
Este proyecto simula el rol de un MLOps Engineer, se entregan unos datos y tiene como objetivo crear una API desplegada de manera pública donde se usa un modelo de recomendacion basado en Machine Learning para la plataforma multinacional de videojuegos Steam. La API permite ofrecer una interfaz intituiva para los usuarios y así consumir la información que se ofrece, como el sistema de recomendación, un análisis de sentimientos sobre los comentarios de los usuarios sobre los juegos y datos sobre géneros de videojuegos y fechas.

## Datos proporcionados

Se proporcionaron tres archivos .JSON:

* **australian_users_items.json** dataset que contiene información sobre los juegos que juegan todos los usuarios, así como el tiempo acumulado que cada usuario jugó en un juego específico.

* **australian_user_reviews.json** dataset que contiene las reseñas que los usuarios realizaron sobre los juegos que consumen, además de datos adicionales como si recomiendan o no ese juego. También presenta el id del usuario que comenta con su url del perfil y el id del juego que comenta.

* **output_steam_games.json** dataset que contiene datos relacionados con los juegos en sí, como los título, el desarrollador, entre otros datos.

En el documento [Diccionario](https://github.com/santiagrueda/PI01_MLOps_SteamGames/blob/main/Dic.md) encuentran cada una de las variables al detalle de los datasets trabajados.

## **Librerías requeridas**
+ Pandas
+ Numpy
+ Seaborn
+ Matplotlib
+ NLTK
+ WordCloud
+ Scikit-Learn
+ FastAPI
+ Uvicorn
+ Render

## Tareas solicitadas

### Transformaciones

Se realizó un proceso de extracción, transformación y carga (ETL) de los tres conjuntos de datos proporcionados. De los cuales dos de los archivos se encontrabas anidados, por lo que se encontró columnas con listas de diccionarios, para ello se aplicó distintos métodos para desanidar estos diccionarios y poder trabajar con esta información de las columnas. Finalmente se borran filas con valores nulos y columnas las cuales no aportaban un valor para el objetivo del proyecto y de esta manera optimizar el rendimiento de la API y las limitaciones de procesamiento y almacenamiemto del despliegue. Finalmente teniendo los conjuntos de los datos desanidados y limpios, se procedió a unificar los dataset en uno solo, esto proporciona agilidad y rapidéz para trabajar con un sólo archivo y no con tres.

## Feature Engineering

Esta etapa del proyecto consistía en aplicar un análisis de sentimiento a las reseñas de los usuarios donde realiza una clasificación en la siguiente escala:

* 0 si es malo.
* 1 si es neutral o no hay review.
* 2 si es positivo.

Este análisis de sentimiento toma texto como entrada, luego clasifica la revisión como negativa, neutral o positiva en función de la polaridad calculada por el modelo. En este caso, se consideraron las polaridades por defecto del modelo y de acuerdo a condicionales que se incorporan en el código, se obtiene una nueva columna con valores de 0,1 y 2 dependiendo del análisis de cada reseña.

## Análisis Exploratorio de Datos (EDA)

Se realizó un EDA con el objetivo de identificar directamente tendencias, valores atípicos u outliers. También correlaciones que se pueden encontrar en los datos, ya que con los archivos originales no es posible tener una exploración profunda y ágil de los datos. Inicialmente se observó los datasets por separado para explorar los tipos de datos que estos contenían, cantidad de columnas y posterior a este primer vistazo, seleccionamos todas las columnas con valores numéricos para poder realizar un análisis descriptivo. Se realizó unadicionalmente varios gráficos para observar si existían correlaciones entre las variables, distribuciones de los datos para identificar algún tipo de tendencia, se realizo un gráfico de un Top 10 de los juegos más frecuentes entre los usuarios como tambien un grafo llamado Nube de palabras, el cual me permite conocer aquellas palabras con más frecuencia entre las reseñas y así tener una idea inicial de las opiniones del juego por parte de los usuarios.

## Modelo de Machine Learning

Se crearon dos modelos de recomendación que tambien son basados en Machine Learning al igual que el análisis de sentimientos, cada modelo devuelve una lista de 5 juegos recomendados ya sea ingresando el id de un juego o el id de usuario. se utilizó la **similitud del coseno** que es una medida comúnmente utilizada para evaluar la similitud entre dos vectores en un espacio multidimensional. De esta manera encontrar usuarios o juegos similares al usuario/juego activo (es decir, los usuarios/juegos para los que se les quiere recomendar) y utilizando sus preferencias para predecir las valoraciones del usuario/juego activo cual sea el caso de la entrada del modelo.

## Desarrollo de la API

En el desarrollo de la API se utilizó el framework FastAPI, creando los siguientes endpoints:

* **def PlayTimeGenre( genero : str ):** Debe devolver año con mas horas jugadas para dicho género.
**Ejemplo de retorno:** {"Año de lanzamiento con más horas jugadas para Género X" : 2013}

* **def UserForGenre( genero : str ):** Debe devolver el usuario que acumula más horas jugadas para el género dado y una lista de la acumulación de horas jugadas por año.
**Ejemplo de retorno:** {"Usuario con más horas jugadas para Género X" : us213ndjss09sdf, "Horas jugadas":[{Año: 2013, Horas: 203}, {Año: 2012, Horas: 100}, {Año: 2011, Horas: 23}]}

* **def UsersRecommend( año : int ):** Devuelve el top 3 de juegos MÁS recomendados por usuarios para el año dado. (reviews.recommend = True y comentarios positivos/neutrales)
**Ejemplo de retorno:** [{"Puesto 1" : X}, {"Puesto 2" : Y},{"Puesto 3" : Z}]

* **def UsersWorstDeveloper( año : int ):** Devuelve el top 3 de desarrolladoras con juegos MENOS recomendados por usuarios para el año dado. (reviews.recommend = False y comentarios negativos)
**Ejemplo de retorno:** [{"Puesto 1" : X}, {"Puesto 2" : Y},{"Puesto 3" : Z}]

* **def sentiment_analysis( empresa desarrolladora : str ):** Según la empresa desarrolladora, se devuelve un diccionario con el nombre de la desarrolladora como llave y una lista con la cantidad total de registros de reseñas de usuarios que se encuentren categorizados con un análisis de sentimiento como valor.
**Ejemplo de retorno:** {'Valve' : [Negative = 182, Neutral = 120, Positive = 278]}

* **def recomendacion_juego( id de producto ):** Ingresando el id de producto, deberíamos recibir una lista con 5 juegos recomendados similares al ingresado.

Todas estas funciones desarrolladas para la API se encuentran en el archivo [main.py](https://github.com/santiagrueda/PI01_MLOps_SteamGames/blob/main/main.py)

> *Importante: Los dos modelos de recomendación, recomendacion_juego y recomendacion_usuario se agregaron a la API, pero sólo recomendacion_juego se pudo deployar en Render debido que  la predicción excedía la capacidad de almacenamiento disponible. Por lo tanto, para utilizar la función recomendacion_usuario se puede ejecutar la API de manera local.*

## Video

En este [video](https://www.youtube.com/watch?v=heSxrgRBKwQ) encuentran una breve explicación de este proyecto, haciendo énfasis en el funcionamiento de la API con sus endpoints.

## **Link**

* [Deploy de la API](https://pi01-prueba.onrender.com/)
* [GitHub](https://github.com/santiagrueda)

## **Contacto**
* Linkedin: [Santiago Rueda Mira](https://www.linkedin.com/in/santiago-rueda-mira-050b55113/)
* Correo: santiagomira72@gmail.com