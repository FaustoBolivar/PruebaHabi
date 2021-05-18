# PruebaHabi

a) Indicar el nombre de los medicamentos consumidos durante el 2019 de mayor precio unitario (o
sea el precio unitario más alto).</br>

b) Indicar el id de las localidades de las farmacias donde afiliados mayores de 50 años consumieron
el medicamento “AMOXIDAL” durante el año 2020.</br>

c) Encontrar el precio más bajo para el 2019 de cada medicamento, para aquellos que hayan sufrido
más de 7 cambios de precio durante ese año.</br>

d) Hallar el nombre de las farmacias que hayan vendido algún medicamento y que sean las de menor
antigüedad (respecto a su fecha de apertura), dentro de las farmacias que pertenecen a su misma
localidad.</br>

RESPUESTA:</br>

a) a continuación se presentan los 10 medicamentos con mayor precio unitario

SELECT NOMBREMEDICAMENTO FROM (SELECT MD.NOMBREMEDICAMENTO, ROW_NUMBER() OVER(ORDER BY CA.PRECIOUNITARIO DESC) COSTOMAYOR FROM CONSUMO_AFILIADO CA INNER JOIN MEDICAMENTO MD ON(MD.IDMEDICAMENTO=CA.IDMEDICAMENTO) WHERE TO_CHAR(CA.FECHACONSUMO,'YYYY')='2019') WHERE COSTOMAYOR<=10

