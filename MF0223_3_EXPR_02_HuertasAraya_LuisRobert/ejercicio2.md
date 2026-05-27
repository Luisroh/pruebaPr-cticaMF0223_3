# 1.1

`mkdir dev_environment`    

Primero el comando para crear directorios, después el nombre del directorio o directorios a crear separados por un espacio

# 1.2 

`mkdir dev_environment/frontend/public dev_environment/frontend/src/components
 dev_environment/frontend/src/pages dev_environment/frontend/src/styles dev_environment/b
ackend/app dev_environment/backend/config dev_environment/backend/tests dev_environment/d
atabase/migrations dev_environment/docs dev_environment/scripts -p`  

Igual que en el punto anterior, *mkdir* es el comando para crear directorios. Después se pone la dirección completa puesto que estamos creando directorios desde afuera de la carpeta. Lo importante para poder crear todos estos directorios anidados sin que existanto *los padres* es el añadir `-p`. Así mismo, se pueden crear todos en un mismo comando separando mediante espacios en blanca cada nombre de directorio

# 2.(1-3)

`touch dev_environment/frontend/public/index.html dev_environment/frontend/src/App.js dev_environment/frontend/src/styles/main.css dev_environment/backend/app/server.js dev_environment/backend/config/config.json dev_environment/README.md dev_environment/scripts/deploy.sh`  

El comando `touch` es para crear archivos vacíos. Seguimos la misma lógica de creaciones de archivos anidados y de uso de un solo comando   (si bien no se demanda en el ejercicio que se haga como tal, tampoco se indnica que haya de ser en diferentes comandos así que mi vagancia opta por ahorrar y compactar todo en una instrucción :') )

# 3.1 

`sudo apt install nano`    

`sudo` en el caso de que no te encuentres en la raíz o no seas el admin. Acto seguido solicitará una contraseña y tras ello procederá a la instalación de nano. Si ya estás en root o tienes los permisos adecuados, basta con el comando `apt install nano`. En este caso, el uso de `sudo` no comporta un fallo, solo una redundancia 

# 3.2
- Para editar index.html:  
`nano dev_environment/frontend/public/index.html`  
- Para editar App.js:  
  `nano dev_environment/frontend/src/App.js`  
- Para editar main.css:  
  `nano dev_environment/frontend/src/styles/main.css`

# 4.1

Para mostrar el contenido del proyecto hay varias formas. La más básica:  
`ls dev_environment/*/*/*`  
Así podemos observar todas las carpetas, subcarpetas y archivos adentro.
La mmaaaaas básica sería simplemente:  
`ls dev_environment`, pero este comando solo mostrarías las subcarpetas directas de dev_environment.
También se podría mostrar con:  
`tree dev_environment/`. Esta es mi favorita. Todo queda más claro y bonito visualmente hablando.
dev_environment/  
├── README.md  
├── backend  
│   ├── app  
│   │   └── server.js  
│   ├── config  
│   │   └── config.json  
│   └── tests  
├── database  
│   └── migrations  
├── docs  
├── frontend  
│   ├── public  
│   │   └── index.html  
│   └── src  
│       ├── App.js  
│       ├── components  
│       ├── pages  
│       └── styles  
│           └── main.css  
└── scripts  
    └── deploy.sh  

# 4.2 

`ls dev_environment/ -lha` 
- ls comando para lista, dev_environment la carpeta a explayar. 
- -h para que sea "*human readable*"
- -a para carpetas ocultas
- -l para los permisos

`tree  dev_environment/ -pah` la versión de lo mismo pero para tree (me gusta más :))
- -p para mostrar los permisos
- -h para que sea "*human readable*"
- -a para ocultos (si bien, es recomendable el uso de ls para mejor visualización de los ocultos. Carpeta `.` o `..` a veces no se muestran con el uso de tree)

# 4.3

`ls dev_environment/frontend/src/` para el formato en listado

`tree dev_environment/frontend/src/` para el formato en árbol

# 5.1 

`mkdir dev_environment/backup`

# 5.2 

`cp dev_environment/frontend/ dev_environment/backup/ -r`  
El comado `cp` es el que copia carpetas y archivos. Con `-r` hacemos que sea recursivo y copie no solamente la carpeta frontend, sino toooodo su contenido. La primera dirección es el archivo a copiar, el segundo es la dirección en la cual ser copiado

# 5.3

`cp dev_environment/backend/app/server.js dev_environment/backup/server_backup.js`  
Aquí la diferencia en cuanto al archivo anterior radica en que no solo especificamos la carpeta de destino, sino que colocamos el nombre de archivo en el que queremos que se copie (de existir, copiaría en él, sino, lo crea (copia el origen))

# 6.1

`mv dev_environment/frontend/src/styles/main.css dev_environment/frontend/public/`  
`mv` es el comando que mueve archivos (o que copia archivos en una dirección borrándolos de su dirección original. Depenede cómo lo veas :)). La primera dirección es el archivo a mover, la segunda el destino del mismo

# 6.2

`mv dev_environment/frontend/src/App.js dev_environment/frontend/src/app.js`   si usamos podemos no solo mover archivos, sino modificar su nombre.

# 6.3 

`mv dev_environment/backend/config/config.json dev_environment/backend/app/`  
Nada nuevo, misma lógica de uso que en los dos subpuntos anteriores

# 7.1

`chmod 100 dev_environment/scripts/deploy.sh`  
`chmod` es el comando para configurar los permisos.ç
- El primer dígito es para el *owner*
- El segundo dígito es para el *group*
- El tercer dígito es para *others*  
Así mismo, el dígito es el resultado de la suma de:
- 4 para permisos de escritura
- 2 para permisos de lectura
- 1 para permisos de ejecución  

# 7.2 

`chmod 640 dev_environment/backend/app/server.js`  
Aplica exactamente el mismo razonamiento/lógica del subpunto anterior.

# 7.3 

`chmod 444 dev_environment/README.md`
Aplica exactamente el mismo razonamiento/lógica del subpunto anterior al anterios.

# 8.1

`rmdir dev_environment/frontend/src/components/`  
`rmdir` es el comando para eliminar directorios. Puesto que este mismo está vacío y no contiene archivos anidados, el comando así tal cual ya es funcional

# 8.2

`cp dev_environment/backup/frontend/src/components dev_environment/frontend/src/ -r`  
Copiamos de la carpeta origen *dev_environment/backup*. En este caso, es necesario el uso de `-r` ya que cp lo requiere al copiar carpetas (aunque estén vacías), precisamente porque diferencia entre carpetas y archivos, y para evitar copias de carpetas enteras por error

# 9.1

`rmdir temp/`  
si esto falla, hacer un chequeo con ls -la para encontrar cómo se llama. No siempre, si bien casi siempre, se llama así. Otros nombres a contemplar son:
- Temp 
- temp 
- tmp

# 9.2

`rm dev_environment/backup/server_backup.js`  

`rm` para eliminar archivos.

# 9.3 

`tree dev_environment/` como ya empleado antes, para visualizar la estructura completa del proyecto  (también cabe destacar que no viene instalado por defecto. En mi caso ya lo tenía porque lo uso, pero sino habría de emplearse `sudo apt install tree`)

dev_environment/  
├── README.md  
├── backend  
│   ├── app  
│   │   ├── config.json  
│   │   └── server.js  
│   ├── config  
│   └── tests  
├── backup  
│   └── frontend  
│       ├── public  
│       │   └── index.html  
│       └── src  
│           ├── App.js  
│           ├── components  
│           ├── pages  
│           └── styles  
│               └── main.css  
├── database  
│   └── migrations  
├── docs  
├── frontend  
│   ├── public  
│   │   ├── index.html  
│   │   └── main.css  
│   └── src  
│       ├── app.js  
│       ├── components  
│       ├── pages  
│       └── styles  
└── scripts  
    └── deploy.sh  

# 9.4

`pwd` comando para saber el directorio actual. (Print Working Directory). En mi caso `/home/luisroh`

# 9.5 

`history` muestra tooooodo el historial de comando utilizados