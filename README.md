# WhiteBird Registration API

This API is used for merchant backend-to-backend registration and KYC-related actions.
Use these endpoints to register clients, update legal agreements, and generate SDK tokens without exposing merchant secrets in frontend code.

> BASE_URL https://api.dev.wbdevel.net

## 1) Full KYC Registration

### Step 1. Register client with full KYC data
Use this endpoint to register a client with full PID/KYC profile data from the merchant side.
Use the response `id` as the WhiteBird `clientId` for status checks, crypto test flow, and token generation.

**POST** `/api/v2/kyc/merchant/client/register`

**Headers**
- `x-api-key: {{x-api-key}}`

`ClientManagementService.registerClient()` updates CRM as processed in this flow only when all three flags are `true`:
- `agreedWithOffer`
- `notUSTaxPayer`
- `exchangeInPersonalInterests`

**Request**
```json
{
  "email": "test.user.testov+29042026b@ya.ru",
  "phone": "+375298814561",
  "firstNameRu": "Кирилл",
  "lastNameRu": "Данилов",
  "patronymicRu": "Леонидович",
  "firstName": "Kirill",
  "lastName": "Danilov",
  "residence": "112",
  "placeOfBirth": "МТЭБЫТ ЖФЖМСЗХ ЦТПИЫ ЕЗАОКЙ ФГЗЕНЖГО",
  "birthDate": "1994-11-02",
  "registrationCountry": "112",
  "registrationRegion": "ЧРПЮЩАЦПХКР",
  "residenceDistrict": "РРМИОЕБАЙБЙЩЦ",
  "registrationCity": "НРГЬЮБМФЦ",
  "registrationStreet": "ЫФЛЦЕНФЖЩЛ",
  "registrationHouseAndFlat": "ШЩБЦЙГЁЁЦЭХД",
  "identityDocType": "3",
  "identityDocIssueDate": "2021-04-18",
  "identityDocExpireDate": "2028-12-23",
  "identityDocNumber": "SC4861515",
  "identityDocIssuer": "ИЛГРГКЩОЗЕ",
  "personalNumber": "6685261B729PB7",
  "postCode": "579477",
  "gender": "муж",
  "nationality": "112",
  "notUSTaxPayer": true,
  "exchangeInPersonalInterests": true,
  "agreedWithOffer": true,
  "externalClientId": "partner-client-123",
  "isPotentialDrop": false
}
```

**Response**
```json
{
  "id": "0d58e7ec-0369-48d7-9804-90c6b23a52be",
  "status": "PENDING"
}
```

### Headers

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| `x-api-key` | `string` | Yes | Authenticates merchant backend request for KYC registration endpoints. |

### Request

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| `email` | `string` | Yes | Client email (`@NotEmpty`). |
| `phone` | `string` | Yes | Client phone (`@NotEmpty`). |
| `firstNameRu` | `string` | Yes | First name in Cyrillic (`@NotEmpty`). |
| `lastNameRu` | `string` | Yes | Last name in Cyrillic (`@NotEmpty`). |
| `patronymicRu` | `string` | Yes | Patronymic in Cyrillic (`@NotEmpty`). |
| `firstName` | `string` | Yes | First name in Latin (`@NotEmpty`). |
| `lastName` | `string` | Yes | Last name in Latin (`@NotEmpty`). |
| `residence` | `string` | Yes | Residence country code (`@NotEmpty`). |
| `placeOfBirth` | `string` | Yes | Place of birth (`@NotEmpty`). |
| `birthDate` | `string` | No | Birth date in `YYYY-MM-DD` (mapped to `LocalDate`). |
| `registrationCountry` | `string` | Yes | Registration country code (`@NotEmpty`). |
| `registrationRegion` | `string` | Yes | Registration region/state (`@NotEmpty`). |
| `residenceDistrict` | `string` | Yes | Registration district field (`@NotEmpty`). |
| `registrationCity` | `string` | Yes | Registration city/locality (`@NotEmpty`). |
| `registrationStreet` | `string` | Yes | Registration street (`@NotEmpty`). |
| `registrationHouseAndFlat` | `string` | Yes | House/flat part of registration address (`@NotEmpty`). |
| `identityDocType` | `string` | Yes | Identity document type (`@NotEmpty`). |
| `identityDocIssueDate` | `string` | No | Document issue date string. |
| `identityDocExpireDate` | `string` | No | Document expiry date string. |
| `identityDocNumber` | `string` | Yes | Identity document number (`@NotEmpty`). |
| `identityDocIssuer` | `string` | Yes | Identity document issuer (`@NotEmpty`). |
| `personalNumber` | `string` | Conditional | Required when `registrationCountry` contains `112` (Belarus). |
| `postCode` | `string` | Yes | Postal code (`@NotEmpty`). |
| `gender` | `string` | Yes | Gender value (`@NotEmpty`), e.g. `муж`, `жен`. |
| `nationality` | `string` | Yes | Nationality country code (`@NotEmpty`). |
| `notUSTaxPayer` | `boolean` | No | Consent flag for non-US taxpayer declaration. |
| `exchangeInPersonalInterests` | `boolean` | No | Consent flag for personal-interest exchange declaration. |
| `agreedWithOffer` | `boolean` | No | Consent flag for WhiteBird public offer. |
| `files` | `array of string` | No | Optional list of file references stored with client KYC data. |
| `externalClientId` | `string` | No | Merchant-side external client identifier. |
| `isPotentialDrop` | `boolean` | No | Optional risk marker propagated to CRM processing. |

### Response

| Name | Type | Description |
| --- | --- | --- |
| `id` | `string` | WhiteBird client id created/linked during registration. |
| `status` | `string` | Client status after registration. Allowed values: `NOT_VERIFIED`, `PENDING`, `VERIFIED`, `FROZEN`, `ARREST`. |

### Errors

| Name | Code | Description |
| --- | --- | --- |
| `400 ValidationException` | BUSINESS | One or more required fields are missing/empty, or validation rules fail. |
| `400 ValidationException` | BUSINESS | `personalNumber` is missing for Belarus registration country (`112`). |
| `401 Unauthorized` | HTTP | `x-api-key` is missing, invalid, or expired. |
| `403 Forbidden` | HTTP | Merchant has no permission for this endpoint (`KYC_REGISTER_EP`). |

## 2) Client Status

### Step 2. Check registered client status
Use this endpoint to fetch current KYC status for a registered client.
Use the response to decide whether client can proceed to operations or must complete additional verification.

**POST** `/api/v2/kyc/merchant/client/status`

**Headers**
- `x-api-key: {{x-api-key}}`

**Request**
```json
{
  "clientId": "0d58e7ec-0369-48d7-9804-90c6b23a52be"
}
```

**Response**
```json
"VERIFIED"
```

### Headers

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| `x-api-key` | `string` | Yes | Authenticates merchant backend request for status checks. |

### Params

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| `externalUserId` | `string` | No | Merchant-side external id used as optional additional client validation key. |

### Request

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| `clientId` | `string` | Yes | WhiteBird client id returned by registration. |

### Response

| Name | Type | Description |
| --- | --- | --- |
| `status` | `string` | KYC status string from backend. Possible values include `NOT_VERIFIED`, `PENDING`, `VERIFIED`, `FROZEN`, `ARREST`. |

### Errors

| Name | Code | Description |
| --- | --- | --- |
| `401 Unauthorized` | HTTP | `x-api-key` is missing, invalid, or expired. |
| `403 Forbidden` | HTTP | Merchant has no permission for this endpoint (`KYC_GET_CLIENT_STATUS_EP`). |

## 3) Crypto Test

### Step 3. Get crypto-test requirements/questions
Use this endpoint to determine whether the client must pass crypto test and to fetch questions for residents where test is required.
Use the response to render the test UI or skip test step if `cryptoTestRequired=false`.

**GET** `/api/v2/kyc/merchant/client/crypto-test?clientId={{clientId}}`

**Headers**
- `x-api-key: {{x-api-key}}`
- `externalClientId: {{externalClientId}}` (optional)

**Response**
```json
{
  "cryptoTestRequired": true,
  "questions": [
    {
      "id": "1",
      "title": "Question text",
      "answers": [
        {
          "id": 10,
          "title": "Answer text",
          "correct": true
        }
      ]
    }
  ]
}
```

### Headers

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| `x-api-key` | `string` | Yes | Authenticates merchant backend request for crypto test endpoints. |
| `externalClientId` | `string` | No | Merchant-side external client id for additional client lookup. |

### Params

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| `clientId` | `string` | Yes | WhiteBird client id. |

### Request

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| `body` | `none` | - | GET endpoint without request body. |

### Response

| Name | Type | Description |
| --- | --- | --- |
| `cryptoTestRequired` | `boolean` | Indicates whether client must pass crypto test. |
| `questions` | `array of objects` | Test questions list, returned when `cryptoTestRequired=true`. |
| `questions[].id` | `string` | Question identifier. |
| `questions[].title` | `string` | Question text. |
| `questions[].answers` | `array of objects` | Answer options list. |
| `questions[].answers[].id` | `number` | Answer identifier (`Long` serialized as number). |
| `questions[].answers[].title` | `string` | Answer text. |
| `questions[].answers[].correct` | `boolean` | Correct-answer marker in backend response payload. |

### Errors

| Name | Code | Description |
| --- | --- | --- |
| `401 Unauthorized` | HTTP | `x-api-key` is missing, invalid, or expired. |
| `403 Forbidden` | HTTP | Merchant has no permission for this endpoint (`KYC_CRYPTO_TEST_EP`). |

### Step 4. Submit crypto-test answers
Use this endpoint to submit crypto test answers and optionally update legal agreement flags in the same call.
Use the response `accepted` to detect whether the update was applied (`true`) or skipped for non-resident clients (`false`).

**POST** `/api/v2/kyc/merchant/client/crypto-test`

**Headers**
- `x-api-key: {{x-api-key}}`
- `externalClientId: {{externalClientId}}` (optional)

**Request**
```json
{
  "clientId": "0d58e7ec-0369-48d7-9804-90c6b23a52be",
  "answers": {
    "1": 10,
    "2": 20,
    "3": 30,
    "4": 40,
    "5": 50
  },
  "notUSTaxPayer": true,
  "agreedWithOffer": true,
  "exchangeInPersonalInterests": true
}
```

**Response**
```json
{
  "accepted": true
}
```

### Headers

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| `x-api-key` | `string` | Yes | Authenticates merchant backend request for crypto test endpoints. |
| `externalClientId` | `string` | No | Merchant-side external client id for additional client lookup. |

### Request

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| `clientId` | `string` | Yes | WhiteBird client id. |
| `answers` | `object` | Yes | Map of question id to answer id (`Map<Long, Long>`). |
| `notUSTaxPayer` | `boolean` | No | Optional consent update flag. |
| `agreedWithOffer` | `boolean` | No | Optional offer-agreement update flag. |
| `exchangeInPersonalInterests` | `boolean` | No | Optional personal-interests update flag. |

### Response

| Name | Type | Description |
| --- | --- | --- |
| `accepted` | `boolean` | `true` when crypto test is applied; `false` for non-resident clients. |

### Errors

| Name | Code | Description |
| --- | --- | --- |
| `400 ValidationException` | BUSINESS | Wrong answers to crypto test. |
| `401 Unauthorized` | HTTP | `x-api-key` is missing, invalid, or expired. |
| `403 Forbidden` | HTTP | Merchant has no permission for this endpoint (`KYC_CRYPTO_TEST_EP`). |

## 4) SDK Light Registration

### Step 5. Register client with minimal data (non-full KYC)
Use this endpoint when merchant has only minimal client data and full KYC profile is not provided at registration time.
Use the response `id` as `clientId` for token generation and subsequent KYC-related actions.

**POST** `/api/v2/auth/merchant/client/register`

**Headers**
- `x-api-key: {{x-api-key}}`

**Request**
```json
{
  "email": "client@example.com",
  "phone": "+375297778899",
  "externalClientId": "partner-client-123",
  "agreedWithOffer": true
}
```

**Response**
```json
{
  "id": "0d58e7ec-0369-48d7-9804-90c6b23a52be",
  "status": "NOT_VERIFIED"
}
```

### Headers

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| `x-api-key` | `string` | Yes | Authenticates merchant backend request for auth registration endpoint. |

### Request

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| `email` | `string` | Yes | Client email (`@NotEmpty`). |
| `phone` | `string` | Yes | Client phone (`@NotEmpty`). |
| `merchantId` | `string` | No | Optional merchant id used in OTP registration flow, not required for backend `x-api-key` flow. |
| `externalClientId` | `string` | No | Merchant-side external client identifier. |
| `agreedWithOffer` | `boolean` | No | Offer-agreement flag passed to registration service. |

### Response

| Name | Type | Description |
| --- | --- | --- |
| `id` | `string` | WhiteBird client id created/linked in merchant context. |
| `status` | `string` | Initial client status (typically `NOT_VERIFIED`). |

### Errors

| Name | Code | Description |
| --- | --- | --- |
| `400 ValidationException` | BUSINESS | Required registration field validation failed. |
| `401 Unauthorized` | HTTP | `x-api-key` is missing, invalid, or expired. |
| `403 Forbidden` | HTTP | Merchant has no permission for this endpoint (`AUTH_REGISTER_EP`). |

## 5) SDK Token Generation

### Step 6. Generate SDK access tokens
Use this endpoint to issue SDK access and refresh tokens for an existing merchant client.
Use the response tokens to initialize SDK session for the resolved `clientId`.

**POST** `/api/v2/auth/merchant/client/token/generate`

**Headers**
- `x-api-key: {{x-api-key}}`

**Request**
```json
{
  "clientId": "0d58e7ec-0369-48d7-9804-90c6b23a52be",
  "externalClientId": "partner-client-123"
}
```

**Response**
```json
{
  "token": "access-token",
  "refreshToken": "refresh-token"
}
```

### Headers

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| `x-api-key` | `string` | Yes | Authenticates merchant backend request for token generation endpoint. |

### Request

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| `clientId` | `string` | Yes | WhiteBird client id to generate token for. |
| `externalClientId` | `string` | No | Merchant-side external client identifier used for additional client validation. |

### Response

| Name | Type | Description |
| --- | --- | --- |
| `token` | `string` | OAuth access token for SDK session. |
| `refreshToken` | `string \| null` | OAuth refresh token; can be `null`. |

### Errors

| Name | Code | Description |
| --- | --- | --- |
| `401 Unauthorized` | HTTP | `x-api-key` is missing, invalid, or expired. |
| `403 Forbidden` | HTTP | Merchant has no permission for this endpoint (`AUTH_GET_TOKEN_EP`). |

## 6) Optional KYC Support Endpoints

### Step 7. Update agreement flags for existing client
Use this endpoint to set legal agreement flags for an already registered client.
Use it when agreement data is collected after initial registration and must be persisted in KYC/CRM flows.

**POST** `/api/v2/kyc/merchant/client/agreed-offer`

**Headers**
- `x-api-key: {{x-api-key}}`
- `externalClientId: {{externalClientId}}` (optional)

**Request**
```json
{
  "clientId": "0d58e7ec-0369-48d7-9804-90c6b23a52be",
  "notUSTaxPayer": true,
  "agreedWithOffer": true,
  "exchangeInPersonalInterests": true
}
```

**Response**
```json
"OK"
```

### Headers

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| `x-api-key` | `string` | Yes | Authenticates merchant backend request for agreement update endpoint. |
| `externalClientId` | `string` | No | Merchant-side external client identifier used for additional client lookup. |

### Request

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| `clientId` | `string` | Yes | WhiteBird client id. |
| `notUSTaxPayer` | `boolean` | No | Non-US taxpayer agreement flag. |
| `agreedWithOffer` | `boolean` | No | WhiteBird public offer agreement flag. |
| `exchangeInPersonalInterests` | `boolean` | No | Exchange-in-personal-interests agreement flag. |

### Response

| Name | Type | Description |
| --- | --- | --- |
| `result` | `string` | Constant string response: `OK`. |

### Errors

| Name | Code | Description |
| --- | --- | --- |
| `400 ValidationException` | BUSINESS | Client was not found for provided identifier. |
| `401 Unauthorized` | HTTP | `x-api-key` is missing, invalid, or expired. |
| `403 Forbidden` | HTTP | Merchant has no permission for this endpoint (`KYC_AGREED_OFFER_EP`). |

### Step 8. Get personal number for client
Use this endpoint to retrieve personal number stored for an already registered client.
Use the response for compliance flows where personal number retrieval is required by merchant process.

**POST** `/api/v2/kyc/merchant/client/personal-number`

**Headers**
- `x-api-key: {{x-api-key}}`
- `externalClientId: {{externalClientId}}` (optional)

**Request**
```json
{
  "clientId": "0d58e7ec-0369-48d7-9804-90c6b23a52be"
}
```

**Response**
```json
{
  "personalNumber": "3029120H059PB9"
}
```

### Headers

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| `x-api-key` | `string` | Yes | Authenticates merchant backend request for personal-number endpoint. |
| `externalClientId` | `string` | No | Merchant-side external client identifier used for additional client lookup. |

### Request

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| `clientId` | `string` | Yes | WhiteBird client id. |

### Response

| Name | Type | Description |
| --- | --- | --- |
| `personalNumber` | `string \| null` | Personal number returned from WB CRM profile. |

### Errors

| Name | Code | Description |
| --- | --- | --- |
| `401 Unauthorized` | HTTP | `x-api-key` is missing, invalid, or expired. |
| `403 Forbidden` | HTTP | Merchant has no permission for this endpoint (`KYC_GET_PERSONAL_NUMBER_EP`). |

## Data Dictionaries

```typescript
type Gender = string // examples in existing integrations: "муж", "жен"

type CountryCode = string // numeric ISO 3166-1 code, e.g. "112", "643"

type DocType = string // examples in integrations: "3", "9"

enum KycClientStatus {
  NOT_VERIFIED = "NOT_VERIFIED",
  PENDING = "PENDING",
  VERIFIED = "VERIFIED",
  FROZEN = "FROZEN",
  ARREST = "ARREST"
}
```
