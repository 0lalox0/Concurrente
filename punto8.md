8. En un entrenamiento de fútbol hay 20 jugadores que forman 4 equipos (cada jugador conoce el equipo al cual pertenece llamando a la función DarEquipo()). Cuando un equipo está listo (han llegado los 5 jugadores que lo componen), debe enfrentarse a otro equipo que también esté listo (los dos primeros equipos en juntarse juegan en la cancha 1, y los otros dos equipos juegan en la cancha 2). Una vez que el equipo conoce la cancha en la que juega, sus jugadores 
se dirigen a ella. Cuando los 10 jugadores del partido llegaron a la cancha comienza el partido, juegan durante 50 minutos, y al terminar todos los jugadores del partido se retiran (no es necesario que se esperen para salir).

Process Jugador[id:1..20]:
    int numCancha;
    equipo[DarEquipo()].llegue(DarEquipo(),numCancha)
    Cancha.ir();

Process Partido[0..1] // ESNECESARIO???
    cancha[id].iniciar()
    //juegan 90 min
    cancha[id].fin()



monitor equipo[id:1..4]:
    int cantJugadores = 0
    cond jugadores
    procedure llegue(OUT numCancha)
        cantJugadores++
        if(cantJugadores < 5)
            wait(jugadores)
        else
            admin.DameCancha(numCancha)
            signal_all(jugadores)

monitor adminCancha:
    int cant = 0;

    proceudre DameCancha(OUT numCancha)
        cant++
        if(cant < 3)
            numCancha = 1
        esle
            numCancha = 2  

monitor Cancha[id:1..2]
    int cantJugadores = 0;
    cond esperando, iniciar
    procedure ir()
        cantJugadores++
        if(cantJugadores == 10)
            singal(iniciar)
        wait(esperando)
    procedure iniciar(){
        if(cantJugadores < 10)
            wait(iniciar)
    }
    procedure fin(){
        singal_all(esperando)
    }


    
