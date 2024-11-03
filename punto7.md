Procces Corredor [id:0..C-1]:
    Carrera.llegue()
    //inicia carera
    //fin
    maquina.Tomar()


Procces Repositor:



Monitor Carrera:
    int corredores = C
    cond esperando;
    llegue()
        corredores--
        if(corredores = 0)
            signal_all(esperando)
        else
            wait(esperando)


Monitor Maquina:
    int cantBotellas = 20
    boolean reponiendo = false;
    int cantEspera = 0;
    cond esperando, esperandoReponer;
    boolena trabado = false;
    Procedure Tomar()
        if(reponiendo or esperando > 0 or trabado)
            esperando++
            wait(esperando)
            esperando--
        if(cantBotellas == 0)
            signal(empEsperando)
            trabado = true
            wait(esperandoReponer)
            trabado = false
        cantBotellas--;
        if(esperando > 0)
            signal(esperando)
        

    Procedure iniReponer()
        if(cantBotellas > 0)
            wait(empEseperando)
        reponiendo = true
    
    Procedure FinReponer()
        cantBotellas = 20
        reponiendo = false
        if(trabado)
            signal(esperandoReponer)
        else
            signal(esperando)



//no existe la cola

Segunda opcion

Monitor Maquina
    int cantBotellas = 20
    boolean reponiendo = false;
    boolean libre = true;
    int cantEspera = 0;
    cond esperando, esperandoReponer;

    procedure llegue()
        if(!libre)
            cantEspera++
            wait(esperando)
            cantEspera--
        else
            libre = false;
    
    procedure TomarBotella()
        if(cantBotellas == 0)
            signal(empEsperando)
            wait(esperandoReponer)
        cantBotellas--;
        if(cantEspera > 0)
            signal(esperando)
        else
            libre = true
    procedure inicioReponer()
        if(cantBotellas > 0)
            wait(empEsperando)

    procedure FinReponer()
        cantBotellas = 20;
        signal(esperandoReponer)

Tercera Opcion

Monitor Cola
    cond esperando
    bool libre = true;
    int cantEspera = 0;
    procedure llegue()
        if(!libre)
            cantEspera++
            wait(esperando)
            cantEspera--
        else
            libre = false;
    procedure Irse()
        if(cantEspera > 0)
            signal(esperando)
        else
            libre = true

Monitor Maquina
     int cantBotellas = 20
     cond esperandoReponer,empEsperando;
     procedure TomarBotella()  
        if(cantBotellas == 0)
            signal(empEsperando)
            wait(esperandoReponer)
        cantBotellas--
    
    procedure Reponer()
        if(cantBotellas > 0)
            wait(empEsperando)
        cantBotellas = 20;
        signal(esperandoReponer)
