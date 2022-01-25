
# Series e indexacción 
_________________________________________________________________________
sr = pd.Series(<list>) ⇒ crear una serie

sr = pd.Series(dic, index=<list_of_indexes>) ⇒ crea una Serie con índices específicos.

sr.values ⇒ obtener los valores de la serie

sr.index ⇒ obtener los índices de la serie

sr.shape ⇒ obtener la dimensión

sr[<index>] ⇒ obtener un elemento de la serie a través de un índice

sr[<list_of_indexes>] ⇒ obtener valores a través de una lista índices.

sr.isnull() ⇒ Lista con True cuando el valor es nulo

sr.notnull() ⇒ Lista con True cuando el valor es no nulo

_____________________________________________________________________________
# Filtrado de datos

df.loc Para ubicar por el nombre de las columnas e indices

df.iloc Para ubicar por número de columna y fila

______________________________________________________________________________
# Indexado y manejo CSV, EXCEL, json, pickle, parquet,dhf

CSV - Es muy versatil ya que solo tiene comas y saltos de linea.

JSON - Tiene un formato muy similar al de un diccionario de Python.

Excel - Permite guardar el archivo en formato .xls para trabajar con el en Excel o Spreadsheets.

Pickle - Permite comprimir la información, es util cuando se tienen tablas grandes.

Parquet - Permite darle un formato que puede usarse en ambientes de Big Data como Hadoop
________________________________________________________________________________________
# Consumo de memoria y disco
- CSV y formatos String : Son simples, requieren alto costo computacional y algo lentos.

- HDF : Gran soporte, adecuado para grandes cantidades de datos, rápido a costo de alto costo computacional.

- Parquet : Puede igualar a hdf e inclusive trabajar por chunks y en paralelo.

- Pickle : Es práctico pero lento con grandes cantidades de dato


https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html

- # Para cargar un archivo basta con tener un dataframe llamado df

    - df.to_csv("path\nombre.csv", sep = ",",index= False) 

    - df.to_excel("path\nombre.xlsx", sep = ",",index= False,sheet_name="numero_hoja")

    - df.to_json("path\nombre.json")

    - df.to_pickle("path\nombre.pkl")

    - df.to_parquet("path\nombre.parquet")

    - df.to_dhf("path\nombre.hdf",key="data",format="table)


- # Para leer un archivo basta con usar
    
    - pd.read_csv("path\nombre.csv)

    - pd.read_excel("path\nombre.xlsx")

    - pd.read_json("path\nombre.json")

    - pd.read_pickle("path\nombre.pkl")

    - pd.read_parquet("path\nombre.parquet")

    - pd.read_hdf("path\nombre.hdfs")

# Estructura de los dataframe

Comandos utiltes:

    - df_meteorites["fall"].unique()  # devuelve los tipos de la columna fall

    - df_meteorites["fall"].value_counts() # cuenta las ocurrencias de cada categoria

# Transformaciones 

# Variables dummies

    - pd.get_dummies(df_meteorites["fall"])

descripcion: En df_meteorites["fall"] es una columna que tiene solo dos catagorias "Fell" y "Found" y pd.get_dummies() crea columnas categoricas para cada uno de estos tipos 

# Formato tiempo 

- pd.to_datetime(df_meteorites["year"],errors="coerce",format="%m/%d/%Y %H:%M:%S %p")

    Donde errors ="coerce " dice que donde haya un formato que no sea tiempo genera una variable no numerica

___________________________________________________________

# Renombrar columna

df_meteorites.rename(columns={"old key" : "new key" }, inplace=True)

- el implace=True hace que no se genere una nueva vairiable sino que se guarda en el mismo dataframe

# Borrar filas o columnas

df_meteorites.drop(["nombre_columna"],axis=1,inplace=True)

df_meteorites.drop(["nombre_fila"],axis=0,inplace=True)

df_meteorites.drop(columns=["nombre_columnas"],index=["indices_columnas"],inplace=True)

# Copia del dataframe

df_copiado=df_meteorite.copy(deep=True)

- el parametro deep es para garantizar que no se modifique el original

# Funciones lambdas

- aplicar una funcion a una columna, donde el nombre de la función es func, con argumentos estaticos en args
    df["nombre_colum"].apply(func,args=())

- df["nombre_columna"].apply(lambda x : x**2)

- df.apply(lambda x : x.mean() )  # Esto calcula el promedio de cada columna del df

- df.apply(labmda x : x.mean() ,axis=0) #calcula por las filas

- Además de apply, también se pueden usar las funciones applymap y map, dependiendo de la necesidad.

- apply() se utiliza para aplicar una función a lo largo de un eje (columna o fila).

- applymap() se usa para aplicar una función a todos los elementos del DataFrame

- map() se usa para sustituir cada valor de una fila por otro valor.

__________________________________________________________
# Multiples indices

https://data.worldbank.org/ : base de datos publica

- pd.melt(df,id_vars=["Country Name"],var_name="year", value_name="pob")

originalmente df tenia en cada columna cada año lo que hace melt es concatenar todas estas columns usando la columna de identificacion "Country Name" y nombre de variable "year" y la columan con los valores que sera "pob"

- df["Country Name"].isin(["Aruba","Colombia"])

devuelve una columan booleana donde es verdadera si esta aruba o colomiba y false en otro caso

# crear multiples indices 

- df_sample.set_index(["Country Name","year"])
En este caso el df_sample tendra dos indices que son el nombre del pais y el año

- df_sample.xs("Colombia","2015")
Realiza un filtro de indice Colombia y el año 2015

-df_countries=df.set_index(["Country Name","year"]).sort_index(ascending=[True,True])
Ordena los indices de manera creciente

- ids=pd.IndexSlice
- df_countries.loc[ids["Aruba":"Colombia","2015":"2017"],:].sort_index()

realiza una seleccion de las filas teniendo un orden especifico y toma 

# Obtener los indices

- df_countries.index.get_level_values(0)
- df_countries.index.get_level_values(1)
obtiene los indices del primer nivel y segundo nivel respectivamente

# Volver los indices columnas

- df_sample.unstack("year")

Cada uno de los indices del año se volveran columnas

# Como tratar datos faltantes

- df.dropna()
dropna es perfecto para elimnar rapidamente las filas con registros faltantes: 

- df.fillna(0) 
Usando la función fillna podremos reemplazarlas por el valor que querramos, en este caso 0.

- df_d = pd.concat([df[['colmna']], df[['colmna']].interpolate()],axis=1)
Por último, Pandas también puede interporlar los valores faltanes calculando el valor que puede haber existido en el medio.


# Group by

- df.groupby('nombre_columna') 
 Agrupa los datos en relacion a la columna seleccionada


- df.groupby('nombre_columna').mean()
 Calcula el valor esperado

- df.groupby('nombre_columna').count()
  cuenta las ocurrencias de cada clase

- df.groupby('nombre_columna')['Otra_columna'].mean()  
    Calcula la media de uno de los elementos de la columna 

# groupby de multiples indices

- df.groupby(['cut','color']).mean()
crea un dataframe con dos indices y luego calcula la media de cada una de las columnas

# Aggregate

- df.groupby(['cut','color']).aggregate([f1,f2,f3])
realiza un agrupamiento de las dos columnas y crea tres columnas donde se aplica las funciones f1,f2 y f3

con un diccionario se puede decir a que columnas quiero aplicar el aggregate
ejemplo:
dict_agg={"carat":[min,max],"price":[np.mean]}
df.groupby(["cut","color"]).agg(dict_agg)

# Crear filtros con filter()
Sea una funcion booleana func,
- df.groupby("cut").filter(func)

# Datos duplicados
Es muy usual que los registros de una base de datos aparezcan más de una vez, así que veamos cómo pandas puede ayudarnos a lidiar con estos casos. Para comenzar, importemos pandas y creemos un DataFrame con dos columnas y algunos datos repetidos.

Para encontrar los registros duplicados usamos duplicated , que marca con True aquellos casos de filas duplicadas:

- df.duplicated()

Podemos usar keep='first' para marcar solo la primera ocurrencia o keep='last' para marcar la última:

- df.duplicated(keep='first')

Identificados los casos duplicados, podemos usar este resultado para filtrar y seleccionar aquellos que no tienen un registro duplicado:

- df[~ df.duplicated()]

Remarco el hecho de que usé negación '~' para ver los registros no duplicados.

si me interesara ver cuáles son los registros duplicados, podemos usar keep=False:

- df.duplicated(keep=False)

 puedes usar el comando 'drop_duplicates' para eliminar los duplicados. Por defecto, la función guarda el primer resultado keep='first':

 - df.drop_duplicates()

 Y si quieres solo borrar duplicados teniendo en cuenta una sola columna, lo puedes hacer mediante una lista nombrando las columnas donde vas a eliminar los duplicados, en este caso, ['a']:

 - df.drop_duplicates(['a'],keep='last')

 - df.groupby("sex")[["porc_propina","total_bill"]].describe()
Realiza una describcion de cada na de las columnas que le ingrese al data set

- df.groupby("sex")["total_bill"].apply(lambda x: x*1.12)

Si se quiere agregas mas funciones se usa aggregate


# Extraer valor con variables categoricas

Para convertir datos en categoritco se uns cut

- pd.cut(df["total_bill"],bins=3)

donde bins es el numero de subconjutos que se van a realizar y usando pd.cut(df["total_bill"],bins=3).value_counts(), se ve cuandos elementos estan en cada categoria

- pd.cut(df["total_bill"],bins=[3,18,35,60])
 en este aso se define los puntos de corte en 3 < 18< 35<60
 y para ver cuantos elementos se encuetran en cada categoria se usa  pd.cut(df["total_bill"],bins=[3,18,35,60]).value_counts()

 # Tablas dinamicas con pivot_table

 Nota: con .reset_index() se resetean los indices y quedan las tablas completas con cada uno de estos

- df.pivot_table(values="total_bill", index="sex","columns="time", aggfunc=funcion)

donde values son los valores que se van a tener en la tabla, index va a ser nuetros indices y columns las columnas, aggfunc lo que hace es aplicar la funcion que se pide, en caso de no tener este parametro esta calcula ma media

# Series de tiempo
Comandos importantes:

- list(df)
Retorna una lista con las columnas del df

Para concatenar dos series donde se tienen indices compartidos se pueden realizar operaciones y si existe un indice que no se comparten se queda un NAN.


Supongamos que se tiene un serie (x_1,x_2,...x_n), entonces con el comando df.diff()  genera una nueva serie (y_1,y_2,...,y_n)
donde 
y_1=Nan
y_2=x_1-x_2
   .
   .
   .
y_n=x_{n-1}-x_n

- df.fillna(dict)

llena los datos faltantes del dataframe, donde en dict hay un diccionario en que las llaves son las columans del dataframe y el valor es el valor del dataset.

- df.cumsum()
Realiza la operacion inversa en el sentido que si se tiene {y_1,y_2,...,y_n} observaciones entonces la suma acumulativa df_diff.cumsum()
devuelve una serie de tiempo {x1,x2,...xn} tal que

x1=y1
x2=y1+y2
x3= x2+y3=y1+y2+y3
x4=x3+y4=y1+y2+y3+y4
  .
  .
  .
xn=sum(y1,y2,y3,...yn)

# resample()

- df.resample('7D').apply(funcion)

para realizar una agrupacion por dias o semanas en el data set y aplicarle una funcion.

ejemplo df.resample('7D').sum() devulve la suma de los datos cada 7 dias 

- df.resample('W-Sun') : agrupa cada domingo

- df.resample('M') : agrupa cada mes

- df.resample('12h') : agrupa cada 12 horas

# Variables nulas en series de tiempo

- df.bfill() 
copia el valor siguiente donde habia un valor nulo es decir si en la entrada
i hay un null .bfill() la remplaza por el valor que esta en la entrada i+1.

- df.ffill()
copia el valor anterior donde habia un valor nulo es decir si en la entrada
i hay un null .bfill() la remplaza por el valor que esta en la entrada i-1.

- df.fillna(valor) remplaza los nulos por el valor ingresado en valor

- df.interpolate() 
donde hay null se pondrá la interpolacion de los datos (el valor medio)

# Crear series de tiempo donde la variable tiempo no es el indice

para esto se se agrupo usando pd.Grouper(key,freq)

donde key es la columna que tiene los indices de la serie de tiempo y freq es cada cuanto se va agrupar, si 
freq="M" es cada mes,
frqe="1D" es cada dia
 ejemplo:
df_cum.groupby(pd.Grouper(key="ObservationDate",freq="M"))[["rate"]].mean()

agrupa cada mes y realiza la media con groupby


# Series de tiempo
Uno diferencia con la serie de tiempo y el data set es que en general las entradas del dataset son doble corchete mientras que en la serie se tiene un solo corchete, un ejemplo es el anterior

- df_cum.groupby(pd.Grouper(key="ObservationDate",freq="M"))[["rate"]].mean()  : esto es un dataset

- df_cum.groupby(pd.Grouper(key="ObservationDate",freq="M"))["rate"].mean() : esto es una serie de tiempo 


# rolling

dada una serie sr el comando .rolling permite hacer promedios con ventanas de frecuencia.

- sr.rolling(windows=7).mean()

- sr.rolling(windows=7).apply(<funcion>)

esto realiza ventanas cada 7 dias, en el sentido que agrupa los datos cada 7 dias

# grafica de series de tiempo

Dada una serie st de tiempo, esta tiene los metodos

- st.plot() hace una grafia de la serie

- df_monthly=df_colombia.resample("M").max()
    realiza una agrupacion de cada mes y mira cual fue su pico en este mes

- df_monthly.plot(figsize=(10,7), kind="bar",stacked=True) 
    En la variable df_monthly hay una serie de tiempo y se grafica una grafica de barras donde el parametro stacked es usado para decir si se superponeo o no las graficas 

- df_year[['Traitment', 'Deaths', 'Recovered', ]].T.plot(figsize=(10,7),kind="pie",subplots=True)

Esta es una grafica tipo pastel y es necesaria trasponer los datos y para cada na de las columnas se tiene una grafica.

- df_colombia["rate"].hist(figsize=(10,7),bins=10)

Realiza un histograma

- df_colombia["rate"].plot(figsize=(10,7),kind="kde")
 realiza la distrubucion


# Documentacion Para las graficas
https://pandas.pydata.org/docs/



# Mas funcione necesarias

- df_temp_averange.xs("Colombia")
Si se quiere escoger un indice en particular se utiliza el paramento .xs("indice")

- df["year"].dt.year
Supongase que en df["year"] hay variables tipo tiempo y se quiere extraer el año, entonces se usa el comando anterior

# renombrar columnas

- df.rename(columns={"old_name": "new_name"},inplace=True)

# boxplot

Para hacer un boxplot se puede usar una tabla pivote de la siguiente forma

- df_t_pivot=df_t_median.pivot_table(values="Temperature",index="date",columns="Country")

esta tabla tiene de columnas los paises, indices los años y los valores son las temperaturas,

ahora al realizar el boxplot el numero de columnas es el numero de cajas que se vana a analizar, y las filas son los valores a los cuales se realizará el analisis

# .melt(id_vars)

el metodo .melt(id_vars=["colum1","colum2"],var_name="nombre_columna") 
recibe como entrada un lista donde se tiene las columans idenficadores las cuales se mantendran intactas,y el resto del columnas del dataset pasaran a ser parte de una nueva columna llamada por var_name , y asi se generan otra columan mas con los valores que tenian cada una de las columnas, y esta columna tendra el nombre de valuess



# pd.merge(df1 ,df2,on=["col1","col2"],how="inner")

donde el how es el metodo en el que se conserva la interseccion y el parametro on, son las columnas que se van a agregar, por lo tanto se conservran ambos datos


- ejemplo:
Si deseo unir a un primer DataFrame (df_left) los datos de un segundo (df_right) preservando las dimensiones del segundo usando una columna que tienen en común (col1), puedo usar:

- df.merge(df_left,df_right,how="rigth",on="col1")


# Problema clasico,
- Sopongase que se tiene un data set en el  cual en las columnas esta dividido por ejemplo por paises y se quiere hacer un filtro para analisar cada pais, entonces esto se hace con
- df_merge.set_index(["Country","date"]).sort_index().loc["Afghanistan":]

- -set_index(["index1","index_2"]) convierte cada una de estas columnas en sus indeces y asi puedo localizar facilmente cada pais usando su nombre


# Graficar resultados 

sns.pairplot(data=df_med.reset_index())

donde df_med es un data set con varias columnas y lo que calcula pairplot es la correlacion entre las columnas

- from pandas.plotting import parallel_coordinates

usando el modulo anterior se puede analizar graficar por paises donde, existe una columna con paises que será nuestra variable que indica cada uno de los coordenadas paralelas

- parallel_coordinates(df_samerica,"Country")