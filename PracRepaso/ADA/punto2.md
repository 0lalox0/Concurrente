Resolver el siguiente problema. En un negocio de cobros digitales hay P personas que deben pasar 
por la única caja de cobros para realizar el pago de sus boletas. Las personas son atendidas de acuerdo 
con el orden de llegada, teniendo prioridad aquellos que deben pagar menos de 5 boletas de los que 
pagan más. Adicionalmente, las personas ancianas tienen prioridad sobre los dos casos anteriores. 
Las personas entregan sus boletas al cajero y el dinero de pago; el cajero les devuelve el vuelto y los 
recibos de pago.  

task type persona;


task caja;
    Entry Pagar(Boletas text in,monto int float,vuelto out float,recibo out text)
    Entry PagarAnciano (Boletas text in,monto int float,vuelto out float,recibo out text)
    Entry pagarMenos(Boletas text in,monto int float,vuelto out float,recibo out text)
End caja;
task body persona is
    text[] boletas;
    float monto;
    float vuelto;
    text recibo;
    if(viejo)
        cajas.pagarAnciano(boletas,monto,vuelto,recibo)
    elif(boletas.size()< 5)
        cajas.pagarMenos(boletas,monto,vuelto,recibo)
    else
        cajas.pagar(boletas,monto,vuelto,recibo)
    end if;
    end persona

task body caja is
for i in 1 .. P loop
    select
       Accept PagarAnciano (Boletas text in,monto int float,vuelto out float,recibo out text) do
            facturar(Boletas,monto,vuelto,recibo)
        end PagarAnciano
    or
        (PagarAnciano'Count == 0)    Accept PagarMenos (Boletas text in,monto int float,vuelto out float,recibo out text) do
                                            facturar(Boletas,monto,vuelto,recibo)
                                    end PagarMenos
    or
        (PagarAnciano'Count == 0 && PagarMenos'Count == 0) ->
            Accept Pagar (Boletas text in,monto int float,vuelto out float,recibo out text) do
                facturar(Boletas,monto,vuelto,recibo)
            end Pagar
    end select
end loop
    


arrPersona is array (1..P) of persona

Begin
    null
End