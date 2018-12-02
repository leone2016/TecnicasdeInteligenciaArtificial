# Algoritmo árbol de decisión 
#### Pseudocódigo 
 ```Js
 
    function iniciar_arbol (Ejemplos E, lista_atributos, metodos_seleccion_atributos ){
        // comienza
        crear nodo N; // Paso 1
        if ( todos los elementos de E pertenecen a la misma clase C){ // Paso 2
            # entronces 
            return N como nodo hoja etiquetado con la clase mas numerosa en los ejemplos
        }else{ # Paso 3
            Atributo A = this.metodo_seleccion_atributos(E, lista_atributos); // para seleccionar el atributo A que mejor particione E
            lista_atributos.remove(A); // borrar Atributo a de lista_atributos
        }            

        
    }
 
 ```