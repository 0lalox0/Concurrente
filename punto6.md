6. Existe una comisión de 50 alumnos que deben realizar tareas de a pares, las cuales son 
corregidas por un JTP. Cuando los alumnos llegan, forman una fila. Una vez que están todos 
en fila, el JTP les asigna un número de grupo a cada uno. Para ello, suponga que existe una 
función AsignarNroGrupo() que retorna un número “aleatorio” del 1 al 25. Cuando un alumno 
ha recibido su número de grupo, comienza a realizar su tarea. Al terminarla, el alumno le avisa 
al JTP y espera por su nota. Cuando los dos alumnos del grupo completaron la tarea, el JTP 
les asigna un puntaje (el primer grupo en terminar tendrá como nota 25, el segundo 24, y así 
sucesivamente hasta el último que tendrá nota 1). Nota: el JTP no guarda el número de grupo 
que le asigna a cada alumno.


process alumno [id:0..49]:
    int nroGrupo;
    int nota;
    aula.llegue(nroGrupo)
    //realiza Tarea
    Banco.entregarTarea(nroGrupo,nota)

process JTP:
int x;
for i = 0 to 49
    x = AsignarNroGrupo()
    aula.depositarNumero(x);
asignarNota()


monitor aula
    cond fila
    int alumnos = 0;
    int nroGrupo;
    procedure llegue(OUT x)
        alumnos++
        if(alumnos == 50)
            signal(duermeJTP)
        wait(fila)
        x = nroGrupo
    procedure depositarNumero(in x)
        if(alumnos < 50)
            wait(duermeJTP)
        nroGrupo = x
        signal(fila)

monitor Banco
    cond esperandoNota[1..25]
    cola grupoEsperando;
    int notas[1..25];
    int cantEsperando[1..25] = ([25] = 0)
    int notaDefault = 25;
    int grupoE;

    procedure entregarTarea(nroGrupo,nota):
        cantEsperando[nroGrupo]++
        if(cantEsperando[nroGrupo] == 2)
            grupoEsperando.push(nroGrupo)
            signal(esperandoJTP)
        wait(esperandoNota[nroGrupo])
        nota = notas[nroGrupo]
    
    procedure asignarNota()
        while(nota > 0)
            if(!empty(grupoEsperando))
                grupoE = grupoEsperando.pop();
                notas[grupoE] = notaDefault;
                notaDefault--
                signal_all(esperandoNota[grupoE])
         wait(esperandoJTP)

        




    