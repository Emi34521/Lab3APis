# API Onboarding Report - CheapShark API

## Resumen de la API

| Campo | Detalle |
|---|---|
| **Nombre** | CheapShark API |
| **Base URL** | `https://www.cheapshark.com/api/1.0` |
| **Tipo de Auth** | Ninguna (API pública, no requiere token ni API key) |
| **Rate Limit** | No documentado oficialmente |
| **Formato de respuesta** | JSON |
| **Descripción** | API pública que permite consultar ofertas y precios de videojuegos digitales en tiendas como Steam, GOG, Epic Games, entre otras. |

---

## Tabla de Endpoints

| Método | URL | Query Params | Headers | Status Esperado | Status Obtenido |
|---|---|---|---|---|---|
| GET | `/deals` | - | `Accept: application/json` | 200 | 200  |
| GET | `/deals/{id}` | - | `Accept: application/json` | 200 | 500  |
| GET | `/deals` | `title=batman` | `Accept: application/json` | 200 | 200  |
| GET | `/deals` | `upperPrice=15&storeID=1` | `Accept: application/json` | 200 | 200  |
| GET | `/deals` | `pageNumber=1&pageSize=10` | `Accept: application/json` | 200 | 200  |
| GET | `/stores` | - | `Accept: application/json` | 200 | 200  |
| GET | `/recursoinventado` | - | `Accept: application/json` | 404 | 404  |
| GET | `/deals` | `upperPrice=abc` | `Accept: application/json` | 400 | 200  |

> **Nota 400:** La API maneja parámetros inválidos silenciosamente, retornando `[]` en lugar de un error 400. Este comportamiento indica que la API tiene validaciones internas robustas.

> **Nota 401/403:** La API no requiere autenticación, por lo que estos errores no aplican.

> **Nota 500:** El endpoint de detalle por ID retornó un error 500 del servidor para el ID utilizado, lo que indica un problema interno de la API con ese recurso específico.

---

## Evidencia de Respuestas

### Respuesta Exitosa 1 - Listar Deals
**Request:** `GET https://www.cheapshark.com/api/1.0/deals`
**Status:** 200 OK

```json
[
  {
    "internalName": "STARTREKVOYAGERACROSSTHEUNKNOWN",
    "title": "Star Trek: Voyager - Across the Unknown",
    "dealID": "znV0gTCyWvLN8L2sP3OJ%2Bat0Vudm7h%2FQQp3ysER8S%2Bg%3D",
    "storeID": "23",
    "gameID": "316380",
    "salePrice": "25.79",
    "normalPrice": "34.99",
    "isOnSale": "1",
    "savings": "26.293227",
    "metacriticScore": "77",
    "steamRatingText": "Mostly Positive",
    "dealRating": "10.0"
  },
  {
    "internalName": "SUICIDESQUADKILLTHEJUSTICELEAGUE",
    "title": "Suicide Squad: Kill the Justice League",
    "dealID": "nF9%2FdPXYPajMwowT9vhxtb%2BbzqK%2FsuFSBFt5snmLx24%3D",
    "storeID": "30",
    "gameID": "273928",
    "salePrice": "1.39",
    "normalPrice": "69.99",
    "isOnSale": "1",
    "savings": "98.014002",
    "metacriticScore": "63",
    "steamRatingText": "Mixed",
    "dealRating": "10.0"
  }
]
```

---

### Respuesta Exitosa 2 - Búsqueda por título (batman)
**Request:** `GET https://www.cheapshark.com/api/1.0/deals?title=batman`
**Status:** 200 OK

```json
[
  {
    "internalName": "LEGOBATMAN",
    "title": "LEGO Batman",
    "dealID": "r5pU2qL0yMcQTXWlvVyaxVOuBAjgtKdD81zA65nVJak%3D",
    "storeID": "30",
    "gameID": "612",
    "salePrice": "2.29",
    "normalPrice": "19.99",
    "isOnSale": "1",
    "savings": "88.544272",
    "metacriticScore": "80",
    "steamRatingText": "Very Positive",
    "dealRating": "8.9"
  }
]
```

---

### Respuesta Exitosa 3 - Listar Tiendas
**Request:** `GET https://www.cheapshark.com/api/1.0/stores`
**Status:** 200 OK

```json
[
  {
    "storeID": "1",
    "storeName": "Steam",
    "isActive": 1,
    "images": {
      "banner": "/img/stores/banners/0.png",
      "logo": "/img/stores/logos/0.png",
      "icon": "/img/stores/icons/0.png"
    }
  }
]
```

---

### Respuesta Fallida 1 - 404 Not Found
**Request:** `GET https://www.cheapshark.com/api/1.0/recursoinventado`
**Status:** 404 Not Found

La API retornó una página HTML con el mensaje "404 Page Not Found", indicando que el recurso solicitado no existe en el servidor.

```
404 Page Not Found
Oops, looks like this page is missing!
```

---

### Respuesta Fallida 2 - 500 Internal Server Error
**Request:** `GET https://www.cheapshark.com/api/1.0/deals/1`
**Status:** 500 Internal Server Error

El servidor encontró un error interno al intentar recuperar el detalle del deal con ID 1. Este error es del lado del servidor y no del cliente, lo que sugiere un problema con ese recurso específico en la API.

---

## Observaciones Generales

- La API es pública y no requiere ningún tipo de autenticación, lo que la hace ideal para proyectos de aprendizaje.
- La API es robusta en el manejo de parámetros inválidos, prefiriendo retornar respuestas vacías `[]` antes que errores 400.
- No se encontró documentación oficial sobre rate limits, aunque se recomienda no hacer solicitudes masivas para evitar bloqueos.
