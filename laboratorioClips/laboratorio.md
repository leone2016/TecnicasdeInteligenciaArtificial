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
CLIPS> (defrule small-apples
            ?f <- (manzanas ~verde pequena)
        =>
            (retract ?f)
        )

CLIPS> (run)
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

 
````JS 
CLIPS> (defrule big-red-apple
            ?f <- (manzanas roja grande)
        =>
            (  assert ( manzana envenenada ) )
        )

CLIPS> (run)
````
 > salida paso 5
 
 ````console
 ==> Activation 0      big-red-apple: f-9
 CLIPS> (run)
 FIRE    1 big-red-apple: f-9
 ==> f-12    (manzana envenenada)
 ````
 
6.-	Añade otra regla que afirme que si existe una manzana envenenada Blancanieves muere (blancanieves muerta).

 ````JS 
 CLIPS> (defrule poison_apple
             ?f <- (manzana envenenada)
         =>
             (  assert ( blancanieves muerta ) )
         )
 
 CLIPS> (run)
 ````
 > salida paso 6
 
 ````console
==> Activation 0      poison_apple: f-12
CLIPS> (run)
FIRE    1 poison_apple: f-12
==> f-13    (blancanieves muerta)
````

7.-	Establece en este paso la estrategia de resolución de conflictos en profundidad mediante el comando: (set-strategy depth). Por defecto, la estrategia de resolución de conflictos en CLIPS es en profundidad (depth). Sin embargo, ejecuta el comando mencionado anteriormente para asegurarte de que la configuración de la ejecución es correcta. En CLIPS 6.3, y en versiones anteriores, existe también la posibilidad de configurar las opciones de ejecución a través del menú Execution -> Options tal como se muestra en la Figura 2. Aunque se ha observado que algunas distribuciones de CLIPS, en concreto para MAC, no configuran bien la estrategia de resolución de conflictos mediante el cuadro de diálogo mostrado en la Figura 2. Por este motivo, recomendamos modificar la estrategia de resolución de conflictos mediante línea de comandos para asegurarse de que la estrategia configurada es la de profundidad. 

 
 <p align="center">
   <img width="460" height="300" src="https://raw.githubusercontent.com/leone2016/TecnicasdeInteligenciaArtificial/master/laboratorioClips/resolicionConflictos.png">
 </p>

 
 > salida paso 7
  
 ````console
 CLIPS> (set-strategy depth)
 depth
 ````
 
 8.- Ejecuta el comando (reset) para borrar las reglas activadas de la agenda, manteniendo su definición, y eliminar los hechos que se hayan podido generar (si estás realizando pruebas) volviendo a la lista de hechos inicial determinada por el comando (deffacts) ejecutado en el paso 2.
 
 > salida paso 8
 
 ````console
 CLIPS> (reset)
 <== f-1     (color roja)
 <== f-2     (color verde)
 <== f-3     (color amarilla)
 <== f-4     (tamano grande)
 <== f-5     (tamano pequena)
 <== f-7     (manzanas verde pequena)
 <== f-9     (manzanas roja grande)
 <== f-10    (manzanas verde grande)
 <== f-11    (manzanas amarilla grande)
 <== f-12    (manzana envenenada)
 <== f-13    (blancanieves muerta)
 ==> f-1     (color roja)
 ==> f-2     (color verde)
 ==> f-3     (color amarilla)
 ==> f-4     (tamano grande)
 ==> Activation 0      concate: f-3,f-4
 ==> Activation 0      concate: f-2,f-4
 ==> Activation 0      concate: f-1,f-4
 ==> f-5     (tamano pequena)
 ==> Activation 0      concate: f-3,f-5
 ==> Activation 0      concate: f-2,f-5
 ==> Activation 0      concate: f-1,f-5
 CLIPS> 
 
 ```` 
 
 
 9.- Ejecuta las reglas utilizando el comando (run). Recuerda que la resolución de conflictos se va a realizar mediante la estrategia depth (en profundidad). En la consola puedes observar el orden en que se ejecutan (o disparan) las reglas [FIRE], los hechos que generan [==> f-] o eliminan [<== f-] las reglas disparadas, y las nuevas reglas que son aplicables (que se activan [Activation]) al haberse generado nuevos hechos. Copia en el informe la salida que genera CLIPS (salida mostrada en la ventana de dialogo -Dialog Window- en CLIPS 6.3 o en la consola en CLIPS 6.4) al ejecutar las reglas con el comando (run). De forma alternativa, en vez de utilizar el comendo (run), también puedes utilizar el comando (run 1) que ejecuta sólo una regla. Ejecutando varias veces el comando (run 1) hasta que no queden reglas activas en la Agenda podrás observar cómo se modifica la lista de hechos (Facts) y la agenda (Agenda) con la ejecución de cada una de las reglas. 
 
 > salida paso 9
 
 ````console
 CLIPS> (run)
 FIRE    1 concate: f-1,f-5
 ==> f-6     (manzanas roja pequena)
 ==> Activation 0      small-apples: f-6
 FIRE    2 small-apples: f-6
 <== f-6     (manzanas roja pequena)
 FIRE    3 concate: f-2,f-5
 ==> f-7     (manzanas verde pequena)
 FIRE    4 concate: f-3,f-5
 ==> f-8     (manzanas amarilla pequena)
 ==> Activation 0      small-apples: f-8
 FIRE    5 small-apples: f-8
 <== f-8     (manzanas amarilla pequena)
 FIRE    6 concate: f-1,f-4
 ==> f-9     (manzanas roja grande)
 ==> Activation 0      big-red-apple: f-9
 FIRE    7 big-red-apple: f-9
 ==> f-10    (manzana envenenada)
 ==> Activation 0      poison_apple: f-10
 FIRE    8 poison_apple: f-10
 ==> f-11    (blancanieves muerta)
 FIRE    9 concate: f-2,f-4
 ==> f-12    (manzanas verde grande)
 FIRE   10 concate: f-3,f-4
 ==> f-13    (manzanas amarilla grande)
 ````
 
 
10.- Cambia la estrategia de resolución de conflictos de reglas a anchura (breadth), ejecutando el siguiente comando: (set-strategy breadth). En CLIPS 6.3 podrías hacerlo a través del menú Execution->Options, pero por el motivo expuesto en el paso 7 es preferible utilizar la línea de comandos. 

> salida paso 10

````console
CLIPS> (set-strategy breadth)
depth
````

11.	Ejecuta el comando (reset) para borrar las reglas activadas de la agenda y eliminar los hechos volviendo a la lista de hechos inicial determinada por el comando (deffacts) ejecutado en el paso 2. 

>salida paso 11

````console
CLIPS> (reset)
<== f-1     (color roja)
<== f-2     (color verde)
<== f-3     (color amarilla)
<== f-4     (tamano grande)
<== f-5     (tamano pequena)
<== f-7     (manzanas verde pequena)
<== f-9     (manzanas roja grande)
<== f-10    (manzana envenenada)
<== f-11    (blancanieves muerta)
<== f-12    (manzanas verde grande)
<== f-13    (manzanas amarilla grande)
==> f-1     (color roja)
==> f-2     (color verde)
==> f-3     (color amarilla)
==> f-4     (tamano grande)
==> Activation 0      concate: f-3,f-4
==> Activation 0      concate: f-2,f-4
==> Activation 0      concate: f-1,f-4
==> f-5     (tamano pequena)
==> Activation 0      concate: f-3,f-5
==> Activation 0      concate: f-2,f-5
==> Activation 0      concate: f-1,f-5
````

12.- Ejecuta las reglas utilizando el comando (run). Recuerda que la resolución de conflictos se va a realizar mediante la estrategia breadth (en anchura). Al igual que en el paso 9 puedes observar el orden en que se ejecutan las reglas. Copia en el informe la salida que genera CLIPS (salida mostrada en la ventana de dialogo -Dialog Window- en CLIPS 6.3 o en la consola en CLIPS 6.4) al ejecutar las reglas con el comando (run). 

>salida paso 12

````console
CLIPS> (run)
FIRE    1 concate: f-3,f-4
==> f-6     (manzanas amarilla grande)
FIRE    2 concate: f-2,f-4
==> f-7     (manzanas verde grande)
FIRE    3 concate: f-1,f-4
==> f-8     (manzanas roja grande)
==> Activation 0      big-red-apple: f-8
FIRE    4 concate: f-3,f-5
==> f-9     (manzanas amarilla pequena)
==> Activation 0      small-apples: f-9
FIRE    5 concate: f-2,f-5
==> f-10    (manzanas verde pequena)
FIRE    6 concate: f-1,f-5
==> f-11    (manzanas roja pequena)
==> Activation 0      small-apples: f-11
FIRE    7 big-red-apple: f-8
==> f-12    (manzana envenenada)
==> Activation 0      poison_apple: f-12
FIRE    8 small-apples: f-9
<== f-9     (manzanas amarilla pequena)
FIRE    9 small-apples: f-11
<== f-11    (manzanas roja pequena)
FIRE   10 poison_apple: f-12
==> f-13    (blancanieves muerta)
CLIPS> 
````
 