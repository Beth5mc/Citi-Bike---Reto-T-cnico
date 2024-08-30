# Citi-Bike---Reto-T-cnico
Ficha técnica del reto técnico de Job Search Prep Laboratoria - Citi Bike
Laboratoria Bootcamp  
Reto Técnico
Betsabé Morales Castro
Agosto 26, 2024


Ficha Técnica

Título del Proyecto:	Citi Bike Trips

Objetivo:

Realizar una exploración y análisis de los datos proporcionados con el fin de presentar a la nueva CEO de Citi Bike nuestra información mas relevante en cuanto a usuarios.


Listado de herramientas y tecnologías utilizadas en el proyecto:

1) Workspace Google (Hojas de cálculo, Presentaciones, Gemini y Documentos).
2) Chat GPT
3) Big Query (SQL)
4) Looker Studio


Para el proyecto de Citi Bike Trips contamos con la hoja de datos que nos proporcionó la empresa.



Procesamiento y análisis.

Para el proceso y preparación de la base de datos realizamos los siguientes pasos:
Se revisó la tabla de datos proporcionada para conocer las variables y comenzar a ver que tratamiento se le iba a dar.
Conectar/importar datos a herramienta Big Query.   
Identificar y manejar valores nulos, se encontraron 4,639 nulos en la columna birth_year.
La query utilizada es la siguiente:
SELECT
  COUNTIF(tripduration IS NULL) AS tripduration_null_count,
  COUNTIF(stoptime IS NULL) AS stoptime_null_count,
  COUNTIF(start_station_id IS NULL) AS start_station_id_null_count,
  COUNTIF(start_station_name IS NULL) AS start_station_name_null_count,
  COUNTIF(start_station_latitude IS NULL) AS start_station_latitude_null_count,
  COUNTIF(start_station_longitude IS NULL) AS start_station_longitude_null_count,
  COUNTIF(end_station_id IS NULL) AS end_station_id_null_count,
  COUNTIF(end_station_name IS NULL) AS end_station_name_null_count,
  COUNTIF(end_station_latitude IS NULL) AS end_station_latitude_null_count,
  COUNTIF(end_station_longitude IS NULL) AS end_station_longitude_null_count,
  COUNTIF(bikeid IS NULL) AS bikeid_null_count,
  COUNTIF(usertype IS NULL) AS usertype_null_count,
  COUNTIF(birth_year IS NULL) AS birth_year_null_count,
  COUNTIF(gender IS NULL) AS gender_null_count,
  COUNTIF(customer_plan IS NULL) AS customer_plan_null_count
FROM
  `citi_bike_trips.citi_bike_trips`;


Identificar y manejar valores duplicados, no se identificaron duplicados en la  tablas ya que no existe un id de usuario.

Identificar y manejar datos fuera del alcance del análisis, no eliminé ningún dato, lo que realicé fué su acotación por medio de winsorización para que no altere los cálculos de las estadísticas. No se tomará en cuenta la variable género para el análisis por considerarlo discriminatorio.
Se crearon nuevas variables como age y se obtuvo la edad promedio de los usuarios sin contar a los nulos, para posteriormente utilizar esa edad promedio de 45.78 años en los valores nulos. Lo realicé con la siguiente query 
SELECT
    AVG(age) AS average_age
FROM
    `citi_bike_trips.view_birth_year_edad`
WHERE
    age IS NOT NULL;

Se hizo una nueva view en donde también se sustituyeron los nulos de la edad por la edad promedio, se acotó el máximo de edad a 80 años para manejar los valores atípicos,  la variable tripduration estaba en segundos y se convirtió a minutos. De igual forma se dividió la columna stoptime utilizando el comando date en stoptime_date y en stoptime_time y se creó la variable start_time con la siguiente query:
SELECT
    -- Especificar las columnas que deseas incluir
    birth_year,
    tripduration,
    stoptime,
    start_station_id,
    start_station_name, 
    start_station_latitude,
    start_station_longitude,
    end_station_id,
    end_station_name,
    end_station_latitude,
    end_station_longitude,
    bikeid,
    usertype,
    gender,


    -- Crear columna age con valores nulos reemplazados por 46 y limitar el máximo a 80 años
    CASE
        WHEN birth_year IS NOT NULL THEN 
            LEAST(EXTRACT(YEAR FROM CURRENT_DATE) - birth_year, 80)
        ELSE 46
    END AS age,


    -- Convertir tripduration de segundos a minutos
    tripduration / 60 AS tripduration_minutes,


    -- Separar stoptime en fecha y hora
    DATE(stoptime) AS stoptime_date,
    TIME(stoptime) AS stoptime_time,


    -- Crear columna start_time con la resta de stoptime y tripduration
    TIME(TIMESTAMP_SUB(stoptime, INTERVAL tripduration SECOND)) AS start_time


FROM
    `citi_bike_trips.citi_bike_trips`;



Se realizó un análisis exploratorio con los siguientes pasos:
Agrupar datos según variables categóricas
Visualizar las variables categóricas
Aplicar medidas de tendencia central
Visualizar distribución
Aplicar medidas de dispersión
Visualizar el comportamiento de los datos a lo largo del tiempo
Calcular cuartiles, deciles o percentiles
Calcular correlación entre variables
Se aplicó técnica de análisis
Aplicar segmentación por tipo de usuario
Se realizó un análisis descriptivo
Distribución de Edad - Se analizó la distribución de la edad de los usuarios usando gráfico de densidad para visualizar cómo se distribuyen las edades.
Duración de los Viajes - Se examinó la duración de los viajes (tripduration). Se Utilicé estadística descriptiva como la media, mediana, máximo y mínimo. Uso por Género:
Compara la duración del viaje, número de viajes y otros aspectos entre géneros. Puedes usar gráficos de barras o boxplots para estas comparaciones.

Resumì la información en un dashboard de Looker Studio
Representar los datos en una tabla o scorecard
Representar datos a través de gráficos simples
Representar datos a través de gráficos o visuales avanzados
Aplicar opciones de filtro para manejo e interacción
Para presentar resultados se realizó lo siguiente:
Seleccioné gráficos con información relevante
Creamos una presentación en GitHub



Enlaces de interés:


Looker Studio - Dashboard 

