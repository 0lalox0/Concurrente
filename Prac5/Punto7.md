Hay un sistema de reconocimiento de huellas dactilares de la policía que tiene 8 Servidores 
para realizar el reconocimiento, cada uno de ellos trabajando con una Base de Datos propia; 
a su vez hay un Especialista que utiliza indefinidamente. El sistema funciona de la siguiente 
manera: el Especialista toma una imagen de una huella (TEST) y se la envía a los servidores 
para que cada uno de ellos le devuelva el código y el valor de similitud de la huella que más 
se asemeja a TEST en su BD; al final del procesamiento, el especialista debe conocer el 
código de la huella con mayor valor de similitud entre las devueltas por los 8 servidores. 
Cuando ha terminado de procesar una huella comienza nuevamente todo el ciclo. Nota: 
suponga que existe una función Buscar(test, código, valor) que utiliza cada Servidor donde 
recibe como parámetro de entrada la huella test, y devuelve como parámetros de salida el 
código y el valor de similitud de la huella más parecida a test en la BD correspondiente. 
Maximizar la concurrencia y no generar demora innecesaria.

Task Especialista
Entry BuscarHuella(huella OUT data )
Entry DevolverData(id IN int,codigo IN int,Valor IN float)

Task type Servidor;
    Entry quienSoy(id in integer)


Task body Especialista
    huellasSimilares:cola
    huella:data
    loop
    huella = newHuella()
        for i = 1 to 16 loop
        select
            Accept BuscarHuella(huella OUT data)
                huella = huella
            End accept
        or  Accept DevolverData(id IN int,codigo IN int,Valor IN float)
                huellasSimilares.push(codigo,Valor,id)
            End accept
        end loop
        SacarMaximo(huellasSimilares)
    end loop
task body Servidor
    test:data
    idYo:integer
    codigo:integer
    valor:float
    loop
        Accept quienSoy(id in integer)
            idYo = id
        end quienSoy;
        Especialista.BuscarHuella(test)
        Buscar(test, código, valor)
        Especialista.DevolverData(codigo,valor,idYO)
    end loop
arrServidor(id:1..8) of Servidor
Begin
    for i = 1 to 8
        Servidor[i].quienSoy(i)
end