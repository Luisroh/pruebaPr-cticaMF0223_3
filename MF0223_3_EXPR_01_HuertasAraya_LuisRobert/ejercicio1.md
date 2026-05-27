# Errores que impiden el funcionamiento // malas prácticas 

## 1. El nombre de la red es incosistente

En el servicio *svr_dev_XX* encontramos que la conexión es referenciada de la siguiente manera:  
`networks:`  
  `  - net_devXX`  
Sin embargo, en la definición de la red encontramos que tiene '_':  
`networks:`  
  `  net_dev_XX:`    

Falla porque el servicio usa un nombre de red que no es con el cual ha sido definida la red.

## 2. Red indefinida    

`networks:`  
`net_dev_XX:`  

Si bien al dejarlo vacío Docker crea una red de puente funcional por defecto, es mejor explicitar la definición de red.

## 3. *container_name* fijo  
Hace que el servicio sea poco escalable. Es mejor que Docker genere el nombre  

## 4. Uso de XX como *placeholder*

En ficheros reales profesionales el fichero debería contener valores concretos.  

## 5. Volumen sin driver explícito  

Si bien no es un error per se, en entornos de producción es recomendable especificar driver y opciones.  

## 6. Falta del *restart*  
De nuevo, no es un error mortal que impide el arranque, pero sí que es recomendable la definición de esta policy estando el servicio en un entorno de producción.

## 7. No hay control de recursos  

No ha límites definidos de CPU/memoria   (si bien, me parece que en clase nunca hemos definido estos límites, es cierto que es una mala práctica el no definirlos, sobre todo en entornos compartidos)

# Correcciones hechas para su correcto despliegue / mejoras extra / corrección de malas prácticas

# 1. Nombre de red corregido

# 2. Añadida policy *restart*
`restart: unless-stopped` -> de tal forma que el contenedor se reinicia si falla o si Docker es reiniciado

# 3. *deploy.resources*
Limitación del uso tanto en CPU como en RAM

# 4. *driver* especificado

Empleo de `driver: bridge`, de tal forma que el *driver* de red está explícito y es más legible.

# 5. Volumen con nombre explícito

Así evitamos que se autogeneren nombres con prefijos

# 6. Imagen más precisa

En lugar de `ubuntu:22` especificamos a `ubuntu:22.04`  
  (soy muy tiquismiquis :( )

# 7. Empleo de una nomenclatura consistente 

Uso del patrón `devXX` en todos los nombres.

# 8. Eliminación de *`version: '3.8'`*

*`version`* está obsoleto. Es mejor su eliminación para evitar posibles errores.


# Resultado final

Ya tienes un docker-compose.yml completamente funcional y según norma/buenas prácticas, solo queda comprobación con `docker compose up -d` :D