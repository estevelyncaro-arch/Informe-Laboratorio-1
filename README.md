# Informe-Laboratorio-1: Analisis 
## Resumen
En el presente laboratorio se desea analizar señales biológicas mediante el programa python, realizando los cálculos estadísticos de dichas señales y realizando las gráficas correspondientes para facilitar y optimizar el análisis de datos.

### Librerías 

Para el desarrollo de este laboratorío se requerían algunas librerías, las librerías que implementamos fueron las siguientes:

```python
import numpy as np
import matplotlib.pyplot as plt
!pip install wfdb
import wfdb
from scipy.stats import kurtosis
from scipy.stats import gaussian_kde
```

## Parte A

En este primer apartado tomamos una señal electrocardiográfica de la plataforma Physionet, la cual estaba compuesta por varias señales por lo que seleccionamos una única señal para poder realizar un análisis de datos más preciso. Para estu realizamos las siguientes líneas de código:
```python
signals,fields=wfdb.rdsamp('/content/43541326')
signals
dedo=signals[:,1]
plt.plot(dedo)
plt.xlabel("Tiempo (s)")
plt.ylabel("Voltaje (mV)")
plt.title("Grafica A")
plt.axis([0,1000,-1,2])
plt.show()
```
Obteniendo la siguiente gráfica:

<img width="596" height="455" alt="image" src="https://github.com/user-attachments/assets/1c62d1c2-1a1d-49b1-b4d8-5e10f74199cc" />

Posteriormente se desarrollan los cálculos estadísticos paso a paso y adicionalmente se desarrollan empleando las funciones de la librería matplotlib.
- Cálculos desarrollados paso a paso:
```python
suma=0
for n in dedo:
  suma= suma+n
contador=0
for c in dedo:
  contador= contador+1
media=suma/contador

print(f"La media es:{media}")

suma2 = 0
contador2 = 0
for num in dedo:
    suma2= suma2+(num-media) ** 2
for c in dedo:
  contador2= contador2+1
varianza =suma2 / (contador2)
desviacion_p=(varianza)**(1/2)

print(f"La Desviacion estandar de la población es: {desviacion_p}")

suma3 = 0
contador3 = 0
for numer in dedo:
    suma3= suma3+(numer-media) ** 2
for c in dedo:
  contador3= contador3+1
varianza =suma3 / (contador3-1)
desviacion_m= (varianza)**(1/2)

print(f"La Desviacion estandar de la muestra es: {desviacion_m}")

c_variacion=(desviacion_p/media)*100

print(f"El coeficiente de variación es: {c_variacion} %")
```
Obteniendo:

La media es: 0.04070400000000012

La Desviacion estandar de la población es: 0.1607941988505795

La Desviacion estandar de la muestra es: 0.1608102806827796

El coeficiente de variación es: 395.03291777363165 %

- Cálculos desarrollados con funciones:
  
  ```python
  media=np.mean(dedo)
  mediana=np.median(dedo)
  desviacion_poblacion=np.std(dedo)
  desviacion_muestra=np.std(dedo,ddof=1)
  curtosis=kurtosis(dedo)
  coefvar=(desviacion_poblacion/media)*100
  
  print(f"media igual a {media}")
  print(f"mediana igual a {mediana}")
  print(f"la desviacion estandar de la poblacion es igual a {desviacion_poblacion}")
  print(f"la desviacion estandar de la muestra es igual a {desviacion_muestra}")
  print(f"la curtosis es igual a {curtosis}")
  print(f"el coeficiente de variacion es igual a {coefvar} %")
  ```

  Obteniendo:

  media igual a 0.040704000000000004

  mediana igual a 0.0

  la desviacion estandar de la poblacion es igual a 0.16079419885058044

  la desviacion estandar de la muestra es igual a 0.16081028068278053
    
  el coeficiente de variacion es igual a 395.0329177736351 %

  la curtosis es igual a 14.97171146732827

## Parte B
Para la segunda parte de este laboratorio, se utilizó una señal proveniente de un generador de señales biológicas. Esta señal fue procesada mediante un microcontrolador que fue un Arduino, el cual se encargó de capturarla y almacenarla. Posteriormente, se realizó un código en Python para visualizar la señal y evidenciar el análisis estadístico . Finalmente, los resultados obtenidos se compararon con los de la señal registrada en la primera parte del laboratorio. 
