# Bitácora de IA

## Prompt

> Explícame cómo está organizado el código de Spring PetClinic: paquetes, responsabilidades y cómo se atiende la petición GET /owners/1.

## Resumen de la respuesta

La IA explicó que el proyecto está organizado principalmente por funcionalidades, usando paquetes como `owner`, `vet`, `system` y `model`, en lugar de separar todo por capas.

También mencionó que no existe una capa de servicios y que los controladores trabajan directamente con los repositorios.

Para la petición `GET /owners/1`, indicó que primero se recibe la petición en `OwnerController`, luego se busca al dueño mediante `OwnerRepository`, se obtienen los datos de la base de datos y finalmente se envían a la plantilla `ownerDetails.html` para generar la página.

## Verificación de afirmaciones

| Afirmación de la IA | Estado | Verificación |
|---|---|---|
| El proyecto tiene los paquetes `owner`, `vet`, `system` y `model` | Cierta | Estos paquetes existen dentro de `org.springframework.samples.petclinic`. |
| El proyecto está organizado principalmente por funcionalidades y no únicamente por capas | Cierta | En un mismo paquete, como `owner`, se encuentran entidades, controladores, repositorios y clases de apoyo. |
| No existe una capa o paquete `service` | Cierta | En la estructura revisada no existe un paquete `service` y los controladores utilizan directamente los repositorios. |
| `Owner`, `Pet`, `Visit` y `Vet` son entidades | Cierta | Estas clases tienen la anotación `@Entity`. |
| Todos los repositorios extienden `JpaRepository<T, ID>` | Falsa | `OwnerRepository` y `PetTypeRepository` sí extienden `JpaRepository`, pero `VetRepository` extiende `Repository<Vet, Integer>`. |
| `OwnerController.showOwner()` atiende la petición `GET /owners/{ownerId}` | Cierta | El método tiene la anotación `@GetMapping("/owners/{ownerId}")`. |
| El valor `1` de `/owners/1` se recibe como `ownerId` | Cierta | El método recibe `@PathVariable("ownerId") int ownerId`. |
| `OwnerController` usa `OwnerRepository.findById()` para buscar al dueño | Cierta | Dentro de `showOwner()` se llama a `this.owners.findById(ownerId)`. |
| Spring Data genera la implementación del repositorio en tiempo de ejecución | Cierta | `OwnerRepository` es una interfaz y el método `findById()` no tiene una implementación escrita en el proyecto. |
| Hibernate ejecuta exactamente `SELECT * FROM owners WHERE id = ?` | No verificable | Ese SQL no aparece escrito en el código revisado, ya que la consulta es generada en tiempo de ejecución. |
| La consulta pasa por HikariCP antes de llegar a la base de datos | No verificable | En los archivos que revisamos no se encontró directamente esa parte del flujo. |
| `Owner` está relacionado con la tabla `owners` | Cierta | La clase tiene `@Table(name = "owners")` y esa tabla también aparece definida en `db/h2/schema.sql`. |
| Las mascotas de un `Owner` se cargan con `FetchType.EAGER` | Cierta | La relación `pets` de `Owner` tiene `fetch = FetchType.EAGER`. |
| El controlador envía el objeto `owner` a la plantilla | Cierta | En `showOwner()` se utiliza `mav.addObject(owner)`. |
| La vista utilizada es `owners/ownerDetails` | Cierta | El controlador crea un `ModelAndView` con `"owners/ownerDetails"`. |
| `ownerDetails.html` utiliza directamente los datos del objeto `owner` | Cierta | La plantilla utiliza `th:object="${owner}"` y accede a datos como nombre, dirección, ciudad y teléfono. |

## Error encontrado

La IA afirmó que los repositorios del proyecto extienden `JpaRepository<T, ID>`.

Al revisar el código se encontró que esto no se cumple en todos los casos. `OwnerRepository` y `PetTypeRepository` sí extienden `JpaRepository`, pero `VetRepository` está declarado de la siguiente forma:

`VetRepository extends Repository<Vet, Integer>`

Por lo tanto, la IA generalizó el funcionamiento de algunos repositorios y lo presentó como si todos estuvieran implementados de la misma manera.

También mencionó detalles como el SQL exacto generado por Hibernate y el uso de HikariCP. Aunque pueden formar parte del funcionamiento interno de Spring, con el código que revisamos en esta actividad no se puede comprobar directamente que el recorrido ocurra exactamente de esa manera, por lo que esas afirmaciones se dejaron como no verificables.