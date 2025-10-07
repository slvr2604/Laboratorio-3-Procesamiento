# Laboratorio-3-Procesamiento  
Lina María Cortes Almonacid  
María Alejandra Torres Cárdenas  
Silvia Lorena Vargas Rueda    

Durante este laboratorio se se realizó la grabación de señales de voz correspondientes a seis participantes, distribuidos equitativamente por género (tres mujeres y tres hombres), se obtuvieron registros acústicos que permitieran el análisis gráfico de las señales vocales, con el fin de calcular parámetros cuantitativos como Shimmer y Jitter. Estos indicadores son fundamentales para evaluar la estabilidad y variabilidad de la voz, y pueden ser utilizados en estudios clínicos, diagnósticos funcionales y caracterización de patrones. Además de dicho cálculo, este laboratorio contempla la aplicación de la Transformada de Fourier a cada una de las señales vocales registradas, con el fin de obtener y graficar su espectro de magnitudes frecuenciales. Este análisis espectral permite visualizar la distribución de energía en función de la frecuencia, facilitando la comparación entre voces masculinas y femeninas.  

A partir del espectro de cada señal, se identificaron y reportaron características acústicas como la intensidad, frecuencia media, entre otras; todo esto será comparado entre los distintos participantes, permitiendo observar variaciones interindividuales y posibles correlaciones con el género o la calidad vocal. El análisis se realizará mediante herramientas computacionales en Colab, empleando bibliotecas como NumPy, SciPy y Matplotlib para el procesamiento y visualización de datos.  

### Diagrama de flujo
Primero hicimos un diagrama de flujo para organizar la programación que se haría, así como el procedimiento:  
IMAGEN

### PARTE A – Adquisición de las señales de voz  
Para la captura de las señales de voz, se utilizó la aplicación móvil "MyRecorder", ya que permitía configurar la frecuencia de muestreo. Se estableció una frecuencia de 44 kHz, en cumplimiento del criterio de Nyquist, que indica que la frecuencia de muestreo debe ser al menos el doble de la frecuencia máxima. Dado que la banda audible del ser humano se encuentra aproximadamente entre 20 Hz y 20 kHz, se pusieron 44kHz. Posteriormente, se solicitó a tres hombres y tres mujeres que pronunciaran la misma frase. Las grabaciones fueron almacenadas inicialmente en formato MP4, luego se realizó la conversión a formato .WAV utilizando la plataforma en línea "FreeConvert", para poder subirlas al drive de Colab.  

A continuación se va a mostrar la gráfica de cada grabación en dominio del tiempo y la Transformada de Fourier de cada señal con las respectivas gráficas de espectro de magnitudes frecuenciales.  

## Hombre 1:
Para graficar en el dominio de tiempo inicialmente se normalizo la amplitud, dividiendo cada muestra por el valor máximo absoluto de la señal para escalar la señal entre –1 y 1, lo que permitió comparar grabaciones en igualdad de condiciones y se evitaron distorsiones visuales o numéricas, al normalizarla la ampiltud queda adimensional, lo que indica cuán grande o pequeña es una parte de la señal comparada con el resto de ella misma.  

       datos_audio = datos_audio / np.max(np.abs(datos_audio))    

       plt.figure(figsize=(12,4))  
       plt.plot(tiempo, datos_audio, color='teal')  
       plt.title("Señal de voz en el dominio del tiempo")  
       plt.xlabel("Tiempo [s]")  
       plt.ylabel("Amplitud")  
       plt.grid(True)  
       plt.show()  

**Gráfica en el dominio tiempo** 

<img width="1021" height="393" alt="image" src="https://github.com/user-attachments/assets/9a793506-c952-44d4-83a4-b41b4dfdcafe" />

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

La **frecuencia fundamental (F₀)** es el componente de menor frecuencia con mayor energía en una señal periódica, es el tono de base de la voz. En el caso de la voz humana, corresponde al tono base que percibimos al hablar o cantar, y sobre el cual se construyen los armónicos, los armonicos son como copias de esa frecuencia, pero más agudas. Por ejemplo, si la frecuencia fundamental es 100 Hz, los armónicos estarán en 200 Hz, 300 Hz, 400 Hz, y así sucesivamente. Estos sonidos no se escuchan como notas separadas, pero juntos le dan a la voz su timbre o color característico. Gracias a los armónicos, podemos distinguir una voz masculina de una femenina, o una flauta de un violín, aunque toquen la misma nota. La F₀ sirve para caracterizar el tipo de voz (masculina, femenina, infantil), detectar alteraciones fonatorias y realizar análisis acústicos clínicos o técnicos. Se midió así:  

    indice_pico_principal = np.argmax(magnitud_fft)
    frecuencia_fundamental = frecuencias_positivas[indice_pico_principal]  
    
**Frecuencia fundamental: 381.8 Hz**  

La **frecuencia media** es un parámetro que se calcula tomando en cuenta cuánto aporta cada frecuencia según su magnitud en el espectro, lo que permite saber hacia qué zona (graves o agudos) está concentrada la energía de la señal. En este caso, a frecuencia media ayuda a caracterizar el timbre, diferenciar voces masculinas y femeninas, y detectar alteraciones acústicas.  

    frecuencia_media = np.sum(frecuencias_positivas * magnitud_fft) / np.sum(magnitud_fft)  
    
**Frecuencia media:** 4512.8 Hz  

El **brillo espectral** es un parámetro que mide cuánta energía hay en las frecuencias altas respecto al total, lo cual resulta útil en el análisis de voz para evaluar qué tan clara, aguda o “brillante” suena una grabación. Su cálculo se basa en la proporción de energía ubicada por encima de un umbral de frecuencia, que en este caso se fijó en 2000 Hz. Según estudios acústicos, dicho umbral puede variar entre 1500 y 3000 Hz dependiendo del tipo de voz y el objetivo fonético del análisis. Dado que en este estudio se incluyeron voces masculinas y femeninas, se adoptó el valor de 2000 Hz como referencia intermedia. Un valor alto de brillo (cercano a 1) indica una voz clara y aguda, con predominancia de frecuencias altas, común en voces femeninas o infantiles; mientras que un valor bajo (cercano a 0) refleja una voz más grave u opaca, con mayor concentración de energía en frecuencias bajas, como suele observarse en voces masculinas o roncas.  

En resumen, si el valor es alto (por ejemplo, 0.6 o más), decimos que la señal es “brillante”. Si es bajo (0.2 o menos), la señal es más grave o apagada.  

    f_umbral = 2000 
    energia_total = np.sum(magnitud_fft)
    energia_alta = np.sum(magnitud_fft[frecuencias_positivas > f_umbral])
    brillo_espectral = energia_alta / energia_total  

**Brillo espectral:** 0.5  

La **intensidad** es un parámetro que representa la cantidad total de energía contenida en una señal. En el análisis de voz, permite estimar qué tan fuerte o potente es una grabación, y puede relacionarse con el esfuerzo fonatorio, la proyección vocal o la presencia de ruido. Este valor no tiene unidades absolutas si se trabaja con señales normalizadas, pero permite comparar entre grabaciones. Por ejemplo, una voz con mayor energía puede indicar mayor volumen, mejor proyección o menor pérdida de señal; se calcula como la suma de las magnitudes del espectro de Fourier:  

    datos_audio = datos_audio / np.max(np.abs(datos_audio))
    energia_total = np.sum(datos_audio**2)  

**Energía total (intensidad):** 2392.245868  

Se midió el SNR para cada señal y para poder estimar la relación entre señal útil y ruido en cada grabación, se implementó un cálculo del SNR en decibelios. Se definió un umbral de ruido equivalente al 10 % del valor máximo absoluto de la señal, lo que permite separar las muestras con energía significativa (parte señal) de aquellas consideradas ruido de fondo (parte ruido). A partir de esta segmentación, se calculó la energía de cada componente y se aplicó la fórmula clásica del SNR en dB:  
    umbral_ruido = 0.1 * np.max(np.abs(datos_audio))
    parte_senal = datos_audio[np.abs(datos_audio) >= umbral_ruido]
    parte_ruido = datos_audio[np.abs(datos_audio) < umbral_ruido]
    energia_senal = np.sum(parte_senal**2)
    energia_ruido = np.sum(parte_ruido **2)
    SNR_dB = 10 * np.log10(energia_senal / energia_ruido)  
    
**Relación Señal/Ruido (SNR):** 8.05 dB  

## Hombre 2:  

**Gráfica en el dominio tiempo**
<img width="1021" height="393" alt="image" src="https://github.com/user-attachments/assets/ac71d54a-483c-4184-bca3-e89e52f59397" />

**Transformada de Fourier y espectro de magnitudes frecuenciales.**
<img width="1017" height="398" alt="image" src="https://github.com/user-attachments/assets/8de920a7-01ff-4d00-9d7b-abf1cb916b93" />
**Frecuencia fundamental:** 144.3 Hz  
**Frecuencia media:** 3604.4 Hz  
**Brillo espectral:** 0.4   
**Energía total (intensidad):** 3712.911768  
**Relación Señal/Ruido (SNR):** 12.23 dB  



## Hombre 3:
**Gráfica en el dominio tiempo**
<img width="1021" height="393" alt="image" src="https://github.com/user-attachments/assets/323fb4c9-997b-4d93-b82b-22c8e107a75a" />

**Transformada de Fourier y espectro de magnitudes frecuenciales.**
<img width="1017" height="398" alt="image" src="https://github.com/user-attachments/assets/f9fd11aa-5da7-4749-9466-bad1a6accf36" />
**Frecuencia fundamental:** 96.8 Hz  
**Frecuencia media:** 3657.5 Hz  
**Brillo espectral:** 0.4  
**Energía total (intensidad):** 3792.482709  
**Relación Señal/Ruido (SNR):** 10.90 dB  


## Mujer 1:  
**Gráfica en el dominio tiempo**
<img width="1021" height="393" alt="image" src="https://github.com/user-attachments/assets/73f94198-b428-415f-8620-dcfbf0871283" />

**Transformada de Fourier y espectro de magnitudes frecuenciales.**
<img width="1017" height="398" alt="image" src="https://github.com/user-attachments/assets/019b626d-f5e1-46c4-8748-74911d23c72f" />
**Frecuencia fundamental:** 219.9 Hz  
**Frecuencia media:** 5457.8 Hz  
**Brillo espectral:** 0.6  
**Energía total (intensidad):** 3198.715156  
**Relación Señal/Ruido (SNR):** 11.76 dB  


## Mujer 2:  
**Gráfica en el dominio tiempo**
<img width="1021" height="393" alt="image" src="https://github.com/user-attachments/assets/4835d52f-fe8b-47b6-a8d0-d2fe897b96c3" />
  
**Transformada de Fourier y espectro de magnitudes frecuenciales.**
<img width="1017" height="398" alt="image" src="https://github.com/user-attachments/assets/1c0311e4-697e-49b8-b8ac-2697821cd696" />
**Frecuencia fundamental:** 207.1 Hz  
**Frecuencia media:** 2804.6 Hz  
**Brillo espectral:** 0.3   
**Energía total (intensidad):** 6496.236422  
**Relación Señal/Ruido (SNR):** 13.28 dB  

## Mujer 3:  
**Gráfica en el dominio tiempo**
<img width="1021" height="393" alt="image" src="https://github.com/user-attachments/assets/fda830b0-441a-4b4c-a346-23ced04ef648" />
  
**Transformada de Fourier y espectro de magnitudes frecuenciales.**
<img width="1017" height="398" alt="image" src="https://github.com/user-attachments/assets/ce257db0-1abc-42d8-bdfe-34958e770411" />

**Frecuencia fundamental:** 381.8 Hz  
**Frecuencia media:** 4512.8 Hz  
**Brillo espectral:** 0.5   
**Energía total (intensidad):** 2392.245868  
**Relación Señal/Ruido (SNR):** 8.05 dB  


### PARTE B – Medición de Jitter y Shimmer 

Una vez obtenidas las grabaciones de las seis señales de voz, se seleccionaron dos de ellas, una correspondiente a un participante masculino y otra a un participante femenino con el propósito de realizar un análisis más detallado. Sobre estas señales se aplicó un filtro pasa banda, con el objetivo de eliminar componentes de ruido y conservar únicamente las frecuencias características del rango vocal de cada género.

Jitter
Shimmer
Gráficas Jitter
Gráficas Shimmer

### PARTE C – Comparación, análisis y conclusiones  
Los datos obtenidos de las grabaciones de voz permitieron estudiar y comparar diferentes características entre hombres y mujeres. Para ello no solo se tuvieron en cuenta parámetros generales como la frecuencia fundamental, la frecuencia media, el brillo espectral, la intensidad y la relación señal/ruido, sino también medidas más específicas como el jitter y el shimmer, que reflejan la estabilidad de la voz en cuanto a frecuencia y amplitud. Con todos estos resultados es posible observar cómo varían las voces en términos de tono, claridad y regularidad, y así realizar una comparación más completa entre las muestras analizadas.  
La **frecuencia fundamental (F₀)** fue consistentemente más baja en las voces masculinas (96.8 Hz, 144.3 Hz y 381.8 Hz en promedio entre 100 y 200 Hz) que en las voces femeninas (207.1 Hz, 219.9 Hz y 381.8 Hz, en un rango entre 200 y 400 Hz). Esto concuerda con la teoría acústica de la voz, donde los hombres presentan cuerdas vocales más largas y con mayor masa, lo que genera vibraciones más lentas y por ende tonos más graves. En contraste, las mujeres presentan frecuencias fundamentales más altas, lo que produce un tono más agudo y distintivo.  

**Brillo espectral:** Las voces femeninas tendieron a mostrar valores de brillo más altos (0.5 y 0.6), lo que indica una mayor presencia de energía en frecuencias altas, otorgando un timbre más claro y agudo. En contraste, los hombres se concentraron en valores de 0.4, reflejando un carácter más opaco o grave.  

**Frecuencia media:** Las voces femeninas presentaron frecuencias medias notablemente más elevadas (5457.8 Hz y 4512.8 Hz en dos de los casos) en comparación con los hombres (entre 3600 Hz y 3800 Hz), lo que evidencia que gran parte de la energía vocal femenina se concentra en rangos más agudos.

**Intensidad (energía total):** No se observó un patrón uniforme. En algunos casos, los hombres tuvieron mayor energía (ej. Hombre 2: 3712.9 frente a Mujer 1: 3198.7), mientras que en otros, una voz femenina superó ampliamente en intensidad (ej. Mujer 2 con 6496.2, la más alta de todas). Esto sugiere que la intensidad depende más del esfuerzo fonatorio y de la calidad de grabación que del género de la voz, probablemente por las condiciones que habían al grabar todas las personas hablaron en un tono más alto o bajo con la intención de que se escuchará mejor.  

**Relación Señal/Ruido (SNR):** Hubo variabilidad en ambos géneros. Algunos hombres presentaron valores relativamente bajos (Hombre 3: 10.9 dB), mientras que una mujer obtuvo el mayor valor de SNR (Mujer 2: 13.28 dB), lo que refleja que este parámetro está más asociado a las condiciones de grabación y claridad de la señal que al timbre vocal en sí.  

Las voces masculinas presentan frecuencias fundamentales más bajas y menor brillo espectral, lo que les otorga un timbre más grave y opaco. Las voces femeninas muestran frecuencias fundamentales y medias más altas, con mayor energía en los agudos, lo que produce un timbre más brillante y claro. La intensidad y la relación señal/ruido no dependen directamente del género, sino de factores como el esfuerzo fonatorio, la proyección vocal y las condiciones de la grabación.














### Referencias
- FreeConvert. (s.f.). Convertidor de MP4 a WAV en línea. Recuperado el 6 de octubre de 2025, de https://www.freeconvert.com/es/mp4-to-wav
- Moore, B. C. J. (2012). An Introduction to the Psychology of Hearing (6th ed.). Brill, de https://brill.com/display/title/21354
- Oppenheim, A. V., & Schafer, R. W. (2010). Discrete-Time Signal Processing (3rd ed.). Pearson Education. Recuperado de https://www.pearson.com/en-us/subject-catalog/p/discrete-time-signal-processing/P200000003760/9780131988422
- Peeters, G. (2004). A large set of audio features for sound description (similarity and classification) in the CUIDADO project. IRCAM de https://recherche.ircam.fr/equipes/analysesynthese/peeters/ARTICLES/Peeters_2003_cuidadoaudiofeatures.pdf
- Boersma, P., & Weenink, D. (2023). Praat: Doing phonetics by computer [Computer program]. Version 6.3. https://www.fon.hum.uva.nl/praat/
- 
