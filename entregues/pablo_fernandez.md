
# VERSIÓ 1.1

Només agafarem els números que siguin majors de 2, però si tenim menys punts que el contrari llavors els agafarem.

```

create function dbo.mElsQuedo( @nJugador int, @punts int ) 
returns bit
as begin
	declare @quedarme as bit, @mispuntos as int, @puntosadver as int
	set @mispuntos=(select sum(puntsAnotats) from marcador where nJugadorAnota=@nJugador)
	set @puntosadver=(select sum(puntsAnotats) from marcador where nJugadorAnota=@nJugador)
	if @punts <=2
		if @mispuntos<@puntosadver
			set @quedarme=1
		else
			set @quedarme=0
	else if @punts<=6
		set @quedarme=1
	return @quedarme
end

```
