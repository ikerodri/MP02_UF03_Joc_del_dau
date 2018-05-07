# Juego del dado WSAsociados
go
create function dbo.dados(@njugador int , @punts int )
returns bit
as begin
declare @si bit;
declare @puntosaliados int;
declare @puntosenemigos int;
declare @diferencia int;

set @puntosaliados =(select SUM (puntsanotats) from marcador
				  where njugadoranota = 1)
				  
set @puntosenemigos =(select SUM (puntsanotats) from marcador
				  where njugadoranota = 2)
				  
set @diferencia = (@puntosaliados - @puntosenemigos)

if @puntosaliados > @puntosenemigos and @diferencia > @punts set @si = 0
else set @si = 1				
return @si 
end
