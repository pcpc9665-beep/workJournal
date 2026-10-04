# OpenAPI Specification (OAS)

## Introduction

**OpenAPI Specification (OAS)** എന്നത് ഒരു **Web API** എങ്ങനെ പ്രവർത്തിക്കുന്നു എന്ന് YAML അല്ലെങ്കിൽ JSON format-ൽ വിവരിക്കുന്ന standard ആണ്. API-യിലെ **endpoints, HTTP methods, request, response, authentication** തുടങ്ങിയവ വ്യക്തമായി രേഖപ്പെടുത്താൻ ഇത് ഉപയോഗിക്കുന്നു. OAS programming language-നോട് ബന്ധിപ്പിക്കപ്പെട്ടതല്ല; അതിനാൽ JavaScript, Python, Java, PHP തുടങ്ങിയ technologies ഉപയോഗിച്ചുള്ള APIs വിവരിക്കാം.[[openapis](https://www.openapis.org/what-is-openapi)]

മനസ്സിലാക്കേണ്ട prerequisites:

- HTTP basics
- REST API
- HTTP methods: `GET`, `POST`, `PUT`, `PATCH`, `DELETE`
- JSON format
- Basic frontend-backend communication

## OpenAPI Specification

### Definition & Core Usage

**OpenAPI Specification** എന്നത് ഒരു HTTP API-യുടെ **contract** അല്ലെങ്കിൽ blueprint ആണ്. API-യിൽ ലഭ്യമായ resources, endpoints, parameters, request/response formats, authentication methods എന്നിവ മനുഷ്യർക്കും machines-നും മനസ്സിലാകുന്ന രീതിയിൽ നിർവചിക്കുകയാണ് ഇതിന്റെ പ്രധാന purpose.[[swagger](https://swagger.io/specification/)]

പ്രധാന **core terminologies**:

- **API** — Applications തമ്മിൽ communication നടത്താനുള്ള interface.
- **Endpoint** — API-യുടെ ഒരു URL.
- **HTTP Method** — API-യിൽ ചെയ്യേണ്ട operation, ഉദാഹരണം `GET`, `POST`.
- **Request** — Client അയക്കുന്ന data.
- **Response** — Server തിരികെ നൽകുന്ന data.
- **Schema** — Data-യുടെ structure, type, validation rules.
- **Authentication** — User അല്ലെങ്കിൽ application-ന്റെ identity പരിശോധിക്കൽ.
- **Swagger UI** — OpenAPI document-നെ interactive documentation ആക്കി കാണിക്കുന്ന tool.
- **YAML/JSON** — OpenAPI document എഴുതാൻ ഉപയോഗിക്കുന്ന formats.

## Detailed Explanation

ഒരു web application-ൽ React frontend, Node.js/Express backend, database എന്നിവ ഉണ്ടെന്ന് കരുതുക. Frontend-ന് backend-ലേക്ക് ഏത് URL ഉപയോഗിക്കണം, ഏത് HTTP method വേണം, എന്ത് data അയയ്ക്കണം, എന്ത് response പ്രതീക്ഷിക്കണം എന്നിവ അറിയണം. ഈ വിവരങ്ങൾ ഓരോ developer-നും വേറിട്ട രീതിയിൽ പറയുന്നതിനുപകരം, OpenAPI document-ൽ ഒരു standardized contract ആയി രേഖപ്പെടുത്താം.

ഉദാഹരണത്തിന്:

```
GET /api/vehicles/101/location
```

ഈ endpoint ഒരു vehicle-ന്റെ current GPS location നൽകുന്നു എന്ന് OpenAPI document വ്യക്തമാക്കും.

OpenAPI document API-യെ **implement** ചെയ്യുന്നില്ല. അത് API-യെ **describe** ചെയ്യുകയാണ്. Backend code വേറെയും എഴുതണം; OpenAPI ആ code ഉപയോഗിക്കാനുള്ള നിയമങ്ങളും data structure-ഉം വിശദീകരിക്കുന്നു.

## Real-World Example

### Scenario

നിങ്ങൾ ഒരു **vehicle tracking web application** നിർമ്മിക്കുന്നു. അതിൽ:

- React frontend ഉണ്ട്.
- Node.js/Express backend ഉണ്ട്.
- GPS location database-ൽ സൂക്ഷിക്കുന്നു.
- Frontend map-ൽ vehicle-ന്റെ live location കാണിക്കണം.

Frontend developer-ന് താഴെപ്പറയുന്ന കാര്യങ്ങൾ അറിയണം:

```
Endpoint: GET /api/vehicles/{vehicleId}/location
Parameter: vehicleId
Response: latitude, longitude, speed
Error: vehicle not found
```

### Application

ഇത് OpenAPI-ൽ ഇങ്ങനെ രേഖപ്പെടുത്താം:

```
openapi: 3.0.3

info:
  title: Vehicle Tracking API
  version: 1.0.0

servers:
  - url: https://api.example.com

paths:
  /api/vehicles/{vehicleId}/location:
    get:
      summary: Get current vehicle location

      parameters:
        - name: vehicleId
          in: path
          required: true
          schema:
            type: integer
          example: 101

      responses:
        "200":
          description: Current vehicle location
          content:
            application/json:
              schema:
                type: object
                properties:
                  vehicleId:
                    type: integer
                    example: 101
                  latitude:
                    type: number
                    example: 10.7867
                  longitude:
                    type: number
                    example: 76.6548
                  speed:
                    type: number
                    example: 42.5

        "404":
          description: Vehicle not found
```

ഇത് അടിസ്ഥാനമാക്കി frontend developer API call എഴുതാം:

```
const response = await fetch(
  "https://api.example.com/api/vehicles/101/location"
);

const location = await response.json();

console.log(location.latitude);
console.log(location.longitude);
```

API return ചെയ്യുന്ന response:

```
{
  "vehicleId": 101,
  "latitude": 10.7867,
  "longitude": 76.6548,
  "speed": 42.5
}
```

ഇവിടെ OpenAPI സഹായിക്കുന്നത്:

- Correct endpoint കണ്ടെത്താൻ.
- Correct `HTTP method` ഉപയോഗിക്കാൻ.
- Required `path parameter` മനസ്സിലാക്കാൻ.
- Response fields അറിയാൻ.
- Error status code മനസ്സിലാക്കാൻ.
- Swagger UI വഴി API test ചെയ്യാൻ.

## Best Practices & Tips

### Visual Anchor / Key Tip

**OpenAPI = API-യുടെ blueprint അല്ലെങ്കിൽ contract.**

ഇത് ഓർക്കാൻ ഈ flow ഉപയോഗിക്കുക:

```
OpenAPI Document
       ↓
API Contract
       ↓
Swagger UI / Client Generation / Testing
       ↓
Frontend-Backend Integration
```

OpenAPI ഉപയോഗിച്ചാൽ source code കാണാതെ തന്നെ API-യുടെ capabilities മനസ്സിലാക്കാൻ കഴിയും.[[swagger](https://swagger.io/specification/)]

### Efficiency Trick

ആദ്യം ഓരോ endpoint-നും ഈ നാല് കാര്യങ്ങൾ എഴുതുക:

```
1. URL
2. HTTP Method
3. Request Data
4. Response Data
```

ഉദാഹരണം:

```
URL: GET /vehicles/{id}/location
Request: vehicle id
Response: latitude, longitude, speed
Error: 404 if vehicle is not found
```

ശേഷം മാത്രമേ detailed YAML എഴുതേണ്ടതുള്ളൂ.

### Common Pitfall to Avoid

**OpenAPI document ഉണ്ടാക്കിയാൽ API സ്വയം create ആകും എന്ന് കരുതരുത്.**

ഈ YAML:

```
GET /vehicles/{id}/location
```

സ്വയം database-ൽ നിന്ന് location എടുക്കില്ല. Backend-ൽ route implementation വേണം:

```
app.get("/vehicles/:id/location", async (req, res) => {
  const location = await findVehicleLocation(req.params.id);
  res.json(location);
});
```

മറ്റൊരു common mistake:

```
Backend response: latitude, longitude
OpenAPI response: lat, lng
```

Documentation-ലും actual backend response-ലും ഒരേ field names ഉപയോഗിക്കുക.

## Check for Understanding

താഴെയുള്ള API-ക്കായി OpenAPI-ൽ എന്തൊക്കെ document ചെയ്യണം?

```
POST /api/products
```

Request body:

```
{
  "name": "Wireless Mouse",
  "price": 799
}
```

ചിന്തിക്കേണ്ട കാര്യങ്ങൾ:

1. ഏത് **HTTP method** ആണ്?
2. Request body-യുടെ **schema** എന്താണ്?
3. `name`, `price` എന്നിവ required ആണോ?
4. Success status code എന്തായിരിക്കണം?
5. Product already exists അല്ലെങ്കിൽ invalid data ആണെങ്കിൽ ഏത് error response നൽകും?

ഒരു basic answer:

```
Method: POST
Endpoint: /api/products
Request body: name and price
Success response: 201 Created
Error response: 400 Bad Request
```

## Conclusion

OpenAPI Specification ഒരു web application-ിലെ API-യുടെ **clear, standardized documentation and contract** ആണ്. React frontend-നും Node.js backend-നും ഇടയിൽ എന്ത് data കൈമാറണം എന്ന് വ്യക്തമായി നിർവചിക്കാൻ ഇത് സഹായിക്കുന്നു. OpenAPI ഉപയോഗിച്ച് നിങ്ങൾക്ക് **Swagger UI documentation, API testing, client code generation, request validation** എന്നിവ നടത്താം; Swagger സാധാരണയായി OpenAPI-യുമായി പ്രവർത്തിക്കുന്ന toolset ആണ്.[[developer.salesforce](https://developer.salesforce.com/blogs/2023/06/design-a-swagger-api-with-code-to-bring-data-into-salesforce)]

അടുത്ത logical topic: **Express.js API-ക്കായി OpenAPI document എഴുതുകയും Swagger UI-ൽ പ്രദർശിപ്പിക്കുകയും ചെയ്യുന്നത്**.