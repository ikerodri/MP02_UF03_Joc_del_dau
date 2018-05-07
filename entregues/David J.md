# Agafo o no

Aquesta funció és queda tots aquells numeros que siguin més grans que 3 o 3 en cas que vagi perdent.

```
create function dbo.quedo (@numjugador as int,@punts as int)
returns integer
as begin
	declare @sino int, @total_altre int, @total_jo int;
	set @total_jo = 
		(select sum(puntsanotats) from Marcador where nJugadorAnota = @nJugador );
	set @total_altre = 
		(select sum(puntsanotats) from Marcador where nJugadorAnota != @nJugador );

	if @punts between 0 and 3
		begin
		if @punts=3 and @total_jo+@punts<@total_altre
			begin
				set @sino=1
			end;
		else
			begin
				set @sino=0
			end;
		end;
	else 
		begin
			set @sino=1
		end;
	return @sino
end;
```
