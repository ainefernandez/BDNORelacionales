# Cheat Sheat 
## MongoDB

```
#!/bin/bash
docker stop mongo 
docker rm mongo 
cd documents/itam/7semestre/BASESNORELACIONALES
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

## MonetDB
```
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
**Importar:**
```
docker exec -i monetdb  mclient -d voc - <  voc_dump.sql

cat voc_dump.sql | docker exec -i monetdb  mclient -d voc -
```



