1. Resolver  con  MONITORES  el  siguiente  problema.  En  un  sistema  operativo  se  ejecutan  20 procesos  que
periódicamente  realizan  cierto  cómputo  mediante  la  función  Procesar().  Los  resultados  de  dicha  función  son 
persistidos en un archivo, para lo que se requiere de acceso al subsistema de E/S. Sólo un proceso a la vez puede hacer 
uso  del  subsistema  de  E/S,  y el  acceso  al mismo se define  por  la  prioridad  del  proceso (menor  valor indica mayor 
prioridad).

process proceso[id:1..20]{
    Resultado = Procesar()
    ES.escribir(id)
    //Escribe en E/S
    ES.fin()
}

monitor ES{
    cond esperando[1..20];
    colaOrdenada ordenada;
    boolean libre = true;
    procedure escribir(IN id){
        if(!libre)
            ordenada.pushOrdenado(id)
            wait(esperando[id])
        libre = false;

    }
    procedure fin(){  
        int i;  
        if(!empty(ordenada))    
            i = ordenada.pop()
            signal(esperando[i])
        else   
            libre = true;
    }

}


segunda Opcion


monitor colaES{
    cond esperando[1..20];
    colaOrdenada ordenada;
    boolean libre = true;
    procedure escribir(IN id){
        if(!libre)
            ordenada.pushOrdenado(id)
            wait(esperando[id])
        else
            libre = false;
    }
    procedure fin(){  
        int i;  
        if(!empty(ordenada))    
            i = ordenada.pop()
            signal(esperando[i])
        else   
            libre = true;
    }
}

monitor ES{
    procedure write(in resultado){
        //Escribiendo
    }
}