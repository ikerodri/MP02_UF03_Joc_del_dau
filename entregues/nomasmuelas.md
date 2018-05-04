# Me lo quedo
Intento de función del juego del dado, el contador de mi algoritmo es para que no se repita mas de una vez y asi no dar demasiados puntos a mi querido compañero de clase y rival en cuestión.

```
create function dbo.melsquedo(@nºjugador int, @puntuacionT int)
RETURNS bit
AS
BEGIN
	declare @boolean as bit;
	declare @contador as int;
	if @puntuacionT<3
	begin
		declare @puntsmios as int
		declare @puntsmios2 as int
		select  @puntsmios = puntsAnotats from marcador where nJugadorAnota = @nºjugador;
		select @puntsmios2 = puntsAnotats from marcador where nJugadorAnota != @nºjugador;
		if (@puntsmios - @puntsmios2 <5)
		begin
				set @boolean=0
				set @contador+=1
		end else begin
				set @boolean=1
				set @contador=0
		end
	end else begin
		set @boolean=1
		set @contador=0
	end
	if (@boolean=0 and @contador>0)
	begin
		set @boolean = 1
		set @contador = 0
	end
	return @boolean
END
```
