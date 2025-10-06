# Laboratorio-3-Procesamiento  
Lina María Cortes Almonacid  
María Alejandra Torres Cárdenas  
Silvia Lorena Vargas Rueda    

Durante este laboratorio se se realizó la grabación de señales de voz correspondientes a seis participantes, distribuidos equitativamente por género (tres mujeres y tres hombres), se obtuvieron registros acústicos que permitieran el análisis gráfico de las señales vocales, con el fin de calcular parámetros cuantitativos como Shimmer y Jitter. Estos indicadores son fundamentales para evaluar la estabilidad y variabilidad de la voz, y pueden ser utilizados en estudios clínicos, diagnósticos funcionales y caracterización de patrones. Además de dicho cálculo, este laboratorio contempla la aplicación de la Transformada de Fourier a cada una de las señales vocales registradas, con el fin de obtener y graficar su espectro de magnitudes frecuenciales. Este análisis espectral permite visualizar la distribución de energía en función de la frecuencia, facilitando la comparación entre voces masculinas y femeninas.  

A partir del espectro de cada señal, se identificaron y reportaron características acústicas como la intensidad, frecuencia media, entre otras; todo esto será comparado entre los distintos participantes, permitiendo observar variaciones interindividuales y posibles correlaciones con el género o la calidad vocal. El análisis se realizará mediante herramientas computacionales en Colab, empleando bibliotecas como NumPy, SciPy y Matplotlib para el procesamiento y visualización de datos.

### PARTE A – Adquisición de las señales de voz   
Para la captura de las señales de voz, se utilizó la aplicación móvil "MyRecorder", ya que permitía configurar la frecuencia de muestreo. Se estableció una frecuencia de 44 kHz, en cumplimiento del criterio de Nyquist, que indica que la frecuencia de muestreo debe ser al menos el doble de la frecuencia máxima. Dado que la banda audible del ser humano se encuentra aproximadamente entre 20 Hz y 20 kHz, se pusieron 44kHz. Posteriormente, se solicitó a tres hombres y tres mujeres que pronunciaran la misma frase. Las grabaciones fueron almacenadas inicialmente en formato MP4, luego se realizó la conversión a formato .WAV utilizando la plataforma en línea "FreeConvert", para poder subirlas al drive de Colab.  


















### PARTE B – Medición de Jitter y Shimmer 


### PARTE C – Comparación y conclusiones 


### Referencias
- FreeConvert. (s.f.). Convertidor de MP4 a WAV en línea. Recuperado el 6 de octubre de 2025, de https://www.freeconvert.com/es/mp4-to-wav
- Moore, B. C. J. (2012). An Introduction to the Psychology of Hearing (6th ed.). Brill, de https://brill.com/display/title/21354
