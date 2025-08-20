# Informe-Laboratorio-1: Análisis Estadístico de la señal
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

En este primer apartado tomamos una señal electrocardiográfica de la plataforma Physionet, la cual estaba compuesta por varias señales por lo que seleccionamos una única señal para poder realizar un análisis de datos más preciso. 

Se realizo el siguiente diagrama de flujo:

![Imagen de WhatsApp 2025-08-19 a las 20 08 28_246ff55d](https://github.com/user-attachments/assets/3b0a878c-f986-4dc4-94ba-045f9d604e40)

Para este realizamos las siguientes líneas de código:

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

La curtosis da 14.97171146732827 lo que significa que la distribucion tiene un pico muy alto y angosto, tiene colas muy largas lo que indica precensia de valores atipicos 

  Finalmente se realiza el histograma de los datos y se le adiciona la función de probabilidad como se muestra a continuación:
- Histograma:
  ```python
    plt.hist(dedo)
    plt.grid()
    plt.xlabel("Voltaje (mV)")
    plt.ylabel("Frecuencia")
    plt.title("Histograma A")
    plt.show()
  ```
  
Obteniendo el siguiente gráfico:

<img width="580" height="455" alt="image" src="https://github.com/user-attachments/assets/e2f33e8a-c88a-4f06-a338-8e7ff9829529" />

- Histograma con función de probabilidad:
  ```python
    plt.hist(dedo,bins=30, density=True, alpha=0.8, color='pink')

    kde = gaussian_kde(dedo)
    x_vals = np.linspace(min(dedo), max(dedo), 1000)
    plt.plot(x_vals, kde(x_vals), color='cyan', linewidth=2)

    plt.grid()
    plt.xlabel("Voltaje (mV)")
    plt.ylabel("Densidad de probabilidad")
    plt.title("Histograma A con función de probabilidad")
    plt.show()
  ```
  
Obteniendo el siguiente gráfico:

  <img width="596" height="455" alt="image" src="https://github.com/user-attachments/assets/70ea64fd-04d5-44fd-b9f9-321945e4fb87" />

  
## Parte B
Para la segunda parte de este laboratorio, se utilizó una señal proveniente de un generador de señales biológicas. Esta señal fue procesada mediante un microcontrolador que fue un Arduino, el cual se encargó de capturarla y almacenarla. 

Se realizo el siguiente diagrama de flujo:

![Imagen de WhatsApp 2025-08-19 a las 21 08 32_0802b95c](https://github.com/user-attachments/assets/fc93a194-c1b5-444b-90a5-da1bebd39792)



Utilizando las siguientes librerias 
  ```python
import matplotlib.pyplot as plt
import pandas as pd
  ```

Realizando asi el siguiente codigo:

  ```python
data = pd.read_csv("/content/ecg_reconstruido_segmento.txt", sep="\t", header=None, names=["Tiempo", "ECG"])

plt.plot(data["Tiempo"], data["ECG"])
plt.title("Señal ECG Reconstruida")
plt.xlabel("Tiempo (s)")
plt.ylabel("Amplitud (mV)")
plt.grid(True)
plt.show()
 ```
 
Para así lograr la siguiente grafica:

<img width="596" height="455" alt="image" src="https://github.com/user-attachments/assets/8600bbf6-15af-40f9-8d93-fe1d9402d9d0" />

Para luego realizar los calculos paso a paso como en la parte A:
  ```python
data = data["ECG"]

suma=0
for n in data:
  suma= suma+n
contador=0
for c in data:
  contador= contador+1
media=suma/contador
print(f"La media es:{media}")

suma2 = 0
contador2 = 0
for num in data:
    suma2= suma2+(num-media) ** 2
for c in data:
  contador2= contador2+1

varianza =suma2 / (contador2)
desviacion_p=(varianza)**(1/2)
print(f"La Desviacion estandar de la población es: {desviacion_p}")

suma3 = 0
contador3 = 0
for numer in data:
    suma3= suma3+(numer-media) ** 2
for c in data:
  contador3= contador3+1

varianza =suma3 / (contador3-1)
desviacion_m= (varianza)**(1/2)
print(f"La Desviacion estandar de la muestra es: {desviacion_m}")

c_variacion=(desviacion_p/media)*100
print(f"El coeficiente de variación es: {c_variacion} %")
 ```
Obteniendo así los siguientes resultados:
La media es:425.55

La Desviacion estandar de la población es: 63.938341392313355

La Desviacion estandar de la muestra es: 63.97033455988576

El coeficiente de variación es: 15.024871670147657 %
 
- Calculos con funciones

  ```python
  data =data["ECG"]
  media = np.mean(data)
  mediana = np.median(data)
  desviacion_poblacion = np.std(data)
  desviacion_muestra = np.std(data, ddof=1)
  curtosis_val = kurtosis(data)
  coefvar = (desviacion_poblacion / media) * 100

  print(f"Media: {media}")
  print(f"Mediana: {mediana}")
  print(f"Desviación estándar población: {desviacion_poblacion}")
  print(f"Desviación estándar muestra: {desviacion_muestra}")
  print(f"Curtosis: {curtosis_val}")
  print(f"Coeficiente de variación: {coefvar}%")
  ```

Obteniendo los siguientes resultados:

Media: 425.55

Mediana: 411.0

Desviación estándar población: 63.938341392313355

Desviación estándar muestra: 63.97033455988576

Coeficiente de variación: 15.024871670147657%

Curtosis: 5.278386377547983

La curtosis da 5.278386377547983 lo que significa que el pico centra es más alto que una distrubucion normal, tiene colas más pesadas, es mas alto pero más ancho. 

Finalmente, se representa gráficamente la distribución de los datos mediante un histograma, al cual se le añade la curva que describe la función de probabilidad correspondiente.

-Histograma:

  ```python
  plt.hist(data)
  plt.grid()
  plt.xlabel("Voltaje (mV)")
  plt.ylabel("Frecuencia")
  plt.title("Histograma B")
  plt.show()
```

A continuación, se muestra una gráfica:

<img width="596" height="455" alt="image" src="https://github.com/user-attachments/assets/f0ab3adf-03a6-484d-a869-785f649150d0" />

-Histograma con funcion de probavilidad 

  ```python
  plt.hist(data,bins=30, density=True, alpha=0.8, color='purple')

  kde = gaussian_kde(data)
  x_vals = np.linspace(min(data), max(data), 1000)
  plt.plot(x_vals, kde(x_vals), color='orange', linewidth=2)

  plt.grid()
  plt.xlabel("Voltaje (mV)")
  plt.ylabel("Densidad de probabilidad")
  plt.title("Histograma B con función de probabilidad")
  plt.show()
  ```

  A continuación, se muestra una gráfica:
  
<img width="596" height="455" alt="image" src="https://github.com/user-attachments/assets/55d1ec3b-c004-44d8-9ec7-c8205465fea6" />


Para terminar este apartado se lleva a cabo una comparación entre los resultados obtenidos de la parte A y la parte B 

### Comparación de Datos Estadísticos


| **Métrica**              | **Señal A** | **Señal B** | **Interpretación** |
|-------------------------|----------------|----------------|---------------------|
| **Media**               | 0.0407         | 425.55         | La señal B tiene valores mucho más altos . |
| **Mediana**             | 0.0            | 411.0          | La señal A sus datos tienden a 0 y la señal B a 411.0. |
| **Desv. Estándar (Pob.)** | 0.1608       | 63.9383        | A la señal B le varian más los datos que a la señal A. |
| **Desv. Estándar (Mues.)** | 0.1608      | 63.9703        | Anbas señales tienen misma tendencia que arriba. |
| **Coef. Variación (%)** | 395.03%        | 15.02%         | La señal A es muy inestable a comparacion de la señal B que es más consistente. |
| **Curtosis**            | 14.97          | 5.27           | La señal A tiene más valores extremos que la señal B. |

## Parte C

La gráfica de la señal obtenida en la parte B se retoma en esta última parte de la práctica, para así poder contaminar la grafica con tres distintos ruidos, tambien se realizo un diagrama de flujo como se evidencia a continuación:

![Imagen de WhatsApp 2025-08-19 a las 21 09 48_6f3fd5f1](https://github.com/user-attachments/assets/7aa75d31-a2f9-4708-bed9-7bda6a436776)


### 1) Contaminación de la grafica con ruido gaussiano

El ruido gaussiano es un tipo de ruido aleatorio que sigue una distribución normal (o gaussiana) en cuanto a su amplitud.Este ruido es tipico de procesos termico y ruido blanco.

se realizo el siguiente codigo para el ruido gaussiano

 ```Python
  ruido = np.random.normal(8, 6, size=data["ECG"].shape)
  y = data["ECG"] + ruido

  Psignal = np.mean(data["ECG"]**2)
  Pnoise  = np.mean((y - data["ECG"])**2)
  SNR_dB  = 10 * np.log10(Psignal / Pnoise)
  print("SNR (dB):", SNR_dB)

  plt.plot(data["Tiempo"], data["ECG"], label="Señal original")
  plt.plot(data["Tiempo"], y, label="Señal con ruido", alpha=0.7)
  plt.title(f"Señal ECG con y sin ruido (SNR ≈ {SNR_dB:.2f} dB)")
  plt.xlabel("Tiempo (s)")
  plt.ylabel("Amplitud (mV)")
  plt.legend()
  plt.grid(True)
  plt.show()
 ```

Teniendo así la siguiente grafica con su respectivo SNR:

SNR (dB): 32.76257930112977

<img width="596" height="455" alt="image" src="https://github.com/user-attachments/assets/d8309307-9dd9-480f-a81b-1412390cb35a" />

Obteniendo un SNR de 32.76257930112977 lo que significa que la señal es mucho más fuerte que el ruido 

### 2) Contaminación de la grafica con ruido impulso

Este reuido es un tipo de interferencia o distorcción que se caracteriza principalmente por pulsos breves, repentinos y de gran amplitud que aparece de manera espontanea en una señal.

Para este ruido se realizo el siguiente codigo:

 ```python
x = data["ECG"]


prob = 0.05   # PORCENTAJE DE PUNTOS  ALTERAADOS
amplitud = np.max(x) * 0.5  # TAMAÑO DEL IMPULSO

mask = np.random.rand(len(x)) < prob
ruido = np.zeros_like(x)
ruido[mask] = np.random.choice([+amplitud, -amplitud], size=mask.sum())
y = x + ruido


Psignal = np.mean(x**2)
Pnoise  = np.mean((y - x)**2)
SNR_dB  = 10*np.log10(Psignal / Pnoise)
print(f"SNR (dB): {SNR_dB:.2f}")


plt.plot(data["Tiempo"], x, label="Señal original")
plt.plot(data["Tiempo"], y, label="Señal con ruido impulso", alpha=0.8)
plt.xlabel("Tiempo (s)")
plt.ylabel("Amplitud (mV)")
plt.title(f"ECG con ruido impulso (SNR ≈ {SNR_dB:.1f} dB)")
plt.legend()
plt.grid(True)
plt.show()
 ```

A continuacion la grafica con su respectivo SNR:

SNR (dB): 13.39

<img width="596" height="455" alt="image" src="https://github.com/user-attachments/assets/2d6f50c0-9bfa-4800-8f9d-baf1afb91829" />


Obteniendo un SNR de 13.39  lo que significa que la señal es más fuerte que el ruido 

### 3) Contaminación de la grafica con ruido tipo artefacto

El ruido tipo artefacto es un tipo de distorción no deseada que aparece en señales, imágenes, audio o video debido a procesos externos o limitaciones técnicas del sistema de adquisición, transmisión o compresión 

Para este ruido se realizo el siguiente codigo:

```python
t = data["Tiempo"]
x = data["ECG"]

f_bw = 0.3                 # Hz, deriva de linea base
A_bw = 0.3*np.std(x)       # amplitud de la deriva (ajusta 0.1–0.5)

f_pl = 60.0
A_pl = 0.1*np.std(x)       # amplitud de interferencia de red

artefacto = A_bw*np.sin(2*np.pi*f_bw*t) + A_pl*np.sin(2*np.pi*f_pl*t)
y = x + artefacto


Psignal = np.mean(x**2)
Pnoise  = np.mean((y - x)**2)
SNR_dB  = 10*np.log10(Psignal / Pnoise)
print(f"SNR (dB): {SNR_dB:.2f}")


plt.plot(t, x, label="Señal original")
plt.plot(t, y, label="Con artefacto", alpha=0.8)
plt.xlabel("Tiempo (s)"); plt.ylabel("Amplitud (mV)")
plt.title(f"ECG con ruido tipo artefacto (SNR ≈ {SNR_dB:.1f} dB)")
plt.legend(); plt.grid(True); plt.show()
```

A continuacion la grafica con su respectivo SNR:

SNR (dB): 29.57

<img width="596" height="455" alt="image" src="https://github.com/user-attachments/assets/6bbe92fe-9c01-439c-8797-ac9f0e9cbcb0" />

Obteniendo un SNR de 29.57 lo que significa que la señal es 29.57 veces la potencia del ruido teniendo asi buena calidad 



