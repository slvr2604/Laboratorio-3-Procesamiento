# Laboratorio-3-Procesamiento  
Lina María Cortes Almonacid  
María Alejandra Torres Cárdenas  
Silvia Lorena Vargas Rueda    

Durante este laboratorio se se realizó la grabación de señales de voz correspondientes a seis participantes, distribuidos equitativamente por género (tres mujeres y tres hombres), se obtuvieron registros acústicos que permitieran el análisis gráfico de las señales vocales, con el fin de calcular parámetros cuantitativos como Shimmer y Jitter. Estos indicadores son fundamentales para evaluar la estabilidad y variabilidad de la voz, y pueden ser utilizados en estudios clínicos, diagnósticos funcionales y caracterización de patrones. Además de dicho cálculo, este laboratorio contempla la aplicación de la Transformada de Fourier a cada una de las señales vocales registradas, con el fin de obtener y graficar su espectro de magnitudes frecuenciales. Este análisis espectral permite visualizar la distribución de energía en función de la frecuencia, facilitando la comparación entre voces masculinas y femeninas.  

A partir del espectro de cada señal, se identificaron y reportaron características acústicas como la intensidad, frecuencia media, entre otras; todo esto será comparado entre los distintos participantes, permitiendo observar variaciones interindividuales y posibles correlaciones con el género o la calidad vocal. El análisis se realizará mediante herramientas computacionales en Colab, empleando bibliotecas como NumPy, SciPy y Matplotlib para el procesamiento y visualización de datos.

### PARTE A – Adquisición de las señales de voz  
Para la captura de las señales de voz, se utilizó la aplicación móvil "MyRecorder", ya que permitía configurar la frecuencia de muestreo. Se estableció una frecuencia de 44 kHz, en cumplimiento del criterio de Nyquist, que indica que la frecuencia de muestreo debe ser al menos el doble de la frecuencia máxima. Dado que la banda audible del ser humano se encuentra aproximadamente entre 20 Hz y 20 kHz, se pusieron 44kHz. Posteriormente, se solicitó a tres hombres y tres mujeres que pronunciaran la misma frase. Las grabaciones fueron almacenadas inicialmente en formato MP4, luego se realizó la conversión a formato .WAV utilizando la plataforma en línea "FreeConvert", para poder subirlas al drive de Colab.  

A continuación se va a mostrar la gráfica de cada grabación en dominio del tiempo y la Transformada de Fourier de cada señal con las respectivas gráficas de espectro de magnitudes frecuenciales.  

## Hombre 1:
**Gráfica**  
PEGAR 
**Transformada de Fourier y espectro de magnitudes frecuenciales.**  
La Transformada de Fourier (TF) es una herramienta matemática fundamental que permite descomponer una señal temporal en sus componentes frecuenciales. En el contexto del procesamiento de voz, su aplicación permite identificar qué frecuencias están presentes en una señal vocal y con qué intensidad. Al aplicar la TF a una señal de voz, se obtiene su espectro de magnitudes frecuenciales, el cual representa la distribución de energía en función de la frecuencia. Este espectro permite visualizar los armónicos, la frecuencia fundamental y otros componentes relevantes del timbre vocal.
Para la Transformada de Fourier se uso en todas las voces las siguientes líneas de código:  

    numero_puntos = len(datos_audio)
    transformada_fft = fft(datos_audio)
    frecuencias = fftfreq(numero_puntos, d=1/frecuencia_muestreo)
    frecuencias_positivas = frecuencias[:numero_puntos//2]
    magnitud_fft = np.abs(transformada_fft[:numero_puntos//2])  
    
A partir de esto se pudo graficar el espectro de magnitudes frecuenciales:  

    plt.figure(figsize=(12,4))
    plt.semilogx(frecuencias_positivas, 20*np.log10(magnitud_fft/np.max(magnitud_fft)), color='darkorange')
    plt.title("Espectro de magnitud ")
    plt.xlabel("Frecuencia [Hz]")
    plt.ylabel("Magnitud [dB]")
    plt.xlim(20, 20000)
    plt.grid(True)
    plt.show()

<img width="1017" height="398" alt="image" src="https://github.com/user-attachments/assets/615212f5-7a63-4562-b137-0c65cc758ce0" />


## Hombre 2:
**Gráfica**
PEGAR  
**Transformada de Fourier y espectro de magnitudes frecuenciales.**
<img width="1017" height="398" alt="image" src="https://github.com/user-attachments/assets/8de920a7-01ff-4d00-9d7b-abf1cb916b93" />



## Hombre 3:
**Gráfica**
PEEGAR  
**Transformada de Fourier y espectro de magnitudes frecuenciales.**
<img width="1017" height="398" alt="image" src="https://github.com/user-attachments/assets/f9fd11aa-5da7-4749-9466-bad1a6accf36" />



## Mujer 1:  
**Gráfica**
PEGAR  
**Transformada de Fourier y espectro de magnitudes frecuenciales.**
<img width="1017" height="398" alt="image" src="https://github.com/user-attachments/assets/019b626d-f5e1-46c4-8748-74911d23c72f" />


## Mujer 2:  
**Gráfica**
PEGAR  
**Transformada de Fourier y espectro de magnitudes frecuenciales.**
<img width="1017" height="398" alt="image" src="https://github.com/user-attachments/assets/1c0311e4-697e-49b8-b8ac-2697821cd696" />


## Mujer 3:  
**Gráfica**
PEGAR  
**Transformada de Fourier y espectro de magnitudes frecuenciales.**
<img width="1017" height="398" alt="image" src="https://github.com/user-attachments/assets/ce257db0-1abc-42d8-bdfe-34958e770411" />




















### PARTE B – Medición de Jitter y Shimmer 
Filtros
Jitter
Shimmer
Gráficas Jitter
Gráficas Shimmer

### PARTE C – Comparación y conclusiones 


### Referencias
- FreeConvert. (s.f.). Convertidor de MP4 a WAV en línea. Recuperado el 6 de octubre de 2025, de https://www.freeconvert.com/es/mp4-to-wav
- Moore, B. C. J. (2012). An Introduction to the Psychology of Hearing (6th ed.). Brill, de https://brill.com/display/title/21354
