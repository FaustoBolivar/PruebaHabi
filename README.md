# PruebaHabi

Los archivos Model.png y Model.sql incluido con el cuerpo de este readme hacen referencia al primer punto de  la prueba.</br> 

El archivo reporte vivienda.pbix contien el reporte en Power Bi de los datos depurados encontrados en el archivo technical_test_data_analyst.json. reporte vivienda.pdf contiene a forma de resumen el trabajo que se le realizo a los datos en R, por ultimo script r  contiene el script que se realizo para depurar y trabajar los datos en Power Bi.</br>

A continuacion se presentan las consultas y  las instrucciones en SQL 

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

a) a continuación se presentan los 10 medicamentos con mayor precio unitario</br>

SELECT NOMBREMEDICAMENTO
FROM(SELECT MD.NOMBREMEDICAMENTO, ROW_NUMBER() OVER(ORDER BY CA.PRECIONUNITARIO DESC) COSTOMAYOR
     FROM CONSUMO_AFILIADO CA
     INNER JOIN MEDICAMENTO MD ON(MD.IDMEDICAMENTO = CA.IDMEDICAMENTO)
     WHERE TO_CHAR(CA.FECHACONSUMO,'YYYY') = '2019')
WHERE COSTOMAYOR <= 10; </br>

b) SELECT FAR.IDLOCALIDAD
FROM CONSUMO_AFILIADO CAF
INNER JOIN FARMACIA FAR ON (FAR.IDFARMACIA = CAFIDFARMACIA)
INNER JOIN AFILIADO AFL ON (AFL.IDAFILIADO = CAF.IDAFILIADO)
INNER JOIN MEDICAMENTO MED ON (MED.IDMEDICAMENTO = FAR.IDMEDICAMENTO)
WHERE MED.NOMBREMEDICAMENTO = 'AMOXIDAL'
AND MONTHS_BETWEEN(AFL.FECHANACIMIENTO,SYSDATE) > 600
AND TO_CHAR(CAF.FECHACONSUMO,'YYYY') = '2020'; </br>

c) SELECT MIN(PME.PRECIO) PRECIO, MED.NOMBREMEDICAMENTO
FROM PRECIO_MEDICAMENTO PME
INNER JOIN MEDICAMENTO MED ON (MED.IDMEDICAMENTO = PME.IDMEDICAMENTO)
WHERE TO_CHAR(PME.FECHAPRECIO,'YYYY') = '2019'
GROUP BY MED.NOMBREMEDICAMENTO
HAVING COUNT(MED.NOMBREMEDICAMENTO) > 7; </br>


d) SELECT NOMBREFARMACIA 
FROM(SELECT FM.NOMBREFARMACIA, ROW_NUMBER() OVER(PARTITION BY FM.IDLOCALIDAD ORDER BY FM.FECHAAPERTURA DESC) IDAPERTURA
     FROM CONSUMO_MEDICAMENTO CM
     INNER JOIN FARMACIA FM ON (FM.IDMEDICAMENTO = CM.IDMEDICAMENTO))
WHERE IDAPERTURA = 1;
 
