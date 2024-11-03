Consigna
 Se debe modelar el funcionamiento de una casa de comida rápida, en la cual trabajan 2 
cocineros y 3 vendedores, y que debe atender a C clientes. El modelado debe considerar 
que: - - - 
    -Cada cliente realiza un pedido y luego espera a que se lo entreguen. 
    -Los pedidos que hacen los clientes son tomados por cualquiera de los vendedores y se 
    lo pasan a los cocineros para que realicen el plato. Cuando no hay pedidos para atender, 
    los vendedores aprovechan para reponer un pack de bebidas de la heladera (tardan entre 
    1 y 3 minutos para hacer esto). 
    -Repetidamente cada cocinero toma un pedido pendiente dejado por los vendedores, lo 
    cocina y se lo entrega directamente al cliente correspondiente. 
Nota: maximizar la concurrencia.

Codigo:
Chan Pedidos(int,text)
Chan Comida[1..c](comida)
Chan Cocineros(int,text)
Process Cocinero[id:1..2]
{   comida comida;
    text pedido;
    int idC;
    while(true){
        Recive Cocinero(idC,pedido)
        Comida = ArmarComida(pedido)
        Send Comida[idC](comida)
    }

}

Process Vendedor[id:1..3]
{   int idC;
    text pedido
    while(true){
        if(empty(Pedidos)){
            ArmarPackCervezas
        }else{
            Recive Pedido(idC,pedido)
            Send  Cocinero(idC,pedido)
        }
    }

}

Process Cliente[id:1..C]
{
    comida comida;
    text pedido;
    pedido = crearPedido;
    Send Pedidos(id,pedido)
    Recive Comida[id](comdia)
}
------
chan espera(int)
Chan siguiente[3](int,text) // Chan siguiente(int,text)
Procces Vendedor[id:1..3]{
    int idC;
    text pedido;
    while(true){
    send espera(id)
    recive siguiente[idV](idC,pedido) //recive siguiente(idC,pedido)
    if(idc == -1){
        ArmarPackCerveza();
    }else{
        send Cocineros(idC,pedido)
    }
    }

}
Procces Admin{
    int idV
    int idC
    pedido pedido
    while (true){
        idC = -1
        pedido = null;
        recive Espera(idV)
        if(!empty(pedido)){
            recive pedidos(idC,pedido)
        } 
        send siguiente[idV](idC,pedido) // send siguiente(idC,pedido)
    }
}