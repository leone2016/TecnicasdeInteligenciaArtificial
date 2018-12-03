# Algoritmo árbol de decisión 
#### Pseudocódigo 
 ```Js
 
    function iniciar_arbol (Ejemplos E, lista_atributos, metodos_seleccion_atributos ){
        // comienza
        crear nodo N; // Paso 1
        if ( todos los elementos de E pertenecen a la misma clase C){ // Paso 2
            // entronces 
            return N; //como nodo hoja etiquetado con la clase C
        }else if( lista_atributos.isEmpty() ){ // Paso 3
            //entonces 
            return N; //como nodo hoja etiquetado con la clase más numerosa en los ejemplos
        }else { //paso 4
               Atributo A = this.metodo_seleccion_atributos(E, lista_atributos); // para seleccionar el atributo A que mejor particione E
               lista_atributos.remove(A); // borrar Atributo a de lista_atributos //paso 5
               Etiquetar N en el atributo seleccionado //paso 6
               For (para cada valor V de A) {
                    siendo Ev el subconjunto de elemenos en E con valor V en el articulo A
                     if (Ev.isEmpty()){
                        //Entonces unir al nodo N una hoja etiqueta con la clase mayoritaria en E
                     }else{
                        unir al noo N el nodo retornado de this.incluir_arbol(Ev, lista_atributos, metodos_seleccion_atributos)
                     }
               }
        }            
    }
 
 ```