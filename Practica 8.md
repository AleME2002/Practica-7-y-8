--Resumen--{                 // n es la longitud del arreglo a ordenar       // A es el arreglo                                                         Estabilidad     Mejor           Promedio        Peor
    POR COMPARACION
    Selection Sort: Recorre la lista desde i hasta n-1 buscando el minimo, para despues intercambiarlo en la posicion i.                                NO ESTABLE      O(n²)           O(n²)           O(n²)
    Insertion Sort: Toma A[ i] y lo va moviendo para atras hasta encontrar uno que sea menor que el en la lista.                                        ESTABLE         O(n)            O(n²)           O(n²)
    HeapSort: Arma un heap con los elementos de la lista, dsp saca el maximo y lo ordena en la lista.                                                   NO ESTABLE      O(n log n)      O(n log n)      O(n log n)
    MergeSort: Divide el arreglo en dos mitades, ordena recursivamente cada mitad, las mezcla (merge) en un arreglo ordenado.                           ESTABLE         O(n log n)      O(n log n)      O(n log n)
    QuickSort: Elige un pivote, particiona el arreglo en 2 partes, los mayores y los menores para despues ordenarlos recursivamente                     NO ESTABLE      O(n log n)      O(n log n)      O(n²)
    Merge: Combina 2 listas previamente ordenadas       // merge( A, B) con |A|= n  y |B|= m                                                            ESTABLE         O(n + m)        O(n + m)        O(n + m)

    NO POR COMPARACION
    BucketSort: Separa los elementos en M categorias, despues crea una lista para cada categoria y las mete manteniendo el orden,                       ESTABLE         O(n + M) + O(ordenar buckets)  
                 para finalmente volver a meterlos en una lista.                                                                                                        O(n + M)       Sin ordenar

                A = [(rojo, 5), (azul, 2), (verde, 9), (rojo, 1), (azul, 4), (verde, 6), (rojo, 3)] Suponiendo que el orden es Azul Rojo Verde

                Azul = 0, Rojo = 1, Verde = 2
                res = bucketSort( A, M)     // en este caso M = 3

                res = [(azul, 2), (azul, 4), (rojo, 5), (rojo, 1), (rojo, 3), (verde, 9), (verde, 6)]   
                
                Tambien se puede pedir orden adicional

                res = bucketSort( A, M) --> ordeno primero por categoria y desempata ascen. 2da componente                                                              O(n log n)

                res = [(azul, 2), (azul, 4), (rojo, 1), (rojo, 3), (rojo, 5), (verde, 6), (verde, 9)]

    CountingSort: Asume que los elementos son naturales y que estan acotados por un valor k, Crea un arreglo de conteo de tamaño k, cuenta cuántas      NO ESTABLE      O(n + k)
                   veces aparece cada valor, y luego reconstruye la lista ordenada repitiendo cada número la cantidad de veces que apareció.

                A = [1, 3, 1, 7, 2, 7, 1, 7, 3]

                res = countingSort( A, k)  // en este caso k es 8

                res = [1, 1, 1, 2, 3, 3, 7, 7, 7]

    RadixSort: Ordena digito por digito segun el codigo ASCII            // LLamo l a la cantidad de dijitos                                            ESTABLE         O(n log(max(A)))  Generalmente
               -                                                                                                                                                        O(nl)             para palabras acotadas
                A = [329, 457, 657, 839, 436, 720, 355]                                                                                                                 O(n)              Para numeros chicos

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


--Ejercicio 12{
    proc sensorIndustrial(in A: array<int>): array<int>{
        int n = A.lentgh                                //O(1)
        ListaEnlazada enRango = secuenciaVacia          //O(1)
        ListaEnlazada fueraDeRango = secuenciaVacia     //O(1)
        for (i = 0, i < n, i++)                         //O(n)
            if (A[i] >= 20) && (A[i] <= 40)             //O(1)
                enRango.agregarAdelante(A[i])           //O(1)
            else
                fueraDeRango.agregarAdelante(A[i])      //O(1)
            endif
        endfor
        listaToArray(enRango)                           //O(n)
        listaToArray(fueraDeRango)                      //O(n)
        bucketSort(enRango)                             //O(n) (Hay 20 categorias por ende M se puede tomar como O(1))
        selectionSort(fueraDeRango)                     //O(√n²) == //O(n)
        res = merge(enRango, fueraDeRango)              //O(1)
        return res                                      //O(1)
    }
}


--Ejercicio 13--{
    Orden
        -Segunda componente
        -Primer componente

    string[l] ES string de long l (comparar 2 es O(l))
    T ES < int, string[l]> 

    proc ordenarT(in A: array<T>): array<T>{
        mergeSort(A)        --> Ordeno ascen. por 1er componente    //O(n log n)
        radixSort(A)        --> Ordeno por 2da componente           //O(nl) 
    }

    b.

    proc ordenarT(in A: array<T>): array<T>{
        bucketSort(A)       --> Ordeno ascen. por 1er componente    //O(n + n) = O(2n) = O(n)
        radixSort(A)        --> Ordeno por 2da componente           //O(nl) 
    }
}


--Ejercicio 14{
    proc ordenarMultiplos(in A: array<int>, in k: int): array<int>{
        int n = A.length                                        //O(1)
        mergeSort(A)                                            //O(n log n)
        ListaEnlazada listasDeMultiplos = secuenciaVacia        //O(1)
        for (i = 0, i < n, i++)                                 //O(n)
            ListaEnlazada aux = secuenciaVacia                  //O(1)
            for (j = 1, j < k + 1, j++)                         //O(k)
                aux.agregarAdelante(A[i]*j)                     //O(1)
            endfor
            listToArray(aux)                                    //O(k)
            listasDeMultiplos.agregarAtras(aux)                 //O(1)
        endfor // Complejidad: O(nk)
        res = merge(listasDeMultiplos)                          O(nk log nk) 
    }
}


--Ejercicio 17--{
    Datos:
        -Hierbas maximo 100 caracteres (acotado)
        -Stock es una secuencias de tuplas < planta, cantidad> 
            -Se pueden repetir las plantas
        -Usos es un diccionario {planta: usos}
    
    Orden:
        -Recolectar devuelve una lista de plantas
        -Primero las q se usan en mas recetas
        -Desempata cantidad en almacen

    Planta ES string
    
    proc recolectar(in s: Vector< Planta, int>, in u: Diccionario< planta: int>): Vector< Planta>{  // O(n + h log h) con h = # Plantas
        int n = s.length                                     //O(1)
        DiccionarioDigital stockTotal = diccionarioVacio     //O(1)
        for (i = 0, i < n, i++)                              //O(n)
            if stockTotal(s[i][0]).esta                      //O(1)
                int aux = stockTotal.obtener(s[i][0])        //O(1) (ya que las hierbas son acotadas a 100)
                stockTotal.definir(s[i][0], s[i][1] + aux)   //O(1)
            else
                stockTotal.definirRapido(s[i][0], s[i][1])   //O(1)
            endif
        endfor
        int h = stockTotal.length
        diccToArray(stockTotal) //array<(Planta, int)>       //O(h)
        Vector aux = vectorVacio
        for each elem in u                                   //O(h)
            lo meto en aux con tercera componente la cantidad en stockTotal
        endfor
        mergeSort(aux) --> ordena de forma desc. segun 2da componente y desempata de forma desc. la tercer componente
        se termina facil
    }
}

--Preguntas--{
    -Se puede usar "for each elem in"? lo vi en un parcial q estaba bien.
    -Se puede usar "diccToArray" o "conjToArray" o etc.? y q pasa si es por ejemplo un diccTrieToArray o un diccLogToArray, tmb se puede usar?
}


























































