# Laboratorio: Inferencia con CLIPS 
### Descripción del laboratorio

> Mediante este laboratorio se pretende que tengas un primer contacto con la herramienta para la generación de sistemas expertos CLIPS y su lenguaje, así como que observes y comprendas cómo un sistema experto infiere nuevos hechos a partir de la información en su base de conocimiento y de hechos existentes.
Antes de acudir al laboratorio debes repasar el tema 9, sin olvidar visualizar la lección magistral de dicho tema. Ya que se utilizará la herramienta CLIPS, antes de la sesión de laboratorio debes descargar e instalar CLIPS en tu ordenador. Puedes instalar CLIPS 6.3 que es la versión que se presenta en la lección magistral o CLIPS 6.4 una versión 0más moderna. Podrás acceder al sitio web de CLIPS para descargártelo [CLIPSRULES](http://www.clipsrules.net/), así como encontrar información de esta herramienta a través del apartado “+Información” del tema 9.

### Elaboración del laboratorio
> Durante este laboratorio utilizarás la herramienta CLIPS. A través de ella introducirás hechos en la base de hechos (lista de hechos), reglas en la base de conocimiento. Además, observarás cómo nuevos hechos se infieren cuando se ejecutan las reglas y cómo el motor de inferencia de CLIPS resuelve conflictos cuando más de una regla es activada, de acuerdo a diferentes estrategias de resolución de conflictos. El profesor te indicará los pasos concretos que deberás llevar a cabo.

Los pasos que debes llevar a cabo en este laboratorio son los siguientes:

1.-	Ejecuta el programa CLIPS. En el interfaz gráfico vas a ver la consola de CLIPS donde puedes introducir los diferentes comandos. Se recomienda habilitar también las visualizaciones de Facts y Agenda para poder ver los hechos activos y las reglas activadas. Para CLIPS 6.3 la lista de hechos y la agenda se pueden visualizar mediante las 0pciones del menú Debug -> 1 Facts Browser y Window -> 2 Agenda (MAIN), respectivamente. Para CLIPS 6.4 la lista de hechos y la agenda se pueden visualizar mediante las 0pciones del menú Debug -> Facts Browser y Debug -> Agenda Browser, respectivamente. Configura además las opciones de observación para visualizar en la consola la información sobre compilación, hechos, reglas y activaciones. En CLIPS 6.3 se configuran las opciones de visualización a través del menú Execution  -> Watch tal como se muestra en la Figura 1. En CLIPS 6.4 para configurr las opciones de visualización debes ir al menú Debug -> Watch  y seleccionar Activations, Compilations, Facts y Rules.

2.-	Utilizando el comando (deffacts) añade los siguientes hechos iniciales que se conocen acerca de las manzanas: 
Para que estos hechos se activen, hay que introducir el comando (reset) tras la ejecución del comando (facts).

````JS 
CLIPS> (deffacts apples
            (color roja) 
            (color verde) 
            (color amarilla) 
            (tamano grande) 
            (tamano pequena)
        )
CLIPS> (reset)
```` 

3.- Utilizando el comando (defrule) introduce una única regla que genere hechos de la forma (manzana roja grande) para todas las combinaciones posibles de color y tamaño. A partir de los hechos iniciales previamente creados, cuando esta regla se active y se ejecute, debe dar lugar a los hechos (manzana roja grande), (manzana roja pequena), (manzana verde grande), (manzana verde pequena), etc. No se considerará como válida una regla en la que se establezcan de forma manual todas las posibles combinaciones de colores y tamaños. Éstas deben generarse de forma automática y entonces la regla proporcionada deberá funcionar para cualquier posible definición de los hechos iniciales. 


````JS 
CLIPS> (defrule concate
		(  color ?x )
		(  tamano ?y )
=> 
        ( assert( manzanas ?x ?y )  )
     
)
````
> salida paso 3
````CONSOLE
CLIPS> (run)
FIRE    1 concate: f-1,f-5
==> f-6     (manzanas roja pequena)
FIRE    2 concate: f-2,f-5
==> f-7     (manzanas verde pequena)
FIRE    3 concate: f-3,f-5
==> f-8     (manzanas amarilla pequena)
FIRE    4 concate: f-1,f-4
==> f-9     (manzanas roja grande)
FIRE    5 concate: f-2,f-4
==> f-10    (manzanas verde grande)
FIRE    6 concate: f-3,f-4
==> f-11    (manzanas amarilla grande)
````

4.-	Es conocido que las manzanas pequeñas sólo pueden ser verdes. Por este motivo, escribe otra regla que elimine de la memoria de trabajo las manzanas pequeñas que no sean de color verde. No será válida una regla que elimine sólo las manzanas pequeñas rojas o las manzanas pequeñas amarillas porque entonces no funcionaría para cualquier otro color de manzana definido en los hechos iniciales. Se espera que la regla (sólo una) elimine todas las manzanas pequeñas que sean de un color que no sea verde.


````JS 
(defrule small-apples
?f <- (manzanas ~verde pequena)
=>
(retract ?f)
)

(run)
````

> salida paso 4

````console
==> Activation 0      small-apples: f-6
==> Activation 0      small-apples: f-8
CLIPS> (run)
FIRE    1 small-apples: f-8
<== f-8     (manzanas amarilla pequena)
FIRE    2 small-apples: f-6
<== f-6     (manzanas roja pequena)
````

5.-	Añade una regla que afirme que si hay una manzana roja grande, hay una manzana envenenada (manzana envenenada).

 