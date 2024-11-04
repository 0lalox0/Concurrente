En una oficina existen 100 empleados que envían documentos para imprimir en 5 impresoras 
compartidas. Los pedidos de impresión son procesados por orden de llegada y se asignan a la primera 
impresora que se encuentre libre: 
a) Implemente un programa que permita resolver el problema anterior usando PMA
b) Resuelva el mismo problema anterior pero ahora usando PM

a)
Chan esperando(int,TEXT)
Chan imprimir[id:1..5](Text)
Chan liberarImpresora(int)


Proceso empleado[id:1..N]{
    text docu;
    int nro;
    while true{
        CrearDocu(docu)
        send esperando(id,dOCU)

    }
}

Proceso impresora[id:1..5]{
    text docu
    while true{
        send liberarImpresora(id)
        recive imprimir[id](docu)
        //imprime docu
    }
}

Proceso admin {
    cola libres;
    int id;
    int nro;
    text docu;
    while true{
        if(not empty(esperando)&& not libres.vacia()){
            recive esperando(id,docu)
            send imprimir[libres.pop()](docu)
        }
        else if(not empty(liberarImpresora(nro))){
            recive liberarImpresora(nro)
            libres.push(nro)
        }
    }
}
b) PMS
    Process empleado[id:1..N]{
        text Docu
        while true{
            crearDocu(docu)
            admin!imprimir(Docu)
        }
    }
.
    Processs impresora[id:1..5]{
        int docu
        while true{
            admin!listo(id)
            admin?imprimir(docu)
            imprimir(docu)
        }
    }
    .
    Process admin{
        cola docus(texto)
        Do empleado[*]?.imprimir(docu) -> docus.push(docu)
        .
        [] (not docus.esVacia()) impresora[*]?.listo(id) -> docu= docus.pop()
                                                        impresora[id]!.imprimir(docu)
        oD
    }