Resolver el siguiente problema con PMA. En un negocio de cobros digitales hay P personas que 
deben pasar por la única caja de cobros para realizar el pago de sus boletas. Las personas son 
atendidas de acuerdo con el orden de llegada, teniendo prioridad aquellos que deben pagar menos 
de 5 boletas de los que pagan más. Adicionalmente, las personas embarazadas tienen prioridad sobre 
los dos casos anteriores. Las personas entregan sus boletas al cajero y el dinero de pago; el cajero les 
devuelve el vuelto y los recibos de pago. 

chan esperando(int,int)
chan esperandoMenos(int,int)
chan esperandoEmbarazada(int,int)
chan Recibo[id](int,text)
//chan hayPago()
Process persona[id:1..P]{
    int embarazada = getEmbarazada()
    int monto = getMonto()
    int vuelto = 0;
    text factura;
    int cantidad = getCantidad()
    if(embarzada)
        send esperandoEmbarazada(id,monto)
    else if(cantdiad < 5)
        send esperandoMenos(id,monto)
    else
        send esperando(id,monto)
    //send hayPago()
    recive Recibo[id](vuelto,factura)


}
Process caja{
    int monto,id,Vuelto;
    text Factura
    while true{
        if (not empty (esperandoEmbarazada)) -> 
            Recive esperandoEmbarzada(id,monto)
            Procesar(monto,Vuelto,Factura)
            send Recibo[id](Vuelto,Factura)
        []  (empty(esperandoEmbarazada) && not empty (esperandoMenor)) ->
                Recive esperandoMenor(id,monto)
                Procesar(monto,Vuelto,Factura)
                send Recibo[id](Vuelto,Factura)
        [] (empty(esperandoEmbarazada) && empty (esperandoMenor) && not empty esperando) ->Recive esperando(id,monto)
                     Procesar(monto,Vuelto,Factura)
                     send Recibo[id](Vuelto,Factura)
        fi

    }
}  

--------------------------------
Process caja{
    int monto,id,Vuelto;
    text Factura
    for i = 1 to P
        recive hayPago()
        if (not empty (esperandoEmbarazada)) -> 
            Recive esperandoEmbarzada(id,monto)
        []  (empty(esperandoEmbarazada) && not empty (esperandoMenor)) ->
                Recive esperandoMenor(id,monto)
        [] (empty(esperandoEmbarazada) && empty (esperandoMenor) && not empty esperando) ->Recive esperando(id,monto)        
        fi
            Procesar(monto,Vuelto,Factura)
            send Recibo[id](Vuelto,Factura)
    }
}  