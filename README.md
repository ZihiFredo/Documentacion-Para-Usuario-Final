# Documentacion-Para-Usuario-Final

# ProyectoVoxmapp

El proyecto trata de falicitar el uso de datos que recibe una empresa a travez de una encuesta. Esta encuesta esta divida en preguntas las cuales son transferidas a un google sheets para que despues pasen a una base de datos y asi los datos ya esten dividos, ordenados y listos para ser utilizados

## Tabla de contenidos

+ [Informacion General](https://github.com/ZihiFredo/Documentacion-Para-Usuario-Final/blob/main/README.md#informacion-general)
+ [Tecnologia Usada](https://github.com/ZihiFredo/Documentacion-Para-Usuario-Final/blob/main/README.md#tecnologia-usada)
+ [Caracteristicas](https://github.com/ZihiFredo/Documentacion-Para-Usuario-Final/blob/main/README.md#caracteristicas)
+ [Screenshot](https://github.com/ZihiFredo/Documentacion-Para-Usuario-Final/blob/main/README.md#screenshot)
+ [Set up](https://github.com/ZihiFredo/Documentacion-Para-Usuario-Final/blob/main/README.md#set-up)
+ [Uso](https://github.com/ZihiFredo/Documentacion-Para-Usuario-Final/blob/main/README.md#uso)
+ [Estatus del Proyecto](https://github.com/ZihiFredo/Documentacion-Para-Usuario-Final/blob/main/README.md#estatus-del-proyecto)
+ [Mejoras del Proyecto](https://github.com/ZihiFredo/Documentacion-Para-Usuario-Final/blob/main/README.md#mejoras-en-el-futuro)


## Informacion general

#### Problema
Voxmapp quiere mejorar su proceso de recepción y manejo de datos. Estos datos son sobre la infraestructura de salud de hospitales en Afganistán y se reciben a través de un cuestionario que se llena una vez al mes por personal de los hospitales a quienes se les notifica por telefono. Es importante resaltar que estos hospitales están en un contexto de inestabilidad y a veces en zonas remotas. Actualmente reciben los datos como un solo documento en el que las diferentes respuestas al cuestionario se separan por comas y otros caracteres. Además, los datos no se reciben con un formato estándar debido a que se llenan desde diferentes regiones y contextos, lo que causa problemas a la hora de segmentar y analizar los datos. No es una solución óptima que alenta el proceso de pasar la información de la encuesta a la base de datos. Este último proceso se termina haciendo a mano. Poder encontrar un método más eficiente de recepción y estandarización de datos sería de su interés. Luego de analizar los datos, Voxmapp sube estos periodicamente a Tableu. El problema aquí se presenta a la hora de querer actualizar las vistas en Tableu, pues se debe de hacer manualmente. Encontrar la forma de automatizar este proceso también sería de su interés. 

#### Usuario
El Hospital se ubica en un distrito, de una región, de un país y dichos distrito, región y páis pueden tener más de un hospital en ellos, para ello será necesario un catálogo de países, regiones y distritos para el alta de los hospitales (se propone la relación de N Hospitales en 1 Región). En el hospital se registran los datos estáticos del mismo (ubicación,estado general de la infraestructura, etc). Cada hospital tiene un inventario propio en el que se almacenan diferentes recursos como medicinas, equipamiento médico, etc. (se propone la relación de N Recursos en 1 Inventario en 1 Hospital). El hospital tiene varios doctores (Lorenzo menciona en la entrevista algo que da a entender que algunos doctores pasan por más de un hospital o clínica, tenemos que preguntar bien si se refería a que se mantenían contratados en varios por falta de personal calificado en las regiones o si estaban en un solo trabajo a la vez; asumimos la primera situación hasta que tengamos respuesta) y personal; el personal tiende a permanecer por largos periodos de tiempo en los hospitales por lo que asumimos que ellos solo están contratados en un hospital a la vez (se proponen las relaciones de N Doctores en N Hospitales y N Personal en 1 Hospital). Al registrar el hospital se registran 3 Contactos (proponemos la relación  1 Hospital con 3(N) Contactos) 

Se ha dicho que los hospitales tratan casos COVID con frecuencia (proponemos N Casos covid en 1 Hospital). Por lo tanto se debe crear una entidad de Casos Covid. Los casos covid que se tratan asumimos que son seguidos por un solo doctor y tratados por varios miembros del personal (Proponemos 1 Doctor para N Casos Covid y N Personal para N Casos Covid). 

El estado de los hospitales es revisado por Voxmapp periódicamente a través de cuestionarios contestados por un miembro del personal de cada hospital registrado, se actualizan así los datos que se tienen del hospital en cuestión. Cada cuestionario es contestado por un miembro del personal, y esto representa el influjo de los datos de cada hospital y su actualización (proponemos las relaciones 1 Personal contesta N Cuestionarios y 1 Cuestionario crea 1 Actualización).

## Tecnologia Usada

Se utilizan principalmente 4 elementos grandes para que el proyecto funcione:

1. __Google forms__: Aqui es donde se creo la encuesta y se reciben los datos para que la informacion pueda seguir su flujo
2. __Google sheets__: Se guarda la informacion de la encuesta para poder pasara a la base de datos
3. __PostgresSQL__: PostgreSQL es un sistema de gestión de bases de datos relacional orientado a objetos que utilizamos para poder crear la base de datos
4. __DBeaver__: DBeaver es una aplicación de software cliente de SQL en donde creamos la base de datos y le damos mantenimnto al igual que creamos tablas para responder preguntas que pueden ser contestadas con la informacion que tenemos guarda en la base de datos

## Caracteristicas

Las principales caracteristicas de nuestro proyecto 

## Screenshot

#### Ejemplo de una grafica

Se muestra una grafica que fue creada con la informacion rescatada de la encuesta que ahora esta alamcenada en la base de datos

Tiempo promedio de reservas: Tiempo promedio existente de reservas para cada recurso registrado en cada hospital, asi como sus caracteristicas geograficas

![ER_voxmapp_dbeaver](https://user-images.githubusercontent.com/77375206/117859721-fdcd0180-b254-11eb-8251-1de45397df5d.PNG)

```python
--

create view necesidades_hospital as(
	with reservas_num as (
		select
		    case r.oxygen_reserves 
		      when 'Yes, for 30 days' then 30
		      when 'Yes, for 15 days' then 15
		      when 'Yes, for 7 days' then 7
		      when 'Yes, for 3 days' then 3
		      when 'No' then 0
		    end as oxygen_reserves_num,
		    case r.antipyretics_reserves 
		      when 'Yes, for 30 days' then 30
		      when 'Yes, for 15 days' then 15
		      when 'Yes, for 7 days' then 7
		      when 'Yes, for 3 days' then 3
		      when 'No' then 0
		    end as antipyretics_reserves_num,
		    case r.anesthetics_and_muscular_relaxants
		      when 'Yes, for 30 days' then 30
		      when 'Yes, for 15 days' then 15
		      when 'Yes, for 7 days' then 7
		      when 'Yes, for 3 days' then 3
		      when 'No' then 0
		    end as anesthetics_and_muscular_relaxants_num,
		    case r.alcohol_reserves_and_handsoap
		      when 'Yes, for 30 days' then 30
		      when 'Yes, for 15 days' then 15
		      when 'Yes, for 7 days' then 7
		      when 'Yes, for 3 days' then 3
		      when 'No' then 0
		    end as alcohol_reserves_and_handsoap_num,
		    case r.personal_disposable_masks
		      when 'Yes, for 30 days' then 30
		      when 'Yes, for 15 days' then 15
		      when 'Yes, for 7 days' then 7
		      when 'Yes, for 3 days' then 3
		      when 'No' then 0
		    end as personal_disposable_masks_num,
		    case r.personal_vinyl_gloves
		      when 'Yes, for 30 days' then 30
		      when 'Yes, for 15 days' then 15
		      when 'Yes, for 7 days' then 7
		      when 'Yes, for 3 days' then 3
		      when 'No' then 0
		    end as personal_vinyl_gloves_num,
		    case r.personal_disposable_hats
		      when 'Yes, for 30 days' then 30
		      when 'Yes, for 15 days' then 15
		      when 'Yes, for 7 days' then 7
		      when 'Yes, for 3 days' then 3
		      when 'No' then 0
		    end as personal_disposable_hats_num,
		    case r.personal_disposable_aprons
		      when 'Yes, for 30 days' then 30
		      when 'Yes, for 15 days' then 15
		      when 'Yes, for 7 days' then 7
		      when 'Yes, for 3 days' then 3
		      when 'No' then 0
		    end as personal_disposable_aprons_num,
		    case r.personal_visors
		      when 'Yes, for 30 days' then 30
		      when 'Yes, for 15 days' then 15
		      when 'Yes, for 7 days' then 7
		      when 'Yes, for 3 days' then 3
		      when 'No' then 0
		    end as personal_visors_num,
		    case r.personal_disposable_shoe_covers
		      when 'Yes, for 30 days' then 30
		      when 'Yes, for 15 days' then 15
		      when 'Yes, for 7 days' then 7
		      when 'Yes, for 3 days' then 3
		      when 'No' then 0
		    end as personal_disposable_shoe_covers_num,
		    r.num_test_kits,
		    r.respiratory_ventilator_machines,
		    r.reservas_id,
		    r.update_id
			from reservas r join update_ u on(r.reservas_id=u.update_id) join control_ c on(c.update_id=u.update_id)
			where c.update_status like '%completed%' 
	), health_levels as (
		select u.moph_number as hos_id, avg(reservas_num.oxygen_reserves_num) as avg_oxygen, avg(reservas_num.antipyretics_reserves_num) as avg_antipyretics, avg(reservas_num.anesthetics_and_muscular_relaxants_num) as avg_anesthetics, avg(reservas_num.alcohol_reserves_and_handsoap_num) as avg_alcohol, avg(reservas_num.personal_disposable_masks_num) as avg_masks, avg(reservas_num.personal_disposable_aprons_num) as avg_aprons, avg(reservas_num.personal_vinyl_gloves_num) as avg_gloves, avg(reservas_num.personal_disposable_hats_num) as avg_hats, avg(reservas_num.personal_visors_num) as avg_visors, avg(reservas_num.personal_disposable_shoe_covers_num) as avg_shoe_covers, avg(reservas_num.num_test_kits) as avg_test, avg(reservas_num.respiratory_ventilator_machines) as avg_venti
		from update_ u join reservas_num  using (update_id) join control_ c using (update_id)
		where c.update_status like '%completed%' and c.problem like '%none%' and u.update_date <= current_date and u.update_date > (current_date - '1 month'::interval)
		group by u.moph_number
		order by u.moph_number
	)
	select h.moph_number, h.hospital_name, h.district, h.province, health_levels.avg_oxygen, health_levels.avg_antipyretics, health_levels.avg_anesthetics, health_levels.avg_alcohol, health_levels.avg_masks, health_levels.avg_aprons, health_levels.avg_gloves, health_levels.avg_hats, health_levels.avg_visors,health_levels.avg_shoe_covers, health_levels.avg_test,  health_levels.avg_venti
	from hospital h join health_levels  on (health_levels.hos_id = h.moph_number)
	order by h.moph_number
)
```

#### Diagrama-ER

diagrama con db.diagram.io
https://dbdiagram.io/embed/6098481db29a09603d141499

En dbeaver

![ER_voxmapp_dbeaver](https://user-images.githubusercontent.com/77375206/117859721-fdcd0180-b254-11eb-8251-1de45397df5d.PNG)

## Set up

Para poder tener acceso completo al proyecto y poder utilizar de todas sus muy utiles herramientas se requiere installar un par de cosas y conectar el google sheets a la base de datos, para lo que tenemos que conectar el google sheets a pyhton y de python a la base de datos, a continuacion estara el link para accesar a el manual del usuario:

https://docs.google.com/document/d/1f8hk7zHd1ZKWIZ-X9rfSQJVDa396f0n6/edit

## Uso

## Estatus del Proyecto

El proyecto actualmentse esta acabado, sin embargo, siempre estamos arreglando errores y buscando posibles mejoras por lo que en caso de tener cualquier comentarios sobre el proyecto no dudar en contactarme @ZihiFredo

## Mejoras en el futuro

La mejora principal que tiene el projecto es que una vez que tenemos las posibilidades de crear graficas y vistas que contienen informacion que es pertinente para el cliente, queremos poder mostrarlas de la mejor manera posible, por lo que estamos buscando poder conectar la base de datos con Tableau que nos permite mostrar las graficas y las vistas de manera que es mas facil extraer la informacion que se encuentra en la base de datos ya que organiza los graficas en un mismo lugar y permite su acceso din tener que entrar directo a la base de datos

### Link de la encuesta

https://docs.google.com/forms/d/1NnXM4PWHAKxzAtIaQ5KYfcVVXQNEB6snOr99RaMZijE/edit
