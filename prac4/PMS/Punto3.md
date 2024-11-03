Consigna:En un examen final hay N alumnos y P profesores. Cada alumno resuelve su examen, lo 
entrega y espera a que alguno de los profesores lo corrija y le indique la nota. Los 
profesores corrigen los exámenes respetando el orden en que los alumnos van entregando.  
a) Considerando que P=1. 
b) Considerando que P>1. 
c) Ídem b) pero considerando que los alumnos no comienzan a realizar su examen hasta 
que todos hayan llegado al aula. 
Nota: maximizar la concurrencia; no generar demora innecesaria; todos los procesos deben 
terminar su ejecución
Coidgo:
b)
Process alumno[id:1..N]{
    text Examen = HacerExamen()
    int nota;
    int idP;
    admin!.port(id,Examen)
    admin?.quien(idP)
    profesores[idP].port(nota)
}

Process profesores[id:1..P]{
    int cont,idA,nota;
    text Examen;
    bool segui = true
    while(segui){
        admin!.listo(id)
        admin?.corregime(idA,Examen,segui)
        if(segui)
            nota =CorregirExamen(Examen)
            alumno![idA].port(nota)
        }
    }
}
Process admin{
    int cont = 0;
    text Examen;
    int idA,idP;
    cola buffer
    Do  cont < N; alumno[*]?.port(idA,Examen) ->    buffer.push(idA,Examen)
                                                    cont++                
        not empty(buffer) profesores[*]?.listo(idP) ->    buffer.pop(idA,Examen)
                                                          alumno[idA]!.quien(idP)
                                                          profesores[idP]!.corregime(idA,Examen,true)
    oD
    for 1 to P {    
        profesores[*]?.listo(idP)
        profesores[idP]!.corregime(-1,null,false)
    }
}