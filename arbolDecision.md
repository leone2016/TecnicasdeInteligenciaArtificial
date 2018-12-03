# Algoritmo árbol de decisión 
#### Pseudocódigo 
 ```Js
 
    function iniciar_arbol (Ejemplos E, lista_atributos, metodos_seleccion_atributos ){
        // comienza
        crear nodo N; // Paso 1
        if ( todos los elementos de E pertenecen a la misma clase C){ // Paso 2 :: si todos las clases son si o todas son no 
            // entronces 
            return N como nodo hoja etiquetado con la clase C {si ó no }
        }else if( lista_atributos.isEmpty() ){ // Paso 3
            //entonces 
            return N como nodo hoja etiquetado con la clase más numerosa en los ejemplos ::ej  existen 5 veces Si y 4 veces No, en este caso gana el SI por ser numerosa
        }else { //paso 4
               Atributo A = this.metodo_seleccion_atributos(E, lista_atributos); // para seleccionar el atributo A que mejor particione E :: Ambiente
               lista_atributos.remove(A); // borrar Atributo a de lista_atributos //paso 5 :: elimina Ambiente => {temperatura, humedad, viento}
               Etiquetar N en el atributo seleccionado //paso 6 :: N = Ambiente 
               For (para cada valor V de A) { //paso 7 :: en el caso de A = soleado  
                    siendo Ev el subconjunto de elemenos en E con valor V en el articulo A // paso 8 :: Ev = {E1,E2,E8,E9,E11}
                    
                     if (Ev.isEmpty()){ // paso 9 
                        //Entonces unir al nodo N  una hoja etiqueta con la clase mayoritaria en E // paso 10
                     }else{ 
                            unir al noo N el nodo retornado de this.incluir_arbol(Ev, lista_atributos, metodos_seleccion_atributos) // paso 11
                     }
               }
        }            
    }
 
 ```