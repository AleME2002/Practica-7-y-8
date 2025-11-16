--Resumen--{                 // n es la longitud del arreglo a ordenar       // A es el arreglo                                                         Estabilidad     Mejor           Promedio        Peor
    POR COMPARACION
    Selection Sort: Recorre la lista desde i hasta n-1 buscando el minimo, para despues intercambiarlo en la posicion i.                                NO ESTABLE      O(n²)           O(n²)           O(n²)
    Insertion Sort: Toma A[ i] y lo va moviendo para atras hasta encontrar uno que sea menor que el en la lista.                                        ESTABLE         O(n)            O(n²)           O(n²)
    HeapSort: Arma un heap con los elementos de la lista, dsp saca el maximo y lo ordena en la lista.                                                   NO ESTABLE      O(n log n)      O(n log n)      O(n log n)
    MergeSort: Divide el arreglo en dos mitades, ordena recursivamente cada mitad, las mezcla (merge) en un arreglo ordenado.                           ESTABLE         O(n log n)      O(n log n)      O(n log n)
    QuickSort: Elige un pivote, particiona el arreglo en 2 partes, los mayores y los menores para despues ordenarlos recursivamente                     NO ESTABLE      O(n log n)      O(n log n)      O(n²)

    NO POR COMPARACION
    BucketSort: Separa los elementos en M categorias, despues crea una lista para cada categoria y las mete manteniendo el orden,                       ESTABLE         O(n + M) + O(ordenar buckets)  
                 para finalmente volver a meterlos en una lista.                                                                                                        O(n + M)       Sin ordenar

                A = [(rojo, 5), (azul, 2), (verde, 9), (rojo, 1), (azul, 4), (verde, 6), (rojo, 3)] Suponiendo que el orden es Azul Rojo Verde

                Azul = 0, Rojo = 1, Verde = 2
                res = bucketSort( A, M)     // en este caso M = 3

                res = [(azul, 2), (azul, 4), (rojo, 5), (rojo, 1), (rojo, 3), (verde, 9), (verde, 6)]

    CountingSort: Asume que los elementos son naturales y que estan acotados por un valor k, Crea un arreglo de conteo de tamaño k, cuenta cuántas      NO ESTABLE      O(n + k)
                   veces aparece cada valor, y luego reconstruye la lista ordenada repitiendo cada número la cantidad de veces que apareció.

                A = [1, 3, 1, 7, 2, 7, 1, 7, 3]

                res = countingSort( A, k)  // en este caso k es 8

                res = [1, 1, 1, 2, 3, 3, 7, 7, 7]

    RadixSort: Ordena digito por digito segun el codigo ASCII                                                                                           ESTABLE         O(n*log(max(A)))  Generalmente
               -                                                                                                                                                        O(n)              Para numeros chicos
                A = [329, 457, 657, 839, 436, 720, 355]

                res = radixSort(A)

                res = [329, 355, 436, 457, 657, 720, 839]

    Funciones:          // n = |A|    Todas tienen complejidad O(n)
        listToArray(A)      
        conjToArry(A)
        diccToArray(A)
        heapToArray(A)
}


--Ejercicio 1--{
    Las mejores opciones para ordenar es MergeSort, ya que es estable y tiene complejidad O(n log n), aunque si no importa la estabilidad, tmb podes usar HeapSort
}


--Ejercicio 2--{
    La estabilidad es cuando dos elementos tienen la misma clave de comparación, mantienen su orden relativo original después de ordenar.
}


--Ejercicio 6--{
    Ejemplo: A = [1, 3, 1, 7, 2, 7, 1, 7, 3] ------------- res = [1, 1, 1, 7, 7, 7, 3, 3, 2]

    proc ordenarPorCantidad(in A: array<int>): array<int>{
        var n = A.length                                                    //O(1)
        var maximo = A.maximo                                               //O(n) --> consigo el maximo de A
        var cantidades = diccionarioVacio< int, int>                        //O(1)
        for (i = 0, i < n, i++)                                             //O(n)
            if not cantidades.esta(A[i])                                    //O(log n)
                cantidades.definir(A[i], 1)                                 //O(log n)
            else                
                var cantidad = cantidades.obtener(A[i])                     //O(log n)
                cantidades.definir(A[i], cantidad +1)                       //O(log n)
            endif               
        endfor                                                              // Total del for O(n log n)
        var aux: new array< Tupla< int, int>> = diccToArray(cantidades)     //O(n)
        aux = mergeSort(aux) --> ordeno desc. por segunda posicion          //O(n log n)
        var res = array<n>                                                  //O(1)
        var j = 0                                                           //O(1)
        for (i = 0, i < aux.length, i++)                                    //O(n)      Puede pasar de que la lista tenga un solo elemento y que se repita n veces
            for (k = 0, aux[1])                                             //O(1)      o que tenga n elementos y que ninguno se repita, en todo caso la suma de  
                res[j] := aux[0]                                            //O(1)      la complejidad de estos for es O(n)
                j := j + 1                                                  //O(1)
            endfor
        endfor
        return res
    } // Complejidad: O(n log n)
}


--Ejercicio 7--{
    Orden
        -Decreciente de longitud
        -De la misma longitud, creciente segun el primer elemento de la escalera

    proc escaleras(in A: array<int>): array<int>{
        var n = A.length                                      
        var aux = ListaEnlazada< (int, int, int)>
        var inicio = A[0]                                     
        var fin = A[0]
        var long = 0                        
        for (i = 0, i < n - 1, i++)                                                 //O(n)
            if A[i] == A[i-1] + 1 then
                fin = A[i]                      
            else
                long := fin - inicio + 1
                aux.agregarAtras((inicio, fin, long))
                inicio := A[i]
                fin := A[i]
            endif                   
        endfor         
        long := fin - inicio + 1
        aux.agregarAtras((inicio, fin, long))             
        var listaDeEscaleras = ListaEnlazadaToArray(aux)                            //O(n)
        mergeSort(listaDeEscaleras)                                                 //O(n log n) --> ordeno de forma descendiente por long (3ra componente) y desempata de forma ascendente inicio (1er componente)
        var res = array<int>
        var i = 0
        for (j = 0, j < listaDeEscaleras.length, j++)                               //O(n)
            for (k = listaDeEscaleras[j][0], k < listaDeEscaleras[j][1] + 1, k++)
                res[i] = k
                i++
            endfor
        endfor
        return res
    }
}


--Ejercicio 10--{
    Orden
        -Primero Mañana y dsp Noche
        -Calificaciones
    
    Nombre ES string
    Turno ES enum{Mañana, Noche}   Acotado a 2
    Puntaje ES int  Acotado a 11
    
    proc ordenaPlantilla(in A: array<Nombre, Turno, Puntaje>): array<Nombre, Turno, Puntaje>{
        aux = bucketSort( A, 11)               // O(n) --> ordeno de forma descendente por puntaje (tercer componente) (0 = 0, 1 = 1, ..., 9 = 9, 10 = 10)
        res = bucketSort( aux, 2)              // O(n) --> ordeno de forma ascendente turno (segunda componente) (Mañana = 0, Noche = 1)
        return res
    }
}































































