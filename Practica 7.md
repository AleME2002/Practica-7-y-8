--Ejercicio 7--{

    Modulo SistemaImpl implementa Sistema{
        var notas: DiccionarioLog< Materia, DiccionarioLog< Alumno, Nota>>
    }

    proc nuevoSistema(){
        res.notas = DiccionarioLogVacio()               // O(1)     // Cambio el DiccionarioLogVacio a DiccionarioVacio?
        var res: SistemaImpl = new SistemaImpl          // O(1)
    }

    proc registrarMateria(inout s: Sistema, in m: Materia){ 
        s.notas.definir(m, DiccionarioLogVacio())          // O(log m)
    }

    proc registrarNota(inout s: Sistema, in m: Materia, in a: Alumno, in n: Nota){
        var m = s.notas.obtener(m)          // O(log m)
        m.definir(a, n)                     // O(log n)
    }

    proc notaDeAlumno(in s: Sistema, in m: Materia, in a: Alumno): int{
        var mat: DiccionarioLog< Alumno, Nota> = s.notas.obtener(m)         // O(log m)
        var res: int = mat.obtener(a)                                       // O(log n)
        return res
    }
}



--Ejercicio 9--{

    Modulo IngresosAlBncoImpl implementa IngresosAlBanco{
        var totales: Vector< int>
        var totalHasta: Vector< int>          // Suma los totales hasta ese dia
        var maximo
    }

    1. Podes conseguir la cantidad de presonas haciendo totalesHata[ hasta] - totalHasta[ desde - 1]. (habria q separa en casos si desde = 0)
    2. Crece con el tamaño de dias que pasaron, es decir ocupa O(n)
}



--Ejercicio 10--{
    
    -Cliente es un codigo alfanumerico de 10 digitos (acotado)
    
    Modulo MadereraImpl implementa Maderera{
        var listones: ColaDePrioridadMax<Liston>
        var clientes: DiccionarioTrie<Cliente, ConjLineal< Tupla< Fecha, Liston>>>
    }
    
    proc comprarUnListon(m: Maderero, l: Liston){
        m.listones.encolar(l)           // O(log m)
    }
    
    proc venderUnListon(inout m: Maderera, in tamaño: int, in c: Cliente, in f: Fecha){
        int liston = m.listones.desencolar                                              // O(log m)
        int sobrante = liston - tamaño                                                  // O(1)
        if sobrante > 0                                                                 // O(1)
            m.listones.encolar(sobrante)                                                // O(log m)
        endif
        Tupla< Fecha, Liston> = nuevaVenta Tupla<f, tamaño>                             // O(1)
        if not m.clientes.esta(c)                                                       // O(1)
            m.clientes.definir(c, new ConjLineal< Tupla< Fecha, Liston>>)               // O(1)
        endif
        ConjLineal< Tupla< Fecha, Liston>> ventasCliente = m.clientes.obtener(c)        // O(1)
        ventasCliente.agregarRapido(nuevaVenta)                                         // O(1)
    }

    function VentasACliente(inout M: Maderera, in c: Cliente)Conjunto<Tupla<Fecha, Liston>>{
        res := M.clientes.obtener(c)        //O(1)
    }
}



--Ejercicio 11--{

    -Una sola estanteria de capasidad arbitraria, cada libro ocupa una posicion
    -Registro de socios (Nombres acotados)
    -Cuando hay libro nuevo o uno devuelto, se pone en el primer espacio libre de la estanteria

    L es la cantidad de libros de la biblioteca
    R es la cantidad de libros que un socio tiene registrados
    K es la contidad de posiciones libres en la estanteria

    Modulo BibliotecaImpl implementa Biblioteca{
        var estanteria: DiccLog< Libro, Posicion>
        var socios: DiccLog< Socio, ConjLog< Libro>>
        var libres: ConjLog< Posicion>
    }

    proc agregarLibro(inout b: Biblioteca, in l: idLibro){ O(log K + log L)
        Nodo menor = b.libres.raiz                      // O(1)
        while (menor.izq != null)                       // O(log L)
            menor = menor.izq                           // O(1)
        endwhile 
        b.estanteria.agregar( l, menor.valor)           // O(log L)
        b.libres.sacar(menor)                           // O(log K)
    }

    proc pedirLibro(inout b: Biblioteca, in l: idLibro, in s: Socio){ O(log R + log K + log L)

    }

    proc devolverLibro(inout b: Biblioteca, in l: idLibro, in s: Socio){ O(log R + log K + log L)

    }

    proc prestados(inout b: Biblioteca, in s: Socio){ O(1) 

    }

    proc ubicacionDeLibro(inout b: Biblioteca, in l: idLibro){ O(log L)

    }

    Preguntas:
        -Como consigo el menor(por ejemplo) de un avl, tengo q hacer la funcion o solo con poner ejemploAVL.minimo ya basta

}



--Ejercicio 12--{

    -Actividad tiene un identificador, un horario de inicio y otro de final
    -No puede haber dos actividades con el mismo identificador
    -Las actividades solo se pueden iniciar y terminar en punto, y terminan en horario posterior al inicio. Tampoco pueden iniciar y terminar en dias distintos
    -Si una actividad termina a una cierta hr, no cuenta como que la hiciste en esa hr
    -se pueden agregar tags para agrupar las actividades

    d es la cantidad de dias registrados hasta el momento
    a es la cantidad total de actividades

    Modulo AgendaImpl implementa Agenda{
        var actividades: DiccLog< Actividad, ConjLineal< Tupla< Dia, Inicio, Fin>>>
        var dias: DiccLog< Dia, ConjLog< Actividad>>
        var tags: DiccTrie< Tag, ConjLineal< Actividad>>
    }

    proc registrarActividad(inout ag: Agenda, in act: IdActividad, in dia :Dia, in inicio :Hora, in fin :Hora){ O(log a + log d)
        Tupla< Dia, Inicio, Fin> tiempoDeActividad = new Tupla< dia, inicio, fin>           // O(1)
        ag.actividades.definir(act, tiempoDeActividad)                                      // O(log a)
        if not ag.dias.esta(dia)                                                            // O(log d)
            ag.dias.definir( dia, new ConjLogvacio)                                         // O(log d)
        ConjLineal< Tupla< Dia, Inicio, Fin>> elDia = ag.dias.obtener(dia)                  // O(log d)
        elDia.agregarRapido(tiempoDeActividad)                                              // O(1)
    }

    proc verActividad(in ag: Agenda, in act: IdActividad): struct < dia: Dia, inicio: Hora, fin: Hora>{ O(log a)
        
    }

    proc agregarTag(inout ag: Agenda, in act: IdActividad, in t: Tag){ O(1)

    }

    proc horaMasOcupada(in ag: Agenda, in d: Dia) : Hora{ O(log d)

    }

    proc actividadesPorTag(in : Agenda, in : Tag): Conjunto< IdActividad>{ O(1)

    }
}






























































