# Registration API

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
}
```

**Response**
```json
{
  "id": "0d58e7ec-0369-48d7-9804-90c6b23a52be",
  "status": "PENDING"
}
```

**Headers**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">x-api-key</td>
      <td>string</td>
      <td>Yes</td>
      <td>Authenticates merchant backend request for KYC registration endpoints.</td>
    </tr>
  </tbody>
</table>

**Request**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">email</td>
      <td>string</td>
      <td>Yes</td>
      <td>Client email.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">phone</td>
      <td>string</td>
      <td>Yes</td>
      <td>Client phone.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">firstNameRu</td>
      <td>string</td>
      <td>Yes</td>
      <td>First name in Cyrillic.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">lastNameRu</td>
      <td>string</td>
      <td>Yes</td>
      <td>Last name in Cyrillic.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">patronymicRu</td>
      <td>string</td>
      <td>Yes</td>
      <td>Patronymic in Cyrillic.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">firstName</td>
      <td>string</td>
      <td>Yes</td>
      <td>First name in Latin.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">lastName</td>
      <td>string</td>
      <td>Yes</td>
      <td>Last name in Latin.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">residence</td>
      <td>string</td>
      <td>Yes</td>
      <td>Residence country code.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">placeOfBirth</td>
      <td>string</td>
      <td>Yes</td>
      <td>Place of birth.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">birthDate</td>
      <td>string</td>
      <td>No</td>
      <td>Birth date in YYYY-MM-DD (mapped to LocalDate).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">registrationCountry</td>
      <td>string</td>
      <td>Yes</td>
      <td>Registration country code.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">registrationRegion</td>
      <td>string</td>
      <td>Yes</td>
      <td>Registration region/state.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">residenceDistrict</td>
      <td>string</td>
      <td>Yes</td>
      <td>Registration district field.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">registrationCity</td>
      <td>string</td>
      <td>Yes</td>
      <td>Registration city/locality.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">registrationStreet</td>
      <td>string</td>
      <td>Yes</td>
      <td>Registration street.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">registrationHouseAndFlat</td>
      <td>string</td>
      <td>Yes</td>
      <td>House/flat part of registration address.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">identityDocType</td>
      <td>string</td>
      <td>Yes</td>
      <td>Identity document type.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">identityDocIssueDate</td>
      <td>string</td>
      <td>No</td>
      <td>Document issue date string.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">identityDocExpireDate</td>
      <td>string</td>
      <td>No</td>
      <td>Document expiry date string.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">identityDocNumber</td>
      <td>string</td>
      <td>Yes</td>
      <td>Identity document number.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">identityDocIssuer</td>
      <td>string</td>
      <td>Yes</td>
      <td>Identity document issuer.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">personalNumber</td>
      <td>string</td>
      <td>Conditional</td>
      <td>Required when registrationCountry contains 112 (Belarus).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">postCode</td>
      <td>string</td>
      <td>Yes</td>
      <td>Postal code.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">gender</td>
      <td>string</td>
      <td>Yes</td>
      <td>Gender value, e.g. муж, жен.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">nationality</td>
      <td>string</td>
      <td>Yes</td>
      <td>Nationality country code.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">notUSTaxPayer</td>
      <td>boolean</td>
      <td>No</td>
      <td>Consent flag for non-US taxpayer declaration.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">exchangeInPersonalInterests</td>
      <td>boolean</td>
      <td>No</td>
      <td>Consent flag for personal-interest exchange declaration.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">agreedWithOffer</td>
      <td>boolean</td>
      <td>No</td>
      <td>Consent flag for WhiteBird public offer.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">files</td>
      <td>array of string</td>
      <td>No</td>
      <td>Optional list of file references stored with client KYC data.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">externalClientId</td>
      <td>string</td>
      <td>No</td>
      <td>Merchant-side external client identifier.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">isPotentialDrop</td>
      <td>boolean</td>
      <td>No</td>
      <td>Optional risk marker propagated to CRM processing.</td>
    </tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">id</td>
      <td>string</td>
      <td>WhiteBird client id created/linked during registration.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">status</td>
      <td>string</td>
      <td>Client status after registration. Allowed values: NOT_VERIFIED, PENDING, VERIFIED, FROZEN, ARREST.</td>
    </tr>
  </tbody>
</table>

**Errors**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Code</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 ValidationException</td>
      <td>BUSINESS</td>
      <td>One or more required fields are missing/empty, or validation rules fail.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 ValidationException</td>
      <td>BUSINESS</td>
      <td>personalNumber is missing for Belarus registration country (112).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">401 Unauthorized</td>
      <td>HTTP</td>
      <td>x-api-key is missing, invalid, or expired.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">403 Forbidden</td>
      <td>HTTP</td>
      <td>Merchant has no permission for this endpoint.</td>
    </tr>
  </tbody>
</table>

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

**Headers**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">x-api-key</td>
      <td>string</td>
      <td>Yes</td>
      <td>Authenticates merchant backend request for status checks.</td>
    </tr>
  </tbody>
</table>

**Params**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">externalUserId</td>
      <td>string</td>
      <td>No</td>
      <td>Merchant-side external id used as optional additional client validation key.</td>
    </tr>
  </tbody>
</table>

**Request**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">clientId</td>
      <td>string</td>
      <td>Yes</td>
      <td>WhiteBird client id returned by registration.</td>
    </tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">status</td>
      <td>string</td>
      <td>KYC status string from backend. Possible values include NOT_VERIFIED, PENDING, VERIFIED, FROZEN, ARREST.</td>
    </tr>
  </tbody>
</table>

**Errors**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Code</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">401 Unauthorized</td>
      <td>HTTP</td>
      <td>x-api-key is missing, invalid, or expired.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">403 Forbidden</td>
      <td>HTTP</td>
      <td>Merchant has no permission for this endpoint.</td>
    </tr>
  </tbody>
</table>

## 3) Crypto Test

### Step 3. Get crypto-test requirements/questions
Use this endpoint to determine whether the client must pass crypto test and to fetch questions for residents where test is required.
Use the response to render the test UI or skip test step if `cryptoTestRequired=false`.

**GET** `/api/v2/kyc/merchant/client/crypto-test?clientId={{clientId}}`

**Headers**
- `x-api-key: {{x-api-key}}`

**Response**
```json
{
    "cryptoTestRequired": true,
    "questions": [
        {
            "id": "1",
            "title": "Что такое криптовалюта?",
            "answers": [
                {
                    "id": 1,
                    "title": "Зашифрованная валюта госбанка.",
                    "correct": false
                },
                {
                    "id": 2,
                    "title": "Биткоин, иной цифровой знак (токен), используемый в международном обороте в качестве универсального средства обмена.",
                    "correct": true
                },
                {
                    "id": 3,
                    "title": "Международная платежная система.",
                    "correct": false
                }
            ]
        },
        {
            "id": "2",
            "title": "Пожалуйста, укажите верное утверждение",
            "answers": [
                {
                    "id": 4,
                    "title": "Приобретение токенов может привести к полной потере денежных средств в том числе в результате волатильности стоимости токенов, совершения противоправных действий, технических сбоев (ошибок).",
                    "correct": true
                },
                {
                    "id": 5,
                    "title": "Приобретение токенов гарантирует полную сохранность денежных средств.",
                    "correct": false
                }
            ]
        },
        {
            "id": "3",
            "title": "Чем определяется цена Биткоина?",
            "answers": [
                {
                    "id": 7,
                    "title": "Цену устанавливают разработчики Биткоина.",
                    "correct": false
                },
                {
                    "id": 8,
                    "title": "Цена зависит от стоимости барреля нефти на биржах.",
                    "correct": false
                },
                {
                    "id": 9,
                    "title": "Спросом и предложением на рынке.",
                    "correct": true
                }
            ]
        },
        {
            "id": "4",
            "title": "Биткоин является:",
            "answers": [
                {
                    "id": 10,
                    "title": "Криптовалютой, обеспеченной долларом США.",
                    "correct": false
                },
                {
                    "id": 11,
                    "title": "Необеспеченной криптовалютой.",
                    "correct": true
                },
                {
                    "id": 12,
                    "title": "Криптовалютой, обеспеченной золотом.",
                    "correct": false
                }
            ]
        },
        {
            "id": "5",
            "title": "Пожалуйста, укажите верное утверждение",
            "answers": [
                {
                    "id": 13,
                    "title": "Биткоин является официальным расчетным (платежным) средством на территории РБ за который можно приобретать товары, работы услуги.",
                    "correct": false
                },
                {
                    "id": 14,
                    "title": "Биткоин не является расчетным (платежным) средством на территории РБ.",
                    "correct": true
                }
            ]
        }
    ]
}
```

**Headers**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">x-api-key</td>
      <td>string</td>
      <td>Yes</td>
      <td>Authenticates merchant backend request for crypto test endpoints.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">externalClientId</td>
      <td>string</td>
      <td>No</td>
      <td>Merchant-side external client id for additional client lookup.</td>
    </tr>
  </tbody>
</table>

**Params**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">clientId</td>
      <td>string</td>
      <td>Yes</td>
      <td>WhiteBird client id.</td>
    </tr>
  </tbody>
</table>

**Request**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">body</td>
      <td>none</td>
      <td>-</td>
      <td>GET endpoint without request body.</td>
    </tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">cryptoTestRequired</td>
      <td>boolean</td>
      <td>Indicates whether client must pass crypto test.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">questions</td>
      <td>array of objects</td>
      <td>Test questions list, returned when cryptoTestRequired=true.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">questions[].id</td>
      <td>string</td>
      <td>Question identifier.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">questions[].title</td>
      <td>string</td>
      <td>Question text.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">questions[].answers</td>
      <td>array of objects</td>
      <td>Answer options list.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">questions[].answers[].id</td>
      <td>number</td>
      <td>Answer identifier (Long serialized as number).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">questions[].answers[].title</td>
      <td>string</td>
      <td>Answer text.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">questions[].answers[].correct</td>
      <td>boolean</td>
      <td>Correct-answer marker in backend response payload.</td>
    </tr>
  </tbody>
</table>

**Errors**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Code</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">401 Unauthorized</td>
      <td>HTTP</td>
      <td>x-api-key is missing, invalid, or expired.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">403 Forbidden</td>
      <td>HTTP</td>
      <td>Merchant has no permission for this endpoint.</td>
    </tr>
  </tbody>
</table>

### Step 4. Submit crypto-test answers
Use this endpoint to submit crypto test answers and optionally update legal agreement flags in the same call.
Use the response `accepted` to detect whether the update was applied (`true`) or skipped for non-resident clients (`false`).

**POST** `/api/v2/kyc/merchant/client/crypto-test`

**Headers**
- `x-api-key: {{x-api-key}}`

**Request**
```json
{
    "clientId": "{{clientId}}",
    "answers": {
        "1": 2,
        "2": 4,
        "3": 9,
        "4": 11,
        "5": 14
    }
}
```

**Response**
```json
{
  "accepted": true
}
```

**Headers**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">x-api-key</td>
      <td>string</td>
      <td>Yes</td>
      <td>Authenticates merchant backend request for crypto test endpoints.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">externalClientId</td>
      <td>string</td>
      <td>No</td>
      <td>Merchant-side external client id for additional client lookup.</td>
    </tr>
  </tbody>
</table>

**Request**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">clientId</td>
      <td>string</td>
      <td>Yes</td>
      <td>WhiteBird client id.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">answers</td>
      <td>object</td>
      <td>Yes</td>
      <td>Map of question id to answer id (Map<Long, Long>).</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">notUSTaxPayer</td>
      <td>boolean</td>
      <td>No</td>
      <td>Optional consent update flag.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">agreedWithOffer</td>
      <td>boolean</td>
      <td>No</td>
      <td>Optional offer-agreement update flag.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">exchangeInPersonalInterests</td>
      <td>boolean</td>
      <td>No</td>
      <td>Optional personal-interests update flag.</td>
    </tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">accepted</td>
      <td>boolean</td>
      <td>true when crypto test is applied; false for non-resident clients.</td>
    </tr>
  </tbody>
</table>

**Errors**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Code</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 ValidationException</td>
      <td>BUSINESS</td>
      <td>Wrong answers to crypto test.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">401 Unauthorized</td>
      <td>HTTP</td>
      <td>x-api-key is missing, invalid, or expired.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">403 Forbidden</td>
      <td>HTTP</td>
      <td>Merchant has no permission for this endpoint.</td>
    </tr>
  </tbody>
</table>

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
  "email": "n.soldatov@whitebird.by",
  "phone": "+375295805525",
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

**Headers**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">x-api-key</td>
      <td>string</td>
      <td>Yes</td>
      <td>Authenticates merchant backend request for auth registration endpoint.</td>
    </tr>
  </tbody>
</table>

**Request**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">email</td>
      <td>string</td>
      <td>Yes</td>
      <td>Client email.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">phone</td>
      <td>string</td>
      <td>Yes</td>
      <td>Client phone.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">merchantId</td>
      <td>string</td>
      <td>No</td>
      <td>Optional merchant id used in OTP registration flow, not required for backend x-api-key flow.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">externalClientId</td>
      <td>string</td>
      <td>No</td>
      <td>Merchant-side external client identifier.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">agreedWithOffer</td>
      <td>boolean</td>
      <td>No</td>
      <td>Offer-agreement flag passed to registration service.</td>
    </tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">id</td>
      <td>string</td>
      <td>WhiteBird client id created/linked in merchant context.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">status</td>
      <td>string</td>
      <td>Initial client status (typically NOT_VERIFIED).</td>
    </tr>
  </tbody>
</table>

**Errors**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Code</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 ValidationException</td>
      <td>BUSINESS</td>
      <td>Required registration field validation failed.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">401 Unauthorized</td>
      <td>HTTP</td>
      <td>x-api-key is missing, invalid, or expired.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">403 Forbidden</td>
      <td>HTTP</td>
      <td>Merchant has no permission for this endpoint.</td>
    </tr>
  </tbody>
</table>

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
    "clientId": "{{clientId}}"
}
```

**Response**
```json
{
  "token": "access-token",
  "refreshToken": "refresh-token"
}
```

**Headers**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">x-api-key</td>
      <td>string</td>
      <td>Yes</td>
      <td>Authenticates merchant backend request for token generation endpoint.</td>
    </tr>
  </tbody>
</table>

**Request**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">clientId</td>
      <td>string</td>
      <td>Yes</td>
      <td>WhiteBird client id to generate token for.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">externalClientId</td>
      <td>string</td>
      <td>No</td>
      <td>Merchant-side external client identifier used for additional client validation.</td>
    </tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">token</td>
      <td>string</td>
      <td>OAuth access token for SDK session.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">refreshToken</td>
      <td>string | null</td>
      <td>OAuth refresh token; can be null.</td>
    </tr>
  </tbody>
</table>

**Errors**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Code</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">401 Unauthorized</td>
      <td>HTTP</td>
      <td>x-api-key is missing, invalid, or expired.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">403 Forbidden</td>
      <td>HTTP</td>
      <td>Merchant has no permission for this endpoint.</td>
    </tr>
  </tbody>
</table>

## 6) Optional KYC Support Endpoints

### Step 7. Update agreement flags for existing client
Use this endpoint to set legal agreement flags for an already registered client.
Use it when agreement data is collected after initial registration and must be persisted in KYC/CRM flows.

**POST** `/api/v2/kyc/merchant/client/agreed-offer`

**Headers**
- `x-api-key: {{x-api-key}}`

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

**Headers**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">x-api-key</td>
      <td>string</td>
      <td>Yes</td>
      <td>Authenticates merchant backend request for agreement update endpoint.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">externalClientId</td>
      <td>string</td>
      <td>No</td>
      <td>Merchant-side external client identifier used for additional client lookup.</td>
    </tr>
  </tbody>
</table>

**Request**

<table width="100%">
  <thead>
    <tr>
      <th width="200" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="100">Required</th>
      <th width="580">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">clientId</td>
      <td>string</td>
      <td>Yes</td>
      <td>WhiteBird client id.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">notUSTaxPayer</td>
      <td>boolean</td>
      <td>No</td>
      <td>Non-US taxpayer agreement flag.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">agreedWithOffer</td>
      <td>boolean</td>
      <td>No</td>
      <td>WhiteBird public offer agreement flag.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">exchangeInPersonalInterests</td>
      <td>boolean</td>
      <td>No</td>
      <td>Exchange-in-personal-interests agreement flag.</td>
    </tr>
  </tbody>
</table>

**Response**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Type</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">result</td>
      <td>string</td>
      <td>Constant string response: OK.</td>
    </tr>
  </tbody>
</table>

**Errors**

<table width="100%">
  <thead>
    <tr>
      <th width="240" style="word-break: break-word; white-space: normal;">Name</th>
      <th width="120">Code</th>
      <th width="640">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="word-break: break-word; white-space: normal;">400 ValidationException</td>
      <td>BUSINESS</td>
      <td>Client was not found for provided identifier.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">401 Unauthorized</td>
      <td>HTTP</td>
      <td>x-api-key is missing, invalid, or expired.</td>
    </tr>
    <tr>
      <td style="word-break: break-word; white-space: normal;">403 Forbidden</td>
      <td>HTTP</td>
      <td>Merchant has no permission for this endpoint.</td>
    </tr>
  </tbody>
</table>
