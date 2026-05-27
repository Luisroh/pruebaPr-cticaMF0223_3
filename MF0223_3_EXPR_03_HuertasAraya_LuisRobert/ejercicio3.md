# 1. Análisis de CPU

`htop` para realizar en tiempo real el análisis del consumo de CPU. En este caso encontramos tres procesos con un consumo inaudito (99-100%) de CPU. Por id son:  
- 3481
- 3482
- 3483  
Todos asociados al comando yes

# 2. Análisis de procesos

Usamos el comando `ps aux` para listar todos los procesos activos. Identificamos como sospechosos los que ya habíamos destacado en el subapartado anterior:
- luisroh   3481 99.8  0.0   3124  1536 pts/5    R    19:56   6:49 yes
- luisroh   3482 99.8  0.0   3124  1536 pts/5    R    19:56   6:49 yes
- luisroh   3483 99.8  0.0   3124  1536 pts/5    R    19:56   6:49 yes

# 3. Análisis de memoria

`free -h` es el indicado para observar el estado de uso de memoria. En mi caso obtenemos:  
**-------total used free shared buff available**  
**Mem:           7.4Gi       1.1Gi       4.8Gi        18Mi       1.7Gi       6.3Gi**  
**Swap:          2.0Gi          0B       2.0Gi**  
Por el tipo de script que se estña ejecutando, esto no tiene mucho sentido. Hay demasiada paz en el uso de la memoria. Aunque el script creó 751MB en archivos temporales, en WSL estos datos residen en el disco (no en RAM), por lo que no afectan significativamente a la memoria, dejándonos con una RAM estable y no creando el cuello de botella actual en el sistema.  

# 4 Análisis de disco

`df -h` nos muestra el uso de memoria. En mi caso obtenemos los siguientes datos a destacar:
- Espacio en '/' 4.1G de 1007G / 1% Lo cual es excelente
- Espacio en 'Espacio en /mnt/c' (fuera de WSL) 542G de 931G / 59%  Es aceptable pero no ideal. Muy recomendable vigilarlos de cerca
- Espacio en */tmp* **751MB**, es totalmente anómalo. La carpeta temporal no suele ocupar más de **100MB**

# 5 Diagnóstico

Sobra decir que la causa inicial del problema es la ejecución de un script malicioso, erróneo que nos ejecuta diversos procesos en paralelo sin control alguno, produciendo un consumo agresio de los recursos del sistema como hemos podido analizar en los apartados anteriores.

### CPU
- Tenemos 3 procesos *`yes`* que en total nos tienen el CPU en uhn **300%** de uso.
- Tenemmos 2 procesos *`cat /dev/zero`* que nos generan 0 de forma infinita
- Tenemos 2 procesos *`head -c 300M`* que leen **300MB** de datos en total

### DISCO (I/O)
- Tenemos el proceso *`dd`*  *30* veces, escribiendo archivos de 5MB
- Tenemos el mismo proceso *`cat /dev/zero`* que tiene I/O continuo

# 6 Solución

## Detener procesos problemáticos

- `pkill -f yes` para eliminar todos los bucles infinitos yes  
- `pkill -f dd` para matar todos los procesos dd  
- `pkill -f "cat /dev/zero"`  
- `pkill -f "head -c"` para matar los procesos 'cat' y 'head'  
- `pkill -f mem_test` para matar los procesos relacionados con 'mem_test'  
-  Finalmente comprobamos que no queden ninguno de estos procesos sospechosos:  
`ps aux | grep -E "yes|dd|cat|head|mem_test" | grep -v grep`

`pkill -f "yes|dd|mem_test|cat /dev/zero|head -c"` -> opcionalmente se pueden matar todos en un solo comando, por descontado.

## Liberar memoria

- Primero sincronizamos los archivos del disco:  
`sync`  
- Después liberamos la caché de memoria:  
`sudo sh -c 'echo 1 > /proc/sys/vm/drop_caches'`  
`sudo sh -c 'echo 2 > /proc/sys/vm/drop_caches'`  
`sudo sh -c 'echo 3 > /proc/sys/vm/drop_caches'`  
- Finalmente, comprobamos que la memoria haya sido efectivamente liberada:
`free -h`  
(Aunque en mi caso ya habíamos visto que el uso de memoria no estaba representando un problema como tal por lo ya explicado respecto a WSL)

## Eliminar archivos innecesarios

- Eliminamos los archivos de prueba de memoria:  
`rm -f /tmp/mem_test1 /tmp/mem_test2`  
- Eliminamos el directorio completo de test_disk:  
`rm -rf /tmp/test_disk`  
- Eliminamos cualquier otro archivo generado por el script:  
`rm -f /tmp/file_* 2>/dev/null`
- Verificamos cuánto se liberó:
`du -sh /tmp`
- Opcionalmente, podemos aprovechar y limpiar los archivos temporales viejos. Ya que estamos, dejamos todo bien limpito :):  
`find /tmp -type f -atime +1 -delete 2>/dev/null`

# 7 Verificación

Realmente ya se ha ido comprobando en cada momento, pero resumiendo todo en un mismo apartado sería:
- Listamoss los procesos sospechosos para verificar su inexistencia:  
  `ps aux | grep -E "yes|dd|mem_test|cat /dev/zero|head -c" | grep -v grep
`
- Comprobamos el estado general de la memoria:  
  `free -h`
- Verificamos el disco y los archivos temporales:  
  `df -h`  
  `du -sh /tmp 2>/dev/null`
- También que no existan más los archivos problemáticos:
  `ls -la /tmp/mem_test* 2>/dev/null`
  `ls -la /tmp/test_disk/ 2>/dev/null`
- Comprobamos que el estado de I/O tenga valores normales:  
  `iostat`

Así observamos que el sistema ha vuelto a la normalidad.  No quedan procesos problemáticos, el espacio /tmp ha sido liberado, la CPU vuelve a tener un valor idle > 80% (en mi caso 96.51%) y la carga del sistema es mínima.  
(idle es la columna con el porcentaje de procesador que está a la espera para ser utilizado/que está libre). Por tanto, la solución ha sido efectiva y correcta :D 