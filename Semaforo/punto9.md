9. Resolver el funcionamiento en una fábrica de ventanas con 7 empleados (4 carpinteros, 1 
vidriero y 2 armadores) que trabajan de la siguiente manera:
• Los carpinteros continuamente hacen marcos (cada marco es armando por un único 
carpintero) y los deja en un depósito con capacidad de almacenar 30 marcos.
• El vidriero continuamente hace vidrios y los deja en otro depósito con capacidad para 
50 vidrios.
• Los armadores continuamente toman un marco y un vidrio (en ese orden) de los 
depósitos correspondientes y arman la ventana (cada ventana es armada por un único 
armador


int cantMarcos = 0;
int cantVidrios = 0;
sem depositoMarco = 1;
sem depositoVidrio = 1;

Process carpintero[id:1..4]{
    while (true){
        //hace marco
        P(depositoMarco);
        if(cantMarcos < 30)
            cantMarcos++;
        V(depositoMarco);
    }
}

Process vidriero{
        while (true){
        P(depositoVidrio);
        if(cantVidrio < 50)
            cantVidrio++;
        V(depositoVidrio);
    }
}

Process armadores[id:1..2]{
    bool tengoMarco = false
    bool tengoVidrio = false
    while (true){
        while(!tengoMarco){
            P(depositoMarco);
            if(cantMarcos > 0){
                cantMarcos--;
                tengoMarco = true;
            }
            V(depositoMarco);
        }
        while(!tengoVidrio){
        P(depositoVidrio);
        if(cantVidrio > 0){
            cantVidrio--;
            tengoVidrio = true
            }
        V(depositoVidrio);
        }
        //arma ventana
        DepositarVentana()
        tengoMarco = false;
        tengoVidrio = false;
    }
}





Segunda Solucion

sem espacio_marcos = 30
sem espacio_vidrios = 50
sem deposito_marcos = 1
sem deposito_vidrios = 1
sem hayMarcos = 0
sem hayVidrios = 0
int cantMarco = 0;
int cantVidrio = 0;
int ventanas = 0;
sem deposito_ventanas = 1;
Process carpintero[id:1..4]{
    while (true){
        P(espacio_marcos)
        P(deposito_Marco);
         cantMarcos++;
        V(deposito_Marco);
        V(hayMarcos)
    }
}

Process vidriero{
    while (true){
        P(espacio_vidrio)
        P(deposito_Vidrio);
        cantVidrio++;
        V(depositoVidrio);
        V(hayVidrios)
    }
}

Process armadores[id:1..2]{
    while(true){
        P(hayMarcos)
        P(deposito_marcos)
        cantMarcos--;
        V(deposito_Marcos)
        V(espacio_Marcos)

        P(hayVidrio)
        P(deposito_Vidrio)
        cantVidrio--;
        V(deposito_vidrio)
        V(espacio_Vidrio)

        P(deposito_ventana)
        ventanas++;
        V(deposito_ventana)
    }
}