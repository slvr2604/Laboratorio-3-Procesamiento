# Laboratorio-3-Procesamiento  
Lina María Cortes Almonacid  
María Alejandra Torres Cárdenas  
Silvia Lorena Vargas Rueda    


Durante este laboratorio se se realizó la grabación de señales de voz correspondientes a seis participantes, distribuidos equitativamente por género (tres mujeres y tres hombres), se obtuvieron registros acústicos que permitieran el análisis gráfico de las señales vocales, con el fin de calcular parámetros cuantitativos como Shimmer y Jitter. Estos indicadores son fundamentales para evaluar la estabilidad y variabilidad de la voz, y pueden ser utilizados en estudios clínicos, diagnósticos funcionales y caracterización de patrones. Además de dicho cálculo, este laboratorio contempla la aplicación de la Transformada de Fourier a cada una de las señales vocales registradas, con el fin de obtener y graficar su espectro de magnitudes frecuenciales. Este análisis espectral permite visualizar la distribución de energía en función de la frecuencia, facilitando la comparación entre voces masculinas y femeninas.  

A partir del espectro de cada señal, se identificaron y reportaron características acústicas como la intensidad, frecuencia media, entre otras; todo esto será comparado entre los distintos participantes, permitiendo observar variaciones interindividuales y posibles correlaciones con el género o la calidad vocal. El análisis se realizará mediante herramientas computacionales en Colab, empleando bibliotecas como NumPy, SciPy y Matplotlib para el procesamiento y visualización de datos.  

### Diagrama de flujo
Como primera etapa, se elaboró un diagrama de flujo que permitió estructurar tanto la programación como el procedimiento general del análisis. Esta herramienta facilitó la organización lógica de las tareas, definiendo el orden de ejecución, las funciones necesarias y los criterios de procesamiento que se aplicarían a las señales vocales.

![WhatsApp Image 2025-10-06 at 20 09 51](https://github.com/user-attachments/assets/55a8f93e-e769-4f60-b5ca-e01fad2b21c9)

### PARTE A – Adquisición de las señales de voz  
Para la captura de las señales de voz, se utilizó la aplicación móvil "MyRecorder", ya que permitía configurar la frecuencia de muestreo. Se estableció una frecuencia de 44 kHz, en cumplimiento del criterio de Nyquist, que indica que la frecuencia de muestreo debe ser al menos el doble de la frecuencia máxima. Dado que la banda audible del ser humano se encuentra aproximadamente entre 20 Hz y 20 kHz, se pusieron 44kHz. Posteriormente, se solicitó a tres hombres y tres mujeres que pronunciaran la misma frase. Las grabaciones fueron almacenadas inicialmente en formato MP4, luego se realizó la conversión a formato .WAV utilizando la plataforma en línea "FreeConvert", para poder subirlas al drive de Colab.  

A continuación se va a mostrar la gráfica de cada grabación en dominio del tiempo y la Transformada de Fourier de cada señal con las respectivas gráficas de espectro de magnitudes frecuenciales.  

## Hombre 1:
En cuanto a la adqision de la señal se cuenta con estos datos:
![Imagen de WhatsApp 2025-10-06 a las 22 15 44_c9309c23](https://github.com/user-attachments/assets/30cfea44-1797-42d7-8fa6-aa46a1bd40f8)  
Cabe destacar que el nivel de micrófono (mic level) o ganancia fue ajustado automáticamente por el sistema durante todas las grabaciones, sin posibilidad de configuración manual en la herramienta MyRecorder. Por tanto, no es posible conocer con precisión el valor aplicado en cada caso. Adicionalmente, las grabaciones se realizaron en un entorno no controlado, con presencia constante de ruido ambiental debido al tránsito de personas y sonidos externos, lo cual pudo afectar la calidad acústica de las señales y los datos obtenidos. Esta información aplica de igual forma para todas las grabaciones. 

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
![Imagen de WhatsApp 2025-10-06 a las 22 16 01_a08de259](https://github.com/user-attachments/assets/2dcd7364-c6ce-4cba-a532-ec78fbc6c7b0)


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
![Imagen de WhatsApp 2025-10-06 a las 22 16 19_c3f1fc3c](https://github.com/user-attachments/assets/cbf6d51f-71f7-4026-bd3f-f2a32f10a2f0)

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
![Imagen de WhatsApp 2025-10-06 a las 22 16 37_e7c08ff6](https://github.com/user-attachments/assets/65db78d4-053f-4bd1-a112-68607ecb33b4)  

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
![Imagen de WhatsApp 2025-10-06 a las 22 16 56_6987f4ae](https://github.com/user-attachments/assets/ff9fc276-3aa6-46bf-aa66-6d9897608523)

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
![Imagen de WhatsApp 2025-10-06 a las 22 17 16_a224ae99](https://github.com/user-attachments/assets/67239958-88f3-4986-982e-992e85905c16)

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


    def bandpass_filter(signal, fs, lowcut, highcut, order=2):
    signal = signal / np.max(np.abs(signal))
    nyq = fs / 2
    low = lowcut / nyq
    high = highcut / nyq
    sos = butter(order, [low, high], btype='band', output='sos')
    return sosfiltfilt(sos, signal)


    sig_h, fs_h = sf.read('/content/drive/MyDrive/Colab Notebooks/hombre 3.wav')
    sig_m, fs_m = sf.read('/content/drive/MyDrive/Colab Notebooks/mujer 2.wav')


    if sig_h.ndim > 1:
    sig_h = sig_h.mean(axis=1)
    if sig_m.ndim > 1:
    sig_m = sig_m.mean(axis=1)


    filtro_h = bandpass_filter(sig_h, fs_h, 80, 400)
    filtro_m = bandpass_filter(sig_m, fs_m, 150, 500)


El código implementado en Colab permitió realizar todo el procesamiento descrito de manera estructurada. Se definió una función de filtrado pasa banda basada en la librería scipy.signal mediante el método de Butterworth en formato sos, garantizando estabilidad numérica. Además, se implementó una función auxiliar para extraer un fragmento central de duración controlada, con el fin de analizar un tramo representativo sin bordes ruidosos. Finalmente, las gráficas se generaron con matplotlib, permitiendo comparar visualmente las señales originales y filtradas, y evidenciar el efecto del preprocesamiento sobre las características espectrales de la voz.

<img width="1189" height="789" alt="image" src="https://github.com/user-attachments/assets/8da9a113-850a-42d0-b5b4-113318ad9b15" />

En la Figura se observan las formas de onda correspondientes a las grabaciones de voz masculina y femenina, tanto en su versión original como después de aplicar el filtrado pasa banda. Para la voz masculina se utilizó un filtro de segundo orden con un rango de paso entre 80 y 400 Hz, mientras que para la voz femenina se empleó un filtro con un rango de 150 a 500 Hz, de acuerdo con los valores típicos del rango fundamental de cada género. En ambas señales se aprecia un comportamiento periódico característico de la vibración de las cuerdas vocales, con amplitudes mayores en los segmentos activos de fonación y menor energía en los intervalos de silencio.

El fragmento temporal de 0.3 segundos permite evidenciar con mayor claridad las variaciones de amplitud y la estructura cíclica de la señal. Tras aplicar el filtro, se observa una atenuación de componentes de alta frecuencia y ruido, lo que mejora la visualización del patrón periódico fundamental. En la señal masculina, el filtrado conserva oscilaciones más espaciadas, lo que sugiere una frecuencia fundamental menor, mientras que en la voz femenina las oscilaciones son más densas, lo que indica una frecuencia fundamental mayor.


Cuando calculamos el jitter absoluto y relativo, tenemos en cuenta las siguientes fórmulas: 
<img width="442" height="110" alt="image" src="https://github.com/user-attachments/assets/16d6624d-b4da-4fe7-b538-9a2c5129ebd8" />
<img width="486" height="108" alt="image" src="https://github.com/user-attachments/assets/c6bcc8eb-ffc6-42c8-b32d-c39ecb099ce3" />

Y las adaptamos al código para calcularlo de la siguiente forma: 


```
peaks_h, _ = find_peaks(filtro_h, height=0, distance=fs_h//200)
peaks_m, _ = find_peaks(filtro_m, height=0, distance=fs_m//300)

# cálculo de los periodos Ti
Ti_h = np.diff(peaks_h) / fs_h
Ti_m = np.diff(peaks_m) / fs_m

# jitter absoluto
JitterAbs_h = np.mean(np.abs(np.diff(Ti_h)))
JitterAbs_m = np.mean(np.abs(np.diff(Ti_m)))

# jitter relativo (%)
JitterRel_h = (JitterAbs_h / np.mean(Ti_h)) * 100
JitterRel_m = (JitterAbs_m / np.mean(Ti_m)) * 100
```


Cuando observamos `peaks_h` y `peaks_m` son para detectar máximos locales, es decir los puntos más altos de la señal. `height = 0` detecta los picos mayores a cero y `distance = fs_h/200` limita la distancia entre los picos.
Para el cálculo de los periodos Ti se calcula la diferencia en muestras entre picos consecutivos, que son el número de muestras que dura cada ciclo, y lo divide entre la frecuencia de muestreo.
En el jitter absoluto `np.diff(Ti_h) ` mide las diferencias entre los periodos sucesivos `np.abs()` toma el valor absoluto y `np.mean() ` saca el promedio de esas variaciones.
Y cuando observamos los porcentajes de jitter absoluto y relativo y los picos detectados de las gráficas podemos concluir que:
Los porcentajes de jitter indican que la voz masculina presenta una mayor variabilidad temporal entre los periodos de vibración de las cuerdas vocales que la voz femenina.
La voz del hombre tiene un Jitter ligeramente superior, lo que sugiere mayor irregularidad en la frecuencia de vibración, mientras que la voz de la mujer, con un Jitter menor, esta muestra una frecuencia fundamental más estable, es decir, los ciclos de vibración son más regulares en el tiempo.
Si nos enfocamos en las gráficas de los picos:
 <img width="886" height="404" alt="image" src="https://github.com/user-attachments/assets/3dabd1a6-2e1c-4107-9397-ef0d97706536" />
<img width="886" height="404" alt="image" src="https://github.com/user-attachments/assets/d66ce73d-ca20-4dbb-a3a9-7ea099f78c63" />

En los picos de la voz del hombre los picos se distribuyen con una separación bastante regular, lo que indica que el periodo de vibración (T_i) es constante. Esto indica que la frecuencia fundamental se mantiene estable en el tiempo. La variación de los picos es leve, por lo que el Jitter relativo es bajo o moderado. Y en la gráfica se observa una señal más uniforme y menos afectada por variaciones rápidas, lo que sugiere buena calidad vocal y menor tensión o fatiga en la voz.
En los picos de la voz femenina los picos sucesivos presentan ligeras variaciones en la separación, lo que indica que los periodos T_icambian de forma más notoria. Esto nos muestra un mayor Jitter, reflejando una frecuencia fundamental menos estable. Esto puede ocurrir porque las voces femeninas suelen tener una frecuencia más alta, lo que hace que el jitter pueda verse más pronunciado.
Si continuamos al cálculo del shimmer absoluto y relativo, tenemos en cuenta las siguientes fórmulas:

 <img width="472" height="103" alt="image" src="https://github.com/user-attachments/assets/7b1212a6-abb0-46de-a48d-5d619641225a" />
<img width="520" height="88" alt="image" src="https://github.com/user-attachments/assets/cf883047-3ae0-4325-8ff0-914c0b561fde" />

Y las adaptamos a los datos obtenidos anteriormente para poder calcularlas. El código que la representa es:

```
# Shimmer absoluto
ShimmerAbs_h = np.mean(np.abs(np.diff(A_h)))
ShimmerAbs_m = np.mean(np.abs(np.diff(A_m)))
# Shimmer relativo (%)
ShimmerRel_h = (ShimmerAbs_h / np.mean(A_h)) * 100
ShimmerRel_m = (ShimmerAbs_m / np.mean(A_m)) * 100
```

Previamente hemos calculado los picos de amplitud, con esos picos obtenemos las amplitudes de cada ciclo que se declaran como `A_h` y `A_m`. `np.diff(A_h)` calcula la diferencia entre variaciones es decir Ai+1-Ai `np.abs` toma el valor absoluto de los valores y `np,mean` hace el promedio de las diferencias absolutas, lo que nos permite calcular el shimmer absoluto.
Y para el relativo, se normaliza el shimmer absoluto respecto al promedio de la amplitud y se multiplica por 100, lo que nos da el porcentaje que indica que tanto varía la amplitud en relación al promedio de la señal. Y se procede a hacer la gráfica de los picos.
<img width="886" height="404" alt="image" src="https://github.com/user-attachments/assets/93f17fa0-09eb-4256-86bc-e3f9a5263740" />
<img width="886" height="404" alt="image" src="https://github.com/user-attachments/assets/e1593952-4bf5-4a15-b0e0-f0c96ef88148" />

Ahora, cuando hablamos de los picos de amplitud AI encontramos que la voz masculina presenta un shimmer más alto (33.73%), lo que indica mayores variaciones en la amplitud entre ciclos consecutivos. Y podríamos atribuirlo a que las voces masculinas, al tener una frecuencia fundamental más baja, presentan ciclos más largos y por tanto una mayor susceptibilidad a fluctuaciones en la energía de emisión. La voz femenina tiene un shimmer menor (20.89%), lo que refleja una mayor estabilidad en la intensidad de la señal. Esto porque las voces femeninas suelen ser más agudas y con menor energía en bajas frecuencias, lo que reduce las variaciones de amplitud perceptibles.
Y cuando analizamos las gráficas de los picos de amplitud encontramos que, en la gráfica del hombre, los picos azules presentan una dispersión más irregular. Se observa que la amplitud de los picos varía notablemente a lo largo del tiempo, lo que confirma el valor alto del shimmer calculado. En la voz femenina los picos son más uniformes y constantes, con menor dispersión vertical. Esto coincide con un shimmer más bajo, indicando una señal más estable y con menos fluctuaciones de energía entre ciclos. 
 
Luego  se implementó un código en Colab que permite automatizar el cálculo del jitter y shimmer para las seis grabaciones de voz (tres masculinas y tres femeninas). El programa utiliza la función wavfile.read del módulo scipy.io para importar las señales en formato .wav, asegurando que cada archivo sea procesado con su respectiva frecuencia de muestreo. Posteriormente, se emplea la función find_peaks de scipy.signal para detectar los máximos locales en la señal de voz, lo que permite identificar los ciclos glotales necesarios para calcular las variaciones temporales y de amplitud.


    def calcular_jitter_shimmer(archivo):
    fs, voz = wavfile.read(archivo)

   
    peaks, _ = find_peaks(voz, height=0, distance=fs//200)
    periodos = np.diff(peaks) / fs       
    amplitudes = voz[peaks]             

  
    jitter_val = np.mean(np.abs(np.diff(periodos))) / np.mean(periodos) * 100
    shimmer_val = np.mean(np.abs(np.diff(amplitudes))) / np.mean(amplitudes) * 100

    return jitter_val, shimmer_val


    resultados_hombres = [calcular_jitter_shimmer(f) for f in archivos_hombres]
    resultados_mujeres = [calcular_jitter_shimmer(f) for f in archivos_mujeres]

    
El jitter se calculó como el promedio relativo de las diferencias entre los periodos consecutivos (T_i), mientras que el shimmer se obtuvo a partir de las variaciones en las amplitudes de los picos (A_i). Este procedimiento se encapsuló en una función llamada calcular_jitter_shimmer, que recibe cada archivo como entrada y devuelve ambos valores porcentuales. De esta manera, se logró estandarizar el proceso para todos los participantes y facilitar la comparación entre los resultados.


Valores de Jitter y Shimmer 

Hombre 1: Jitter = 20.24%, Shimmer = 27.83%

Hombre 2: Jitter = 18.81%, Shimmer = 22.86%

Hombre 3: Jitter = 27.18%, Shimmer = 65.59%

Mujer 1: Jitter = 28.50%, Shimmer = 38.66%

Mujer 2: Jitter = 22.15%, Shimmer = 38.03%

Mujer 3: Jitter = 20.50%, Shimmer = 33.43%



Los valores obtenidos muestran una notable variabilidad entre las grabaciones. En las voces masculinas, el jitter osciló entre 18.81 % y 27.18 %, mientras que en las voces femeninas varió entre 20.50 % y 28.50 %. Los resultados de shimmer también presentaron valores elevados, desde 22.86 % hasta 65.59 % en hombres y de 33.43 % a 38.66 % en mujeres. Estos valores superan ampliamente los rangos típicos de voces fisiológicamente estables (=1 % para jitter y =3–5 % para shimmer), lo que indica una inestabilidad considerable tanto en la frecuencia como en la amplitud de las señales registradas.

Las diferencias observadas entre individuos y géneros pueden deberse a factores anatómicos (mayor frecuencia fundamental en mujeres, menor en hombres), pero también a condiciones experimentales como ruido de fondo, saturación o variaciones en la intensidad de fonación. En conjunto, el código permitió evidenciar que, aunque la metodología de detección de picos es efectiva para estimar estos parámetros, los valores obtenidos sugieren la necesidad de un mayor control en las condiciones de grabación para obtener medidas más representativas del comportamiento fisiológico de la voz.



### PARTE C – Comparación, análisis y conclusiones  
Los datos obtenidos de las grabaciones de voz permitieron estudiar y comparar diferentes características entre hombres y mujeres. Para ello no solo se tuvieron en cuenta parámetros generales como la frecuencia fundamental, la frecuencia media, el brillo espectral, la intensidad y la relación señal/ruido, sino también medidas más específicas como el jitter y el shimmer, que reflejan la estabilidad de la voz en cuanto a frecuencia y amplitud. Con todos estos resultados es posible observar cómo varían las voces en términos de tono, claridad y regularidad, y así realizar una comparación más completa entre las muestras analizadas. 


<img width="1039" height="316" alt="image" src="https://github.com/user-attachments/assets/352302af-1e59-4ce8-a2c7-9deb8d1d257f" />



La **frecuencia fundamental (F₀)** fue consistentemente más baja en las voces masculinas (96.8 Hz, 144.3 Hz y 381.8 Hz en promedio entre 100 y 200 Hz) que en las voces femeninas (207.1 Hz, 219.9 Hz y 381.8 Hz, en un rango entre 200 y 400 Hz). Esto concuerda con la teoría acústica de la voz, donde los hombres presentan cuerdas vocales más largas y con mayor masa, lo que genera vibraciones más lentas y por ende tonos más graves. En contraste, las mujeres presentan frecuencias fundamentales más altas, lo que produce un tono más agudo y distintivo.  

**Brillo espectral:** Las voces femeninas tendieron a mostrar valores de brillo más altos (0.5 y 0.6), lo que indica una mayor presencia de energía en frecuencias altas, otorgando un timbre más claro y agudo. En contraste, los hombres se concentraron en valores de 0.4, reflejando un carácter más opaco o grave.  

**Frecuencia media:** Las voces femeninas presentaron frecuencias medias notablemente más elevadas (5457.8 Hz y 4512.8 Hz en dos de los casos) en comparación con los hombres (entre 3600 Hz y 3800 Hz), lo que evidencia que gran parte de la energía vocal femenina se concentra en rangos más agudos.

**Intensidad (energía total):** No se observó un patrón uniforme. En algunos casos, los hombres tuvieron mayor energía (ej. Hombre 2: 3712.9 frente a Mujer 1: 3198.7), mientras que en otros, una voz femenina superó ampliamente en intensidad (ej. Mujer 2 con 6496.2, la más alta de todas). Esto sugiere que la intensidad depende más del esfuerzo fonatorio y de la calidad de grabación que del género de la voz, probablemente por las condiciones que habían al grabar todas las personas hablaron en un tono más alto o bajo con la intención de que se escuchará mejor.  

**Relación Señal/Ruido (SNR):** Hubo variabilidad en ambos géneros. Algunos hombres presentaron valores relativamente bajos (Hombre 3: 10.9 dB), mientras que una mujer obtuvo el mayor valor de SNR (Mujer 2: 13.28 dB), lo que refleja que este parámetro está más asociado a las condiciones de grabación y claridad de la señal que al timbre vocal en sí.  

Las voces masculinas presentan frecuencias fundamentales más bajas y menor brillo espectral, lo que les otorga un timbre más grave y opaco. Las voces femeninas muestran frecuencias fundamentales y medias más altas, con mayor energía en los agudos, lo que produce un timbre más brillante y claro. La intensidad y la relación señal/ruido no dependen directamente del género, sino de factores como el esfuerzo fonatorio, la proyección vocal y las condiciones de la grabación.

El análisis del shimmer (picos de amplitud detectados), tanto absoluto como relativo, nos permite identificar las variaciones en la amplitud de las ondas. Esto nos permite encontrar que la voz masculina suele presentar mayores cambios, probablemente porque hay una diferencia de presión en el aire que proviene de los pulmones, esto se resume en que la voz de los hombres tiende a sonar más fuertes, pero con cierta inestabilidad en el nivel de energía.

Los resultados obtenidos muestran que el jitter absoluto en la voz masculina es de 0.0019 s (≈23.86%), mientras que en la femenina es de 0.00095 s (≈19.13%). Esto muestra que las vibraciones de las cuerdas vocales femeninas son más regulares entre un ciclo y otro, lo que significa que la voz de las mujeres suele ser más estables y a su vez continuas, mientras que la voz de los hombres tiene irregularidades pequeñas que se puede reflejar como variaciones ligeras en el tono. 

En las gráficas que observamos anteriormente, los picos rojos representan los puntos de máxima vibración. En las voces femeninas, estos picos tienen intervalos más uniformes y constantes, mientras que en las masculinas los picos son más separados y con alturas variables. Estas diferencias gráficas confirman los valores obtenidos de jitter y shimmer, reforzando la idea de que la voz femenina es más uniforme y la masculina más variable en su intensidad y frecuencia.
Estas comparaciones pueden ser representadas gráficamente como: 

<img width="1600" height="1062" alt="image" src="https://github.com/user-attachments/assets/905c693a-5771-49f9-81f5-9cc5807e2feb" />


El jitter y el shimmer son parámetros acústicos que permiten cuantificar la estabilidad de la voz y detectar irregularidades en la vibración de las cuerdas vocales. El jitter mide las variaciones de frecuencia entre ciclos consecutivos, mientras que el shimmer mide las variaciones en la amplitud de esos mismos ciclos (Camargo et al., 2015). Estos indicadores son muy utilizados en el ámbito clínico porque ofrecen una forma objetiva de evaluar la calidad vocal y complementan la percepción subjetiva del especialista.

Cuando los valores de jitter y shimmer son altos, pueden indicar alteraciones en la función vocal o la presencia de lesiones estructurales en la laringe. Diversos estudios han encontrado que los pacientes con nódulos, pólipos, disfonías o enfermedades neurológicas presentan niveles significativamente mayores de jitter y shimmer en comparación con personas con voces normales (Cordeiro et al., 2021; Fang et al., 2021). Por esta razón, estos parámetros se utilizan como herramientas diagnósticas en la práctica clínica, permitiendo detectar disfunciones laríngeas y monitorear la evolución de los tratamientos de rehabilitación vocal.

Además, investigaciones recientes muestran que existe una relación entre el incremento del jitter y shimmer y una percepción auditiva de voz más áspera o con soplo, lo cual refuerza su utilidad para validar observaciones (Cordeiro et al., 2021). Por otro lado, también se ha reportado que los valores pueden verse afectados por factores externos como el entorno de grabación, el nivel de intensidad, el tipo de fonema o el ruido ambiental (Arias-Londoño et al., 2023). Por ello, se recomienda interpretar estas medidas junto con otros parámetros acústicos como la relación armónico-ruido.  

Todos estos hallazgos no solo confirman los principios fisiológicos y acústicos relacionados con la anatomía de las cuerdas vocales, sino que también evidencian el valor del análisis espectral y de estabilidad para evaluar objetivamente la calidad vocal. Asimismo, el estudio resalta la importancia de considerar factores externos como el entorno de grabación y el esfuerzo fonatorio, que pueden influir en parámetros como la intensidad y la relación señal/ruido. En conjunto, este tipo de análisis constituye una herramienta útil tanto para la comparación entre géneros como para la evaluación clínica de la función vocal y posibles alteraciones laríngeas.













### Referencias
- FreeConvert. (s.f.). Convertidor de MP4 a WAV en línea. Recuperado el 6 de octubre de 2025, de https://www.freeconvert.com/es/mp4-to-wav
- Moore, B. C. J. (2012). An Introduction to the Psychology of Hearing (6th ed.). Brill, de https://brill.com/display/title/21354
- Oppenheim, A. V., & Schafer, R. W. (2010). Discrete-Time Signal Processing (3rd ed.). Pearson Education. Recuperado de https://www.pearson.com/en-us/subject-catalog/p/discrete-time-signal-processing/P200000003760/9780131988422
- Peeters, G. (2004). A large set of audio features for sound description (similarity and classification) in the CUIDADO project. IRCAM de https://recherche.ircam.fr/equipes/analysesynthese/peeters/ARTICLES/Peeters_2003_cuidadoaudiofeatures.pdf
- Boersma, P., & Weenink, D. (2023). Praat: Doing phonetics by computer [Computer program]. Version 6.3. https://www.fon.hum.uva.nl/praat/
- Arias-Londoño, J. D., Gómez-García, J. A., & Godino-Llorente, J. I. (2023). Acoustic measures for voice pathologies: A review of reliability and clinical relevance. Applied Sciences, 13(2), 941. https://doi.org/10.3390/app13020941
- Camargo, Z., Cordeiro, A., & Behlau, M. (2015). Analysis of fundamental frequency, jitter, shimmer, and vocal intensity in women with and without voice disorders. Brazilian Journal of Otorhinolaryngology, 81(5), 526–532. https://doi.org/10.1016/j.bjorl.2015.07.011
- Cordeiro, A., Behlau, M., & Oliveira, G. (2021). Acoustic and perceptual analysis of voice in patients with benign laryngeal lesions. International Archives of Communication Disorder, 3(14). https://clinmedjournals.org/articles/iacod/international-archives-of-communication-disorder-iacod-3-014.php
- Fang, Q., Zhang, H., & Zheng, Y. (2021). Clinical application of acoustic parameters jitter and shimmer in the evaluation of vocal fold pathology. Annals of Palliative Medicine, 10(6), 6574–6582. https://doi.org/10.21037/apm-21-1048
