Resolver la administración de 3 impresoras de una oficina. Las impresoras son usadas por N 
administrativos, los cuales están continuamente trabajando y cada tanto envían documentos 
a imprimir. Cada impresora, cuando está libre, toma un documento y lo imprime, de 
acuerdo con el orden de llegada.  
a) Implemente una solución para el problema descrito. 
b) Modifique la solución implementada para que considere la presencia de un director de 
oficina que también usa las impresas, el cual tiene prioridad sobre los administrativos. 
c) Modifique la solución (a) considerando que cada administrativo imprime 10 trabajos y 
que todos los procesos deben terminar su ejecución. 
d) Modifique la solución (b) considerando que tanto el director como cada administrativo 
imprimen 10 trabajos y que todos los procesos deben terminar su ejecución. 
e) Si la solución al ítem d) implica realizar Busy Waiting, modifíquela para evitarlo. 
Nota: ni los administrativos ni el director deben esperar a que se imprima el documento. }

PMS
Proccess impresoras[id:1..3]
    txt:text
    while true{
        admin!listo(id)
        admin?imprimir(txt)

    }

Process empleados[id:1..N]
    txt:text
    while true{
        generarDocu(txt)
        admin!mandarDocu(txt)
    }

Procces admin
    docus:cola of text
    do  empleados[*]?mandarDocu(txt)  -> docus.push(docus)
    [] (!empty(docus)) impresoras[*]?listo(id) -> impresoras[id]!imprimir(docus.pop())
    od


PMA
a)
Chan documentos(txt:text)

Proccess impresoras[id:1..3]
    txt:text
    while true{
       recive documentos(txt)
       //imprimir
    }

Process empleados[id:1..N]
    txt:text
    while true{
        generarDocu(txt)
        send documentos(txt)
    }
b)
Chan documentosPrioridad(txt:text)
Chan documentos(txt:text)

Proccess impresoras[id:1..3]
    txt:text
    while true{
    if(empty(documentosPrioridad))
        recive documentos(txt)
    else
        recive documentosPrioridad
    //imprimir
    }

Process empleados[id:1..N]
    txt:text
    while true{
        generarDocu(txt)
        if(!director)
            send documentos(txt)
        else
            send documentosPrioridad(txt)
    }

C y d y e?)
    Chan documentosPrioridad(txt:text)
    Chan documentos(txt:text)
    Chan total(cant:int)
    chan hayDocu()

Proccess impresoras[id:1..3]
    txt:text
    nro:int
    recibio = false;
    if(id = 1)
        send total(0)
    recive total(nro)
    while nro < 10*N{
    send total(nro++);
    recive hayDocu()
    if (empty(documentosPrioridad) && not empty(documentos)) -> recive documento(txt)
                                                                recibe = true
    [] (not empty(documentosPrioridad)) -> recive documento(txt)
                                            recibe = true
        end if
    //imprimir
    recive total(nro)
    }
    

Process empleados[id:1..N]
    txt:text
    for i in 1..10{
        generarDocu(txt)
        if(!director)
            send documentos(txt)
        else 
            send documentosPrioridad(txt)
        send hayDocu(txt)
    }