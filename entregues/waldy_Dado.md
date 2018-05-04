#Juego del dado WSAsociados
go
create function dbo.tirada (@dado int , @puntos int )
returns bit
as begin
declare  @correcto bit ;
set @correcto =  1
begin 
	if @puntos <3  return @correcto 	
	else set @correcto = 0
	end
return @correcto
end
