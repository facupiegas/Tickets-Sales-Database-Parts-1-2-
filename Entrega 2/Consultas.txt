--Se pide 1
begin
select b.IdViaje, count (b.IdBoleto) 'Cantidad Boletos'
from Pasajero as p join boleto as b
on b.IdPasajero = p.IdPasajero
where p.EmailPasajero = 'MartyMcFly@gmail.com' and (month(b.FechaCompraBoleto) =1)
group by b.IdViaje 
order by b.IdViaje asc
end

--Se pide 2
begin
select *
from Tren as t
where t.CapacidadTren > 20 and not exists (select *
										   from viaje as v
										    where t.IdTren = v.IdTren and CONVERT(date, v.FechaHoraViaje) = CONVERT(date, getdate()+1))
end

--Se Pide 3 
begin
	select distinct b.IdBoleto, b.IdPasajero, p.AMaternoPasajero, p.APaternoPasajero, b.FilaAsiento, b.LetraAsiento
	from boleto as b join pasajero as p
	on b.IdPasajero = p.IdPasajero
	where b.IdViaje = 7256441
end

--se pide 4
begin
select * from Pasajero where Pasajero.IdPasajero in
(select IdPasajero from Boleto group by IdPasajero having count(IdPasajero)>5)
end

--Se pide 5	
begin
select  p.NombrePasajero
from Pasajero p join boleto b
on b.IdPasajero = p.IdPasajero
group by p.NombrePasajero
having count(b.idboleto) = (select MAX(S.Cantidad) as 'Cantidad de Boletos'
		from	( select  b.IdPasajero, count(b.IdPasajero) Cantidad
				  from boleto b
				  group by b.IdPasajero) as S )  
end