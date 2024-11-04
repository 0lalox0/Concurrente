Resolver el siguiente problema con PMS. En la estación de trenes hay una terminal de SUBE que 
debe ser usada por P personas de acuerdo con el orden de llegada. Cuando la persona accede a la 
terminal, la usa y luego se retira para dejar al siguiente. Nota: cada Persona usa sólo una vez la 
terminal.  

Process Personas[id:1..P]
    admin!.llegue(id)
    terminal?.usar()
    //usaTerminal
    terminal!.fin()

Process Terminal
id:int
   while true{
        admin!.listo()
        admin?.quien(id)
        Personas[id]!.usar()
        Personas[id]?.fin()
    }


Process admin
    id:integer
    ids:cola of integer
Begin
    for i in 1..P*2
        if Personas[*].llegue(id) -> ids.push(id)  
        [] (not ids.esVacia()) Termina?.listo() -> terminal!.quien(ids.pop())
End


--------------------
Process Persona[id:1..P]{
        terminal!.llegue(id)
        terminal?usar()
        terminal!fin()
}
Process Terminal
    id:integer
    ids:cola of integer
    libre = true
Begin
    for i in 1..P*2
        if Personas[*]?llegue(id) ->if(libre){
                                        Personas[id]!usar()
                                        libre = false
                                    } else 
                                        ids.push(id)
        [] Personas[*]?fin -> if (not ids.esVacia()){
                                    Personas[ids.pop]!usar()
                                    }else
                                        libre = true
End