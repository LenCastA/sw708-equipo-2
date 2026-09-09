# Mapa de Componentes

| Paquete | Clases | Responsabilidad | Depende de |
|---------|--------|----------------|------------|
| model | BaseEntity, NamedEntity, Person | Define atributos comunes para las entidades del sistema | Ninguno |
| owner | Owner, OwnerController, OwnerRepository, Pet, PetController, PetType, PetTypeFormatter, PetTypeRepository, PetValidator, Visit, VisitController | Gestiona propietarios, mascotas y sus visitas | model |
| vet | Specialty, Vet, VetController, VetRepository, Vets | Gestiona la información de los veterinarios y sus especialidades | model |
| system | CacheConfiguration, CrashController, WebConfiguration, WelcomeController | Configura aspectos generales y funciones básicas de la aplicación | Ninguno |
| (raíz) | PetClinicApplication, PetClinicRuntimeHints | Inicia la aplicación y configura los recursos necesarios para su ejecución | model, vet |

# Flujo: ficha de un dueño

1. En el navegador se solicita `GET /owners/1`.
2. `OwnerController.java` - `showOwner()` recibe el `ownerId` y pide la información del dueño.
3. `OwnerRepository.java` - `findById()` busca al dueño por su id. Spring Data JPA se encarga de implementar esa búsqueda.
4. `Owner.java` - representa al dueño y se relaciona con la tabla `owners`.
5. `db/h2/schema.sql` - aquí está definida la tabla `owners`, donde se guardan los datos del dueño.
6. `OwnerController.java` - `showOwner()` agrega el `owner` al modelo y envía la vista `owners/ownerDetails`.
7. `templates/owners/ownerDetails.html` - recibe los datos del `owner` y los muestra en la página.