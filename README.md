# Programmer Simulator

Simulador 3D en primera persona sobre la rutina de un programador. El jugador está en su cuarto y debe sentarse en el computador para cumplir tickets de trabajo, acompañado de un bug que funciona como su mascota. El estilo se inspira en *Internet Cafe Simulator*, con un toque de humor.

Proyecto final de la materia de **Desarrollo de Videojuegos** en la **Universidad de las Américas (UDLA)**.
**Avance 1**: prototipo hecho en **Unity** con **Visual Scripting**.

---

## Enlaces

| Recurso | Enlace |
|---|---|
| Video de presentación | _Agregar link de YouTube_ |
| Jugar en el navegador | _Agregar link de Unity Play_ |

---

## Controles

| Tecla | Acción |
|---|---|
| `W` `A` `S` `D` | Moverse |
| `Shift` | Correr |
| Mouse | Mirar alrededor |
| `E` | Programar en el PC (cuando estás cerca) |
| `Esc` | Liberar el cursor |

---

## Objetivo

Acercarse al escritorio y presionar **E** para completar tickets de programación hasta llegar a **5/5**.

---

## Lo implementado en el Avance 1

### Escenario
- Cuarto 3D con piso, paredes, techo y muebles low poly: escritorio, PC, teclado, silla, cama, cafetera, lámpara, libros y planta.
- Colliders en paredes y muebles para que el jugador no los atraviese.
- Ventana con marco y vidrio transparente, con luz direccional que entra al cuarto y proyecta sombras.
- Materiales con colores cálidos para darle un ambiente acogedor.

### Movimiento en primera persona
- Character Controller controlado por Visual Scripting.
- Cámara con el mouse con límite vertical de ±80°.
- Caminar con WASD y correr con Shift.
- Gravedad propia para mantener al jugador en el piso.
- Movimiento normalizado en diagonal e independiente de los FPS gracias a `Delta Time`.

### Interacción con el PC
- Zona trigger frente al escritorio que detecta al jugador.
- Mensaje en pantalla **"Presiona E para programar"**.
- Contador de tickets en la interfaz con TextMeshPro.

### Bug mascota
- Prefab con animación de rebote hecha con Animator.
- Sigue al jugador por el piso y lo mira.

### Instanciación
- Spawner que genera bugs cada cierto tiempo con `Instantiate` y `Cooldown`.
- Los bugs se eliminan con `Destroy` para mantener el rendimiento.

---

## Visual Scripting

Cada objeto con lógica tiene un componente **Script Machine** con su propio grafo, guardado en `Assets/ScriptGraphs`.

| Grafo | Objeto | Función |
|---|---|---|
| `PlayerMovement` | Player | Bloquea el cursor, controla la cámara, la gravedad y el movimiento |
| `InteraccionPC` | ZonaPc | Detecta al jugador con triggers, muestra el mensaje y suma tickets con E |
| `BugPersigue` | Prefab Bug | Busca al jugador con `Find With Tag` y lo sigue con `Look At` y `Move Towards` |
| `SpawnerBugs` | SpawnerBugs | Instancia bugs cada 5 segundos con `Cooldown` e `Instantiate` |

### PlayerMovement
- **On Start**: bloquea y oculta el cursor.
- **On Update → Sequence** dividido en tres pasos:
  1. **Cámara**: `Mouse X` gira el cuerpo; `Mouse Y` gira la cámara con `Clamp`.
  2. **Gravedad**: `Is Grounded` mantiene al jugador en el piso; si está en el aire, acumula la gravedad.
  3. **Movimiento**: los ejes `Horizontal` y `Vertical` se combinan con `Get Right` y `Get Forward`, pasan por `Clamp Magnitude`, `Select` elige caminar o correr, y `Character Controller Move` aplica el movimiento.

### InteraccionPC
- `On Trigger Enter` / `On Trigger Exit` con `Compare Tag "Player"` cambian la variable `jugadorCerca` y muestran u ocultan el mensaje.
- En `On Update`, `jugadorCerca` **And** `Get Key Down E` suma un ticket y actualiza el texto con `String Format` y `TMP_Text Set Text`.

---

## Tecnologías

- **Unity 6.5** con Universal Render Pipeline (URP)
- **Visual Scripting**
- **TextMeshPro** para la interfaz
- [Kenney Furniture Kit](https://kenney.nl/assets/furniture-kit) para los muebles
- [Mixamo](https://www.mixamo.com) para el personaje y sus animaciones

---

## Estructura del proyecto

```
Assets/
├── Animations/      # Clips y Animator Controllers
├── Materials/       # Materiales del cuarto y del bug
├── Models/
│   ├── Furniture/   # Muebles de Kenney
│   └── Personaje/   # Personaje AJ de Mixamo
├── Prefabs/         # Player y Bug
├── Scenes/          # Escena Oficina
└── ScriptGraphs/    # Grafos de Visual Scripting
```

---

## Cómo abrir el proyecto

1. Clonar el repositorio:
```bash
   git clone https://github.com/Leandrop011/programmer-simulator.git
```
2. Abrirlo desde **Unity Hub** con **Unity 6.5**.
3. Abrir la escena `Assets/Scenes/Oficina`.
4. Presionar **Play**.

---

## Próximos avances

- Reloj de deadline como condición de derrota.
- Pantalla de victoria al completar 5/5 tickets.
- Personaje en tercera persona con animaciones y cambio de cámara con `V`.
- Más objetos instanciados, como tazas de café.
- Sonidos, música y menú principal.

---

## Autor

**Leandro Pozo** · [@Leandrop011](https://github.com/Leandrop011)
Ingeniería de Software, Universidad de las Américas
