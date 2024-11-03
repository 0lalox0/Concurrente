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
        admin?.recibeid(id)
        admin?.labura(idAct)
        //labura
        Espectador[idAct]!.termine()
end

Procces admin
idAc:int
ids:cola of int
libre:boolean
Begin
    libre = true
    Espectador[*]?.llegue(id) -> ids.push(id)
    while(true)
        Do (libre)Espectador[*]?.llegue(id) -> ids.push(id)

       []  (!ids.esVacia() and libre == true) maquina?.libre(id) -> 
                                                                    libre = false
                                                                    idAc = ids.pop()
                                                                    
                                                                    Espectador[idAc]!.teToca()
                                                                    maquina!.labura(idAct)






End