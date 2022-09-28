# Ejercicios find
**1. Escribe una función find() para mostrar todos los documentos de la colección de restaurantes.**
```
db.restaurants.find()
```
**2. Escribe una función find() para mostrar los campos restaurant_id, nombre, municipio y cocina para todos los documentos en el restaurante de la colección.**
```
db.restaurants.find({},{"restaurant_id":1,"name":1,"borough":1,"cuisine":1,"_id":0})
```
**Escribe una función find() para mostrar los campos restaurant_id, nombre, municipio y cocina, pero excluya el campo \_id para todos los documentos de la colección restaurant.
db.restaurants.find({},{"restaurant_id":1,"name":1,"borough":1,"address.zipcode":1,"_id":0})
db.restaurants.find({"borough":"Bronx"})
db.restaurants.find({"borough":"Bronx"}).limit(5).count()
db.restaurants.find({"borough":"Bronx"}).skip(5).limit(5)
db.restaurants.find({"grades.score":{$gt:90}})
db.restaurants.find({"grades":{$elemMatch:{"score": {$gt:80,$lt:100}}}})
db.restaurants.find({"address.coord.0": {$lt: -95.754168}})
db.restaurants.distinct("cuisine")
db.restaurants.find({"cuisine": {$ne: "American "},"address.coord.0": {$lt: -65.754168}, "grades.score":{$gt:70}})


