# Competència T-SQL Datum.
## Participa fent el teu Pull Request.
## [Dilluns dia 7 a les 18:00 la gran final](https://challonge.com/basededades).

Aquesta prova està basada en la [competència de bits en ring **'Datum'**](http://bitsenelring.blogspot.com.es/2013/06/nueva-competencia-datum.html):

>**Reglas del juego**
>
>Juegan dos jugadores, usando un dado común de seis caras.
>
>En cada turno, un jugador toma el dado, lo arroja, y decide cuál de estas dos cosas hacer:
> * Pasar el turno al otro jugador, anotándose los puntos que indica el dado.
> * Mantener el turno, pero anotarle al otro jugador los puntos que indica el dado.
>
>Esto se repite una y otra vez (cambiando o no de turno, según las decisiones de los jugadores), ~~hasta que uno de los jugadores llega a 30 puntos o más y es declarado ganador.~~

Al nostre cas no es traca d'arribar a 30 punts sinó d'acumular més punts en 10K tirades del dau.

Per participar cal entregar una funció T-SQL a la carpeta [entregues](./entregues) mitjançant un Pull Request.

**Requeriments de l'entrega:**

* **Format:** El Pull Request ha de ser un fitxer amb extensió `.MD`. Ha de tenir un títol amb el nom de l'algorisme, una descripció de com actua i un code bloc amb el codi T-SQL de la funció ( incloent el `CREATE FUNCTION` ).
* **La Funció:** Rep dos paràmetres: Quin número de jugador sóc i quina puntuació ha sortit al dau. Ha de retornar un valor de tipus `bit`. Un `1` vol dir que es queda amb els punts, un `0` vol dir que no els vol.

Exemple de funció (no vàlida perquè no compleix la regla adicional explicada a continuació):

```
create function dbo.mElsQuedo( @nJugador int, @punts int ) returns bit as
begin
    return 1; -- Funció que sempre es queda els punts.
end
```

**Regla adicional**

Els punts de la partida es van acumulant a la taula Marcador:

```
create table Marcador (
  tirada int not null identity,
  nJugadorAnota int,
  puntsAnotats int
)
```

La regla adicional ens diu que és obligatori fer algun tipus de consulta a aquesta taula dins la funció.


**Com competiran els algoritmes entre si?**

Aquest és el codi que fa competir dos algoritmes. A cada competició farem l'anada i la tornada. Guanyarà qui hagi fet més punts entre anada i tornada. Anada i tornada vol dir que un cop comença la partida un algoritme i l'altre cop l'altre.

```
declare @n int;
set @n = 10000;

drop table if exists Marcador;
create table Marcador (
  tirada int not null identity,
  nJugadorAnota int,
  puntsAnotats int
)

while @n > 0 BEGIN
    declare @dau int;
    declare @selqueda bit;
    set @selqueda = 0;
    while (@selqueda = 0 and @n > 0 ) begin
        set @dau = (cast(rand()*10000 as int) % 6 ) + 1;
        set @selqueda = dbo.f1( 1, @dau );
        insert into Marcador (nJugadorAnota, puntsAnotats)
               values (1, @dau );
        set @n = @n - 1;
        print 'Jugador 1: ' + str(@dau) + ' s''el queda ' + str(@selqueda)
    end
    set @selqueda = 0;
    while ( @selqueda = 0 and @n > 0 ) begin
        set @dau = (cast(rand()*10000 as int) % 6 ) + 1;
        set @selqueda = dbo.f2( 2, @dau );
        insert into Marcador (nJugadorAnota, puntsAnotats)
               values (2, @dau );
        set @n = @n - 1;
        print 'Jugador 2: ' + str(@dau) + ' s''el queda ' + str(@selqueda)
    end
END

--resultats
select nJugadorAnota, sum(puntsAnotats)
from marcador
group by nJugadorAnota
order by nJugadorAnota;
```

**Premis**

Al guanyador tots li farem :+1: al fitxer pujat al seu Pull Request.

**Canvis de les regles**

L'organització es permet variar les regles en casos excepcionals per a millorar l'experiència o fer-ho més just.

