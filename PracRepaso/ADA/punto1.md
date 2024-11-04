Resolver el siguiente problema. La página web del Banco Central exhibe las diferentes cotizaciones 
del dólar oficial de 20 bancos del país, tanto para la compra como para la venta. Existe una tarea 
programada que se ocupa de actualizar la página en forma periódica y para ello consulta la cotización 
de cada uno de los 20 bancos. Cada banco dispone de una API, cuya única función es procesar las 
solicitudes de aplicaciones externas. La tarea programada consulta de a una API por vez, esperando 
a lo sumo 5 segundos por su respuesta. Si pasado ese tiempo no respondió, entonces se mostrará 
vacía la información de ese banco. 

Procedure BancoCentral

Task type banco is
    Entry cotizacion(precioCompra OUT float, precoVenta OUT float)

Task Admin;

Task body Admin is
    loop
        for i in 1 .. 20 loop
            select
                arrBanco[i].cotizacion(precio)
                actualizarPrecio(i,precio)
            or delay(5)
                actualizaPrecio(i,null)
            end select
        end loop
        delay(tiempo)
    end loop
End admin

Task banco is
    float precio
    Begin
    loop
        Accept Cotizacion(precioCompra OUT float) do
            precioCompra = getPrecioCompra()
            precioVenta = getPrecioVenta()
        End cotizacion
    end loop;
End banco
arrBanco:ARRAY(1..20) of banco
Begin
    null
End