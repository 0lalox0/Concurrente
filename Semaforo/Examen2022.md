1. Resolver con SEMÁFOROS el siguiente problema. En una planta verificadora de vehículos, existen 7 estaciones donde
se  dirigen  150  vehículos  para  ser  verificados.  Cuando  un  vehículo  llega  a  la  planta,  el  coordinador  de la  planta  le 
indica a qué estación debe dirigirse. El coordinador selecciona la estación que tenga menos vehículos asignados en ese 
momento. Una vez que el vehículo sabe qué estación le fue asignada, se dirige a la misma y espera a que lo llamen 
para verificar. Luego de la revisión, la estación le entrega un comprobante que indica si pasó la revisión o no. Más allá 
del resultado, el vehículo se retira de la planta. Nota: maximizar la concurrencia.


sem fila = 0;
sem mutexCola= 1;
sem mutexColas[1..7] = ([7] = 1);
sem filaEstaciones[1..7] = ([7] = 1);
sem usandoCorta = 0;
cola vehiculosEnEstacion[1..7];
cola vehiculos;
int estacionCorta;
boolean aprobado[1..7];
int x = 0;
Process Vehiculo[1..150]{
    int E;
    boolean A;
    P(mutexCola)
    vehiculos.push(id);
    V(mutexCola)
    P(fila);
    int E = estacionCorta;
    V(usandoCorta)
    P(mutexColas[E])
    vehiuclosEnEstacion[E].push(id)
    P(filaEstaciones[E])
    A = aprobado[E];
    V(usandoAprobado)
    
}   

Process Coordinador{
    int estaciones[1..7] = ([7] = 0);
    int corta;
    while(x < 150){
        if(!empty(vehiculos)){
            vehiculos.pop()
            estacionCorta = 9999999;
            for(i = 1 to 7){
                if(estacionCorta > estaciones[i]){
                    corta = i;
                }
            } 
            estacionCorta = corta;
            estaciones[corta]++;
            V(fila);
            x++;
            P(usandoCorta)
        }
    }

}

Procces Estacion[1..7]{
    int v;
    while (x < 150){
        if(!empty(vehiculosEnEstacion[id])){
          v = vehiculosEnEstacion[id].pop()
          //verifica si esta aprobado
          aprobado[id] = true // o false
          V(filaEstaciones[E])
          P(usandoAprobado);
        }
    }
}