
# VERSIÓ 1.1

Només agafarem els números que siguin majors de 2, però si tenim menys punts que el contrari llavors els agafarem.

```

create function dbo.mElsQuedo( @nJugador int, @punts int ) 
returns bit
as begin
	declare @quedarme as bit, @mispuntos as int, @puntosadver as int
	set @mispuntos=coalesce((select sum(puntsAnotats) from marcador where nJugadorAnota=@nJugador),0)
	set @puntosadver=coalesce((select sum(puntsAnotats) from marcador where nJugadorAnota=@nJugador),0)
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
