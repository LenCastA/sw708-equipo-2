| Paquete | Clases | Responsabilidad | Depende de |
|---------|--------|----------------|------------|
| model | BaseEntity, NamedEntity, Person | Define atributos comunes para las entidades del sistema | Ninguno |
| owner | Owner, OwnerController, OwnerRepository, Pet, PetController, PetType, PetTypeFormatter, PetTypeRepository, PetValidator, Visit, VisitController | Gestiona propietarios, mascotas y sus visitas | model |
| vet | Specialty, Vet, VetController, VetRepository, Vets | Gestiona la información de los veterinarios y sus especialidades | model |
| system | CacheConfiguration, CrashController, WebConfiguration, WelcomeController | Configura aspectos generales y funciones básicas de la aplicación | Ninguno |
| (raíz) | PetClinicApplication, PetClinicRuntimeHints | Inicia la aplicación y configura los recursos necesarios para su ejecución | model, vet |
