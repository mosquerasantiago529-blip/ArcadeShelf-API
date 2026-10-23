Respuestas del parcial — ArcadeShelf API
1. ¿Qué error encontraste en la validación de stock?

El schema permitía un valor mínimo de 0. Lo cambié a Field(ge=1, le=50) para aceptar únicamente valores entre 1 y 50. Probé que stock: 0 devuelve 422.

2. ¿Qué hacía que un juego pareciera creado pero no quedara persistido?

En services/game_service.py faltaba ejecutar session.commit() antes de session.refresh(game). Añadí el commit para confirmar la transacción en SQLite antes de refrescar el objeto.

3. ¿Qué problema tenía el endpoint que lista juegos?

En routers/games.py, GET /games declaraba response_model=GameResponse, aunque el servicio devuelve una lista. Lo corregí a response_model=list[GameResponse].

4. ¿Cómo solucionaste la consulta de un ID inexistente?

En get_game(), comprobé si session.get() devuelve None. Si el videojuego no existe, ahora se lanza una HTTPException con estado 404 y el mensaje Game not found.

5. ¿Por qué DELETE podía responder correctamente y aun así no eliminar el registro de SQLite?

Porque session.delete(game) no confirmaba la transacción. Añadí session.commit() después de eliminar el objeto para guardar el cambio en SQLite.