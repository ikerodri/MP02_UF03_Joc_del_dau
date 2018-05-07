# Joc
Esta función mira si el número dado es menor que 3 si es para él o para mi
```
create function dbo.mElsQuedo (
@n_jugador as int,
@punts as int)
returns integer
as begin
	declare @paraMi int, @misPuntos int, @susPuntos int  --Si es para mi será 1 (true), sino será 0 (false)
	set @misPuntos=(select sum(puntsAnotats) from Marcador where nJugadorAnota=@n_jugador);
	set @susPuntos=(select sum(puntsAnotats) from Marcador where nJugadorAnota!=@n_jugador);
	if @misPuntos>@susPuntos
		begin
			if @misPuntos<@susPuntos+@punts
			begin
				set @paraMi=1
			end
			else
			begin
				if @punts>2
				begin
					set @paraMi=1
				end
				else
				begin
					set @paraMi=0
				end
			end
		end;
	else 
		begin
			if @punts>2
				begin
					set @paraMi=1
				end
				else
				begin
					set @paraMi=0
				end
		end;
	return @paraMi
end;
```
