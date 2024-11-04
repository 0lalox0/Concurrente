Resolver el siguiente problema. La oficina central de una empresa de venta de indumentaria debe 
calcular cuántas veces fue vendido cada uno de los artículos de su catálogo. La empresa se compone 
de 100 sucursales y cada una de ellas maneja su propia base de datos de ventas. La oficina central 
cuenta con una herramienta que funciona de la siguiente manera: ante la consulta realizada para un 
artículo determinado, la herramienta envía el identificador del artículo a las sucursales, para que cada 
una calcule cuántas veces fue vendido en ella. Al final del procesamiento, la herramienta debe 
conocer cuántas veces fue vendido en total, considerando todas las sucursales. Cuando ha terminado 
de procesar un artículo comienza con el siguiente (suponga que la herramienta tiene una función 
generarArtículo() que retorna el siguiente ID a consultar). Nota: maximizar la concurrencia. Existe 
una función ObtenerVentas(ID) que retorna la cantidad de veces que fue vendido el artículo con 
identificador ID en la base de la sucursal que la llama.

Procedure oficina

task type sucursal

task central
    entry total(cant in int)
    entry CuantosVendi(id in int)

task body sucursal is
    idP:integer
    cant:integer
    Begin
    loop
        central.CuantosVendi(id in int)do
        cant = ObtenerVentas(idP)
        central.total(cant)
        central.termine()
    end loop
    End

task body central is
    idP:integer
    Begin
        loop
            idP= generarArticulo()
            for i in 1..200 loop
            select
                Accept cuantosVendi(id in int) do
                    id = idP
                end cuantosVendi
            or
                Accept total(cant in int) do
                    agregarTotal(id,cant)
                end total
            end loop
            for i in 1..100
                Accept termine()
        end loop
    End



arrSursales (1..100) of sucursal

Begin
null
end