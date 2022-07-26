# mongodb-springboot
El proyecto demuestra ejemplos básicos de CRUD usando MongoDB y SpringBoot

Para este [tutorial](https://www.mongodb.com/compatibility/spring-boot), necesitamos crear un proyecto Spring Boot, lo cual puede hacerse fácilmente usando Spring Initialzr. Es ventajoso utilizar un IDE (Entorno de Desarrollo Integrado) como Eclipse para este tutorial.

# Configuración del proyecto
Una vez que el proyecto está configurado, necesitamos crear una clase Java y campos que serán mapeados en la colección y campos de MongoDB.

Para la implementación de la API, utilizamos dos enfoques:
1. MongoRepository - Demostramos todas las operaciones CRUD utilizando este enfoque.
2. MongoTemplate - Se utiliza para tener más control sobre los filtros, y también es una buena opción para las agregaciones. Demostraremos la operación de actualización utilizando este enfoque.

# Ejemplos de CRUD con MongoRepository
### Para crear un documento (ítem) en MongoDB, utilice el método save():

  `groceryItemRepo.save(new GroceryItem("Dried Red Chilli", "Dried Whole Red Chilli", 2, "spices"));`.
  
### Para leer todos los documentos, utilice el método findAll():
  `groceryItemRepo.findAll().forEach(item-> item.getName(),item.getQuantity(),item.getCategory());`
  
### Para leer documentos en base a un campo concreto, como el nombre o la categoría, especificando los parámetros de la consulta:
  	@Query("{nombre:'?0'}")
	GroceryItem findItemByName(String name);
	
	@Query(value="{category:'?0'}", fields="{'name' : 1, 'quantity' : 1}")
	List<GroceryItem> findAll(String category);
  
### Para actualizar un documento usando MongoRepository, debemos recuperar los documentos específicos y guardar los nuevos valores. Por ejemplo, para actualizar una categoría, haz lo siguiente:
  	List<GroceryItem> list = groceryItemRepo.findAll(category);
		 
	list.forEach(item -> {
	   // Actualizar la categoría en cada documento
		 item.setCategory(newCategory);
	});
		 
	// Guardar todos los artículos en la base de datos
	List<GroceryItem> itemsUpdated = groceryItemRepo.saveAll(list);
  
 ### Para eliminar un documento, utilice el método delete. Por ejemplo, borre un artículo por su ID utilizando el método deleteById():
  `groceryItemRepo.deleteById(id);`
	
 # Actualización mediante MongoTemplate 
 Las actualizaciones utilizando MongoTemplate son mucho más fáciles con las clases out-of-box que proporciona MongoTemplate.
 Actualicemos la cantidad de un artículo en particular: 
  	
	Query query = new Query(Criteria.where("nombre").is(nombre));
   	Update = new Update();
   	update.set("cantidad", newQuantity);		
   	UpdateResult result = mongoTemplate.updateFirst(query, update, GroceryItem.class);