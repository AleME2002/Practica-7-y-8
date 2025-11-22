--Resumen--{
    -Cuando un programa S es correcto respecto a la especificacion (P,Q) se dice:  {P} S {Q}

    -WP(S,Q) es la p mas debil tal que {P} S {Q}

        Ej: Requiere: {P}
            x = x + 2
            Asegura: {x > 10}

            En este caso la WP seria x > 8,  Se cumple la tripla de Hoare: {x>8} x=x+2 {x>10}

    -Sintaxis:
        -Asignacion: x = E
        -Nada: Skip
        -Secuencia: S1;S2
        -Condicional: if B then S1 else S2 endif
    
    -Axiomas:
        1. WP(x = E, Q) = def(E) yl 
}