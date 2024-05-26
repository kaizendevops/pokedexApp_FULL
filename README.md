# Pokémon Angular Application

Este proyecto es una aplicación Angular que consume la API de Pokémon para listar y mostrar detalles de los Pokémon. La aplicación está diseñada para ilustrar conceptos de DevOps, GitFlow y pruebas unitarias.

## Historias de Usuario

### Historia de Usuario 1: Listar Pokémon

**Descripción:**
Como usuario, quiero ver una lista de Pokémon en la aplicación para poder explorar los diferentes Pokémon disponibles.

**Criterios de Aceptación:**
1. La lista de Pokémon debe mostrarse en una página dedicada.
2. Cada Pokémon debe mostrar su nombre y una imagen.
3. La lista debe obtenerse desde la API de Pokémon (`https://pokeapi.co/api/v2/pokemon`).

**Tareas:**
1. Crear un servicio en Angular para consumir la API de Pokémon.
2. Crear un componente para mostrar la lista de Pokémon.
3. Implementar pruebas unitarias para el servicio y el componente.
4. Crear una rama `feature/list-pokemon` usando GitFlow.
5. Abrir un pull request y realizar revisiones de código antes de fusionar la rama a `develop`.

### Historia de Usuario 2: Mostrar Detalles de un Pokémon

**Descripción:**
Como usuario, quiero ver los detalles de un Pokémon específico para conocer más sobre sus habilidades, estadísticas y otros atributos.

**Criterios de Aceptación:**
1. Al hacer clic en un Pokémon de la lista, debe navegar a una página de detalles.
2. La página de detalles debe mostrar el nombre, imagen, habilidades, estadísticas y otros atributos del Pokémon.
3. Los detalles deben obtenerse desde la API de Pokémon (`https://pokeapi.co/api/v2/pokemon/{id or name}`).

**Tareas:**
1. Crear un componente para mostrar los detalles del Pokémon.
2. Modificar el servicio para obtener los detalles del Pokémon.
3. Implementar pruebas unitarias para el nuevo componente y el método de servicio.
4. Crear una rama `feature/pokemon-details` usando GitFlow.
5. Abrir un pull request y realizar revisiones de código antes de fusionar la rama a `develop`.

## Uso de GitFlow

1. **Ramas:**
   - **`develop`**: Rama de desarrollo donde se integran las nuevas funcionalidades.
   - **`feature/*`**: Ramas de características que contienen nuevas funcionalidades o mejoras.
   - **`main`**: Rama principal que contiene el código en producción.

2. **Flujo de Trabajo:**
   - Crear una rama `feature` para cada nueva funcionalidad o mejora.
   - Trabajar en la rama `feature` y realizar commits.
   - Abrir un pull request de la rama `feature` a `develop`.
   - Realizar revisiones de código y fusionar la rama `feature` a `develop`.
   - Preparar la rama `develop` para un nuevo lanzamiento y fusionarla a `main`.

## Implementación de Pruebas Unitarias

### Servicio `PokemonService`

Crear pruebas unitarias para asegurar que los métodos `getPokemons` y `getPokemonDetails` funcionen correctamente.

Ejemplo de prueba para `getPokemons`:
```typescript
import { TestBed } from '@angular/core/testing';
import { HttpClientTestingModule, HttpTestingController } from '@angular/common/http/testing';
import { PokemonService } from './pokemon.service';

describe('PokemonService', () => {
  let service: PokemonService;
  let httpMock: HttpTestingController;

  beforeEach(() => {
    TestBed.configureTestingModule({
      imports: [HttpClientTestingModule],
      providers: [PokemonService]
    });
    service = TestBed.inject(PokemonService);
    httpMock = TestBed.inject(HttpTestingController);
  });

  it('should retrieve pokemons from the API via GET', () => {
    const dummyPokemons = { results: [{ name: 'bulbasaur' }, { name: 'ivysaur' }] };

    service.getPokemons().subscribe(pokemons => {
      expect(pokemons.results.length).toBe(2);
      expect(pokemons.results).toEqual(dummyPokemons.results);
    });

    const request = httpMock.expectOne(`${service['apiUrl']}?limit=20&offset=0`);
    expect(request.request.method).toBe('GET');
    request.flush(dummyPokemons);
  });

  afterEach(() => {
    httpMock.verify();
  });
});
