En un estadio de fútbol hay una máquina expendedora de gaseosas que debe ser usada por 
E  Espectadores  de  acuerdo  con  el  orden  de  llegada.  Cuando  el  espectador  accede  a  la 
máquina  en  su  turno  usa  la  máquina  y  luego  se  retira  para  dejar  al  siguiente.  Nota:  cada 
Espectador una sólo una vez la máquina.


Procces Espectador [id:1..E]
Begin
    admin!.llegue(id)
    admin?.teToca()
    //Usa la maquina
    maquina?.termine()
End
Process maquina
id:int
Begin
    while true
        admin!.libre()
        admin?.labura(idAct)
        //labura
        Espectador[idAct]!.termine()
end

Procces admin
idAc:int
ids:cola of int
Begin
    Do Espectador[*]?.llegue(id) -> ids.push(id)
    []  (!ids.esVacia()) maquina?.libre(id) -> idAc = ids.pop()
                                               Espectador[idAc]!.teToca()
                                               maquina!.labura(idAct)
End

---------------------------------------
Procces Espectador [id:1..E]
Begin
    admin!llegue(id)
    admin?teToca()
    //Usa la maquina
    admin!termine()
End
Procces admin
idAc:int
ids:cola of int
libre:boolean
Begin
    libre = true
    Do Espectador[*]?.llegue(id) -> if(not libre)
                                        ids.push(id)
                                    else
                                        Espectador[idAc]!teToca()
                                        libre = false
    []  Espectador[*]?libre(id) ->  if(ids.esVacia)
                                        libre = true
                                    else
                                        Espectador[ids.pop()]!teToca()
End 