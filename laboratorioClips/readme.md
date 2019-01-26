# Clase magistral CLIPS
###### Máster Universitario en Análisis y Visualización de Datos Masivos/ Visual Analytics and Big Data
### Plantilla de hecho emergencia
* Tiene simbolos permitidos
* Por defecto el hecho mergencia se creará como inexistente
````JS 
(deftemplate emergencia
(slot problema
(allowed-symbols incendio inundacion cortoCircuito sobreCarga, inexistente)(default inexistente))
)
````

### Plantilla de hecho respuesta
* Tiene un unico campo y por defecto toma el valor de ninguna
````JS 
(deftemplate respuesta 
(slot accion (default ninguna)))
````
### Hechos iniciales 
````JS 
(deffacts situacion-iniciales
(situacion estable)
(emergencia)
(respuesta)
)
````
### luego se ejecuta un reset para visualizar los hechos 
```(reset)```

> En la seccion FACT f-1 f-2 f-3 estos indicadores unicos corresponden a hechos iniciales, para visualizarlo
de diferente forma se usa
``` (facts)``` 

## INGRESAR UNA REGLA	
>Se lo realiza con el comando defrule y de  nombre emergencia-incendio
atras de la flecha ( atras => delante)  se tiene el antecedente de la regla y delante el concecuente 

> **ANTECEDENTE** :: si se da un hecho emergencia con el campo problema = incendio y ademas se da una situacion estable
se activa los consecuentes de la rega 

>**CONCECUENTE** :: consiste crear un hecho con el campo accion (accion activa-extintor-incendio), otro hecho respuesta
respuesta (accion llamar-bomberos), actualizar la situacion a inestable (retract ?situacion)(assert(situacion inestable))

>**retract** :: es un comando que elimina hechos, y estos hechos son referenciados en el indice de hechos, es necesario realizar la operacion de GUARDAR en la variable interrelacion 
**SITUACION** el indice del hecho y esto se hace de esta manera, si yo indico en el antecedente de la regla **?situacion**, referencias el el hecho ( <- (situacion estable) ), la variable (situacion)
almacena el identificador de **(situacion estable)**, esta referencia del identificador es utilizado para el campo retract


````JS 
(defrule emergencia-incendio
		(	emergencia (problema incendio)	)?situacion <- (situacion estable)
=> 
        (   assert (   respuesta (accion activa-extintor-incendio)     )    )
        (   assert (   respuesta (accion llamar-bomberos)   )   )
        (retract ?situacion)
        (   assert(    situacion inestable    )   )
)
````

## crear otra regla que si hay otra situacion inestable se ha de evacuar el edificio

			