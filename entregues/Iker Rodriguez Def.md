La meva funció, mira la puntuació del rival i en funció de si te més o menys punts es comporta d'una o altre forma.
En cas de que vagi guanyant, la funció és menys exigent a l'hora de decidir si es queda un número o no que si va perdent:
```
create function dbo.mElsQuedo (
@n_jugador as int,
@punts as int)
returns integer
as begin
	declare @PaMi int, @misPuntos int, @susPuntos int --Si es para mi será 1 (true), sino será 0 (false)
	set @misPuntos=coalesce ((select sum(puntsAnotats) from Marcador where nJugadorAnota=@n_jugador),0);
	set @susPuntos=coalesce ((select sum(puntsAnotats) from Marcador where nJugadorAnota!=@n_jugador),0);
	if @misPuntos>@susPuntos
		begin
			if @misPuntos<@susPuntos+@punts
				begin
					set @PaMi=1
				end
			else
			begin
				if @punts>3
				begin
					set @PaMi=1
				end
			else
			begin
				if @misPuntos>@susPuntos+@punts and @punts<3
					begin
						set @PaMi=0
					end
			else
			begin
				if @punts<2
					begin
						set @PaMi=0
					end
				end
			end
		end
	end;
	else 
		begin
			if @punts>2
				begin
					set @PaMi=1
				end
				else
				begin
					set @PaMi=0
				end
		end;
	return @PaMi
end;
```