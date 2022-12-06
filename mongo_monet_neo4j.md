# Cheat Sheat 
## MongoDB

```
#!/bin/bash
cd documents/itam/7semestre/BASESNORELACIONALES
docker stop mongo 
docker rm mongo 
export DATA_DIR=`pwd`/data
echo $DATA_DIR
export EX_DIR=`pwd`/mongodb-sample-dataset
echo $EX_DIR
mkdir -p $DATA_DIR
docker run -p 27017:27017 \
       -v $DATA_DIR:/data/db \
       -v $EX_DIR:/mongodb-sample-dataset \
       --name mongo \
       -d mongo
export DHC=$DHC
export DHC=$(docker ps -aqf "name=mongo")
```
**Docker:**
```
docker exec -it $DHC mongosh
```
**Importar:**
```
cat safetycars.json| docker exec -i $DHC mongoimport --db=F1 --collection=safetycars
```
**Exportar:**
```
docker exec -i $DHC mongoexport --db=F1 --collection=safetycars --type=csv --out=safetycarsMonet.csv  --fields=Race,Cause,Deployed,Retreated,FullLaps,Circuit,CauseType,CircuitType
```

**Sacar archivos del docker:**
```
docker exec -i $DHC  cat safetycarsMonet.csv>>safetycarsMonetA.csv
```

## MonetDB
```
cd documents/itam/7semestre/BASESNORELACIONALES
docker volume create monet-data
docker stop monetdb
docker rm monetdb
docker run \
       -v monet-data:/var/monetdb5/dbfarm \
       -p 50001:50000 \
       --name monetdb \
       -d monetdb/monetdb:latest
```
**Docker:**
```
docker exec -it monetdb /bin/bash
```
**Crear base de datos:**
```
monetdb create -p monetdb safetycars
```
**Entrar a la base de datos: Password: monetdb**
```
mclient -u monetdb -d safetycars

CREATE USER "safetycars" WITH PASSWORD 'safetycars' NAME 'safetycars' SCHEMA "sys";
CREATE SCHEMA "safetycars" AUTHORIZATION "safetycars";
ALTER USER "safetycars" SET SCHEMA "safetycars";
```
Nos salimos de la base de datos 
Reconectamos con el nuevo user: Password: safetycars
```
mclient -u safetycars  -d safetycars
```

**Creamos la tabla:**
```
create table safetycars (
Race VARCHAR (100),
Cause VARCHAR (100),
Deployed int,
Retreated int, 
FullLaps int, 
Circuit VARCHAR (100),
CauseType VARCHAR (5),
CircuitType int
); 
```
Nos salimos a la terminal de Bash 

**Meter archivo al docker:**
```
docker cp safetycarsMonetA.csv monetdb:/
```
Nos metemos a monet otra vez 
```
docker exec -it monetdb /bin/bash
mclient -u safetycars  -d safetycars
```
**Importar:**
```
copy offset 2 into safetycars from '/safetycarsMonetA.csv' on client using delimiters ',',E'\n',E'\"' null as ' ';
```
## Neo4j

**Importar:**
```
LOAD CSV WITH HEADERS FROM "https://raw.githubusercontent.com/Skalas/nosql2022/main/datasets/products.csv" AS row
```
**Crear nodos:**
```
CREATE (n:Product)
SET n = row,
n.unitPrice = toFloat(row.unitPrice),
n.unitsInStock = toInteger(row.unitsInStock), n.unitsOnOrder = toInteger(row.unitsOnOrder),
n.reorderLevel = toInteger(row.reorderLevel), n.discontinued = (row.discontinued <> "0")

create (n:superhero {name:"Shazam",team: "JusticeLeague"})
```
**Crear edges:**
```
MATCH (p:Product),(c:Category)
WHERE p.categoryID = c.categoryID
CREATE (p)-[:PART_OF]->(c)

match (n:superhero),(v:villain) where n.name="KidFlash" and v.name="Zoom" create (n)-[:Enemies]-> (v)
```
