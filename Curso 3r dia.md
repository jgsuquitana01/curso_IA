TERCER DIA:


![[Pasted image 20230727010753.png]]

El programa a realizar se basara en la probabilidad que un pasajero del Titanic pueda o pudo sobrevivir a este gran desastre: 

```python
##########LIBRERÍAS A UTILIZAR########## #Se importan la librerias a utilizar import numpy as np import pandas as pd from sklearn.model_selection import train_test_split from sklearn.linear_model import LogisticRegression from sklearn.svm import SVC from sklearn.neighbors import KNeighborsClassifier 


##########IMPORTANDO LA DATA########## #Se guardan los datos en un archivo para 

siempre tenerlos disponibles dir_test = '/Users/devgh/OneDrive/Instituto/Cursos/test.csv' dir_train = '/Users/devgh/OneDrive/Instituto/Cursos/train.csv' 
#Importar los datos de los archivos .csv almacenados 
df_test = pd.read_csv(dir_test) df_train = pd.read_csv(dir_train) print(df_test.head()) print(df_train.head()) 
##########ENTENDIMIENTO DE LA DATA########## 
#Verifico la cantidad de datos que hay en los dataset 
print('Cantidad de datos:') print(df_train.shape) print(df_test.shape) 
#Verifico el tipo de datos contenida en ambos dataset 
print('Tipos de datos:') print(df_train.info()) print(df_test.info()) 
#Verifico los datos faltantes de los dataset 
print('Datos faltantes:') print(pd.isnull(df_train).sum()) print(pd.isnull(df_test).sum()) 
#Verifico las estadísticas del dataset 
print('Estadísticas del dataset:') print(df_train.describe()) print(df_test.describe()) 
##########PREPROCESAMIENTO DE LA DATA########## 
#Cambio los datos de sexos en números 
df_train['Sex'].replace(['female','male'],[0,1],inplace=True) df_test['Sex'].replace(['female','male'],[0,1],inplace=True) 
#Cambio los datos de embarque en números
df_train['Embarked'].replace(['Q','S', 'C'],[0,1,2],inplace=True) df_test['Embarked'].replace(['Q','S', 'C'],[0,1,2],inplace=True) 
#Reemplazo los datos faltantes en la edad por la media de esta columna 
print(df_train["Age"].mean()) print(df_test["Age"].mean()) promedio = 30 df_train['Age'] = df_train['Age'].replace(np.nan, promedio) df_test['Age'] = df_test['Age'].replace(np.nan, promedio) 
#Creo varios grupos de acuerdo a bandas de las edades #
Bandas: 0-8, 9-15, 16-18, 19-25, 26-40, 41-60, 61-100 bins = [0, 8, 15, 18, 25, 40, 60, 100] names = ['1', '2', '3', '4', '5', '6', '7'] 
df_train['Age'] = pd.cut(df_train['Age'], bins, labels = names) df_test['Age'] = pd.cut(df_test['Age'], bins, labels = names) 
#Se elimina la columna de "Cabin" ya que tiene muchos datos perdidos 
df_train.drop(['Cabin'], axis = 1, inplace=True) df_test.drop(['Cabin'], axis = 1, inplace=True) 
#Elimino las columnas que considero que no son necesarias para el analisis 
df_train = df_train.drop(['PassengerId','Name','Ticket'], axis=1) df_test = df_test.drop(['Name','Ticket'], axis=1) 
#Se elimina las filas con los datos perdidos 
df_train.dropna(axis=0, how='any', inplace=True) df_test.dropna(axis=0, how='any', inplace=True) 
#Verifico los datos 
print(pd.isnull(df_train).sum()) print(pd.isnull(df_test).sum()) print(df_train.shape) print(df_test.shape) print(df_test.head()) print(df_train.head()) 
##########APLICACIÓN DE ALGORITMOS DE MACHINE LEARNING########## 
#Separo la columna con la información de los sobrevivientes 
X = np.array(df_train.drop(['Survived'], 1)) y = np.array(df_train['Survived']) #Separo los datos de "train" en entrenamiento y prueba para probar los algoritmos 
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2) ##Regresión logística 
logreg= LogisticRegression() logreg.fit(X_train, y_train) Y_pred = logreg.predict(X_test) print('Precisión Regresión Logística:') print(logreg.score(X_train, y_train)) 
##Support Vector Machines 
svc = SVC() svc.fit(X_train, y_train) Y_pred = svc.predict(X_test) print('Precisión Soporte de Vectores:') print(svc.score(X_train, y_train)) 
##K neighbors 
knn = KNeighborsClassifier(n_neighbors = 3) knn.fit(X_train, y_train) Y_pred = knn.predict(X_test) print('Precisión Vecinos más Cercanos:') print(knn.score(X_train, y_train)) 
##########PREDICCIÓN UTILIZANDO LOS MODELOS########## 
ids = df_test['PassengerId'] df_test['Age'] = df_test['Age'].astype(float) ###Regresión logística 
prediccion_logreg = logreg.predict(df_test.drop('PassengerId', axis=1)) out_logreg = pd.DataFrame({ 'PassengerId' : ids, 'Survived': prediccion_logreg }) print('Predicción Regresión Logística:') print(out_logreg.head()) 
##Support Vector Machines
prediccion_svc = svc.predict(df_test.drop('PassengerId', axis=1)) out_svc = pd.DataFrame({ 'PassengerId' : ids, 'Survived': prediccion_svc }) print('Predicción Soporte de Vectores:') print(out_svc.head()) 
##
K neighbors rediccion_knn = knn.predict(df_test.drop('PassengerId', axis=1)) out_knn = pd.DataFrame({ 'PassengerId' : ids, 'Survived': prediccion_knn }) print('Predicción Vecinos más Cercanos:') print(out_knn.head())

```

  
Como resultado de esta actividad tenemos esta lista:


       PassengerId  Pclass                                          Name     Sex  \
    0          892       3                              Kelly, Mr. James    male   
    1          893       3              Wilkes, Mrs. James (Ellen Needs)  female   
    2          894       2                     Myles, Mr. Thomas Francis    male   
    3          895       3                              Wirz, Mr. Albert    male   
    4          896       3  Hirvonen, Mrs. Alexander (Helga E Lindqvist)  female   
    
        Age  SibSp  Parch   Ticket     Fare Cabin Embarked  
    0  34.5      0      0   330911   7.8292   NaN        Q  
    1  47.0      1      0   363272   7.0000   NaN        S  
    2  62.0      0      0   240276   9.6875   NaN        Q  
    3  27.0      0      0   315154   8.6625   NaN        S  
    4  22.0      1      1  3101298  12.2875   NaN        S  
       PassengerId  Survived  Pclass  \
    0            1         0       3   
    1            2         1       1   
    2            3         1       3   
    3            4         1       1   
    4            5         0       3   
    
                                                    Name     Sex   Age  SibSp  \
    0                            Braund, Mr. Owen Harris    male  22.0      1   
    1  Cumings, Mrs. John Bradley (Florence Briggs Th...  female  38.0      1   
    2                             Heikkinen, Miss. Laina  female  26.0      0   
    3       Futrelle, Mrs. Jacques Heath (Lily May Peel)  female  35.0      1   
    4                           Allen, Mr. William Henry    male  35.0      0   
    
       Parch            Ticket     Fare Cabin Embarked  
    0      0         A/5 21171   7.2500   NaN        S  
    1      0          PC 17599  71.2833   C85        C  
    2      0  STON/O2. 3101282   7.9250   NaN        S  
    3      0            113803  53.1000  C123        S  
    4      0            373450   8.0500   NaN        S  
    Cantidad de datos:
    (891, 12)
    (418, 11)
    Tipos de datos:
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 891 entries, 0 to 890
    Data columns (total 12 columns):
     #   Column       Non-Null Count  Dtype  
    ---  ------       --------------  -----  
     0   PassengerId  891 non-null    int64  
     1   Survived     891 non-null    int64  
     2   Pclass       891 non-null    int64  
     3   Name         891 non-null    object 
     4   Sex          891 non-null    object 
     5   Age          714 non-null    float64
     6   SibSp        891 non-null    int64  
     7   Parch        891 non-null    int64  
     8   Ticket       891 non-null    object 
     9   Fare         891 non-null    float64
     10  Cabin        204 non-null    object 
     11  Embarked     889 non-null    object 
    dtypes: float64(2), int64(5), object(5)
    memory usage: 83.7+ KB
    None
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 418 entries, 0 to 417
    Data columns (total 11 columns):
     #   Column       Non-Null Count  Dtype  
    ---  ------       --------------  -----  
     0   PassengerId  418 non-null    int64  
     1   Pclass       418 non-null    int64  
     2   Name         418 non-null    object 
     3   Sex          418 non-null    object 
     4   Age          332 non-null    float64
     5   SibSp        418 non-null    int64  
     6   Parch        418 non-null    int64  
     7   Ticket       418 non-null    object 
     8   Fare         417 non-null    float64
     9   Cabin        91 non-null     object 
     10  Embarked     418 non-null    object 
    dtypes: float64(2), int64(4), object(5)
    memory usage: 36.1+ KB
    None
    Datos faltantes:
    PassengerId      0
    Survived         0
    Pclass           0
    Name             0
    Sex              0
    Age            177
    SibSp            0
    Parch            0
    Ticket           0
    Fare             0
    Cabin          687
    Embarked         2
    dtype: int64
    PassengerId      0
    Pclass           0
    Name             0
    Sex              0
    Age             86
    SibSp            0
    Parch            0
    Ticket           0
    Fare             1
    Cabin          327
    Embarked         0
    dtype: int64
    Estadísticas del dataset:
           PassengerId    Survived      Pclass         Age       SibSp  \
    count   891.000000  891.000000  891.000000  714.000000  891.000000   
    mean    446.000000    0.383838    2.308642   29.699118    0.523008   
    std     257.353842    0.486592    0.836071   14.526497    1.102743   
    min       1.000000    0.000000    1.000000    0.420000    0.000000   
    25%     223.500000    0.000000    2.000000   20.125000    0.000000   
    50%     446.000000    0.000000    3.000000   28.000000    0.000000   
    75%     668.500000    1.000000    3.000000   38.000000    1.000000   
    max     891.000000    1.000000    3.000000   80.000000    8.000000   
    
                Parch        Fare  
    count  891.000000  891.000000  
    mean     0.381594   32.204208  
    std      0.806057   49.693429  
    min      0.000000    0.000000  
    25%      0.000000    7.910400  
    50%      0.000000   14.454200  
    75%      0.000000   31.000000  
    max      6.000000  512.329200  
           PassengerId      Pclass         Age       SibSp       Parch        Fare
    count   418.000000  418.000000  332.000000  418.000000  418.000000  417.000000
    mean   1100.500000    2.265550   30.272590    0.447368    0.392344   35.627188
    std     120.810458    0.841838   14.181209    0.896760    0.981429   55.907576
    min     892.000000    1.000000    0.170000    0.000000    0.000000    0.000000
    25%     996.250000    1.000000   21.000000    0.000000    0.000000    7.895800
    50%    1100.500000    3.000000   27.000000    0.000000    0.000000   14.454200
    75%    1204.750000    3.000000   39.000000    1.000000    0.000000   31.500000
    max    1309.000000    3.000000   76.000000    8.000000    9.000000  512.329200
    29.69911764705882
    30.272590361445783
    Survived    0
    Pclass      0
    Sex         0
    Age         0
    SibSp       0
    Parch       0
    Fare        0
    Embarked    0
    dtype: int64
    PassengerId    0
    Pclass         0
    Sex            0
    Age            0
    SibSp          0
    Parch          0
    Fare           0
    Embarked       0
    dtype: int64
    (889, 8)
    (417, 8)
       PassengerId  Pclass  Sex Age  SibSp  Parch     Fare  Embarked
    0          892       3    1   5      0      0   7.8292         0
    1          893       3    0   6      1      0   7.0000         1
    2          894       2    1   7      0      0   9.6875         0
    3          895       3    1   5      0      0   8.6625         1
    4          896       3    0   4      1      1  12.2875         1
       Survived  Pclass  Sex Age  SibSp  Parch     Fare  Embarked
    0         0       3    1   4      1      0   7.2500       1.0
    1         1       1    0   5      1      0  71.2833       2.0
    2         1       3    0   5      0      0   7.9250       1.0
    3         1       1    0   5      1      0  53.1000       1.0
    4         0       3    1   5      0      0   8.0500       1.0
    Precisión Regresión Logística:
    0.8143459915611815
    Precisión Soporte de Vectores:
    

    C:\Users\devgh\AppData\Local\Temp\ipykernel_5860\2100590058.py:97: FutureWarning: In a future version of pandas all arguments of DataFrame.drop except for the argument 'labels' will be keyword-only.
      X = np.array(df_train.drop(['Survived'], 1))
    C:\ProgramData\anaconda3\Lib\site-packages\sklearn\base.py:432: UserWarning: X has feature names, but LogisticRegression was fitted without feature names
      warnings.warn(
    C:\ProgramData\anaconda3\Lib\site-packages\sklearn\base.py:432: UserWarning: X has feature names, but SVC was fitted without feature names
      warnings.warn(
    C:\ProgramData\anaconda3\Lib\site-packages\sklearn\base.py:432: UserWarning: X has feature names, but KNeighborsClassifier was fitted without feature names
      warnings.warn(
    

    0.659634317862166
    Precisión Vecinos más Cercanos:
    0.8776371308016878
    Predicción Regresión Logística:
       PassengerId  Survived
    0          892         0
    1          893         0
    2          894         0
    3          895         0
    4          896         1
    Predicción Soporte de Vectores:
       PassengerId  Survived
    0          892         0
    1          893         0
    2          894         0
    3          895         0
    4          896         0
    Predicción Vecinos más Cercanos:
       PassengerId  Survived
    0          892         0
    1          893         0
    2          894         0
    3          895         0
    4          896         1


SEGUNDA TAREA DEL DIA 3:
En esta actividad lo que se realizo es el juego snake-ai que mediante la webcam se podrá mover según al dirección de las manos, esto será posible con la herramienta web "Teachable Machine":

![[Pasted image 20230727175908.png]]

Esta herramienta nos permite realizar fotografías para enseñar a una inteligencia que interactúe con gestos, es decir asimila las fotografías con posibles gestos a futuro.
Después de Crear las fotografías descargamos el archivo  de las dos class.
![[Pasted image 20230727180337.png]]


Como punto final copiamos el link de que nos da la herramienta web: 

![[Pasted image 20230727180446.png]]

y Corremos el programa:

![[Pasted image 20230727180845.png]]
