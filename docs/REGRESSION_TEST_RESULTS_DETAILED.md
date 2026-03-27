# Detailed Regression Test Results (Happy Path + Error Path)

This sheet tracks endpoint-level regression scenarios with explicit inputs, expected behavior, and observed behavior.

## 1) Test Environment

- Base URL: `https://american-airlines-info-app-dev-jjvb3b.5sc6y6-3.usa-e2.cloudhub.io`
- Runtime target: `american-airlines-info-app-dev`
- Method: cURL smoke/regression runs with representative payloads

## 2) API Services - Happy Paths

| ID | Endpoint | Input | Expected | Observed | Result |
|---|---|---|---|---|---|
| HP-01 | `GET /api/flights` | none | `200` with list payload | `200` | Pass |
| HP-02 | `GET /api/flights?destination=SFO` | query param `destination=SFO` | `200` filtered list | `200` | Pass |
| HP-03 | `GET /api/flights/{ID}` | `ID=903777` | `200` single-flight payload | `200` | Pass |
| HP-04 | `POST /dw/simple` | JSON with `Field1` and `phoneNumber` | `200`, uppercase + de-hyphenated phone + bookingDate | `200` | Pass |
| HP-05 | `POST /dw/complex` | XML flights/options payload | `200`, extracted option IDs + seat filter + pricing transform | `200` | Pass |
| HP-06 | `POST /dw/multi1` | Passenger JSON with baggage and travelDate | `200`, XML PNR output with baggage split + date format | `200` | Pass |
| HP-07 | `POST /dw/multi2` | XML flights payload | `200`, JSON array with CANCELLED removed and route/status mapping | `200` | Pass |

## 3) API Services - Error/Negative Paths

| ID | Endpoint | Input | Expected | Observed | Result |
|---|---|---|---|---|---|
| ER-01 | `DELETE /api/flights/{ID}` | missing `ID=9999999` | `404` not found mapping | `404` | Pass |
| ER-02 | `PUT /api/flights/{ID}` | invalid/incomplete update payload | `400` validation error | `400` | Pass |
| ER-03 | `POST /api/flights/batch` | malformed/invalid batch payload | `400` validation error | `400` | Pass |

## 4) Scenarios Requiring Follow-up in Next Cycle

| ID | Endpoint | Input | Expected | Observed | Status |
|---|---|---|---|---|---|
| FU-01 | `POST /api/flights` | create payload for new ID | `201`/`200` create success | `500` in latest smoke run | Needs investigation |
| FU-02 | `DELETE /api/flights/{ID}` | previously created ID | `200` or `204` delete success | `404` when create step failed | Blocked by FU-01 |

## 5) Representative Inputs Used

### 5.1 `POST /api/flights` sample

```json
{
  "ID": 904111,
  "code": "AA904111",
  "price": 450,
  "departureDate": "2026-07-01T10:00:00Z",
  "origin": "CLE",
  "destination": "SFO",
  "emptySeats": 25,
  "plane": {
    "type": "Boeing 737",
    "totalSeats": 180
  }
}
```

### 5.2 `POST /dw/simple` sample

```json
{
  "Field1": "hello",
  "phoneNumber": "555-111-2222"
}
```

### 5.3 `POST /dw/complex` sample

```xml
<orders>
  <order>
    <id>1</id>
    <amount>100</amount>
  </order>
</orders>
```

## 6) Notes

- This document is intentionally evidence-first: if a scenario failed in the latest regression run, it is marked as follow-up instead of being reported as pass.
- Initial `500` signals observed for `/dw/simple` and `/dw/multi1` were caused by malformed JSON in PowerShell inline-curl quoting, not DW logic defects. File-based payload retest confirmed both endpoints return expected output (`200`).
- Full runnable request definitions are maintained in:
  - `postman/american-airlines-info-api-playground.postman_collection.json`
  - `postman/american-airlines-info-api-cloudhub.postman_environment.json`
  - `postman/american-airlines-info-api-local.postman_environment.json`

## 7) Latest DW Output Evidence (Validated)

### 7.1 `/dw/simple` observed output (`200`)

```json
{
  "Field2": "HELLO",
  "phoneNumber": "5551112222",
  "bookingDate": "2026-03-27T19:18:16Z"
}
```

### 7.2 `/dw/multi1` observed output (`200`)

```xml
<?xml version='1.0' encoding='UTF-8'?>
<pnrData>
  <passengerName>Jane Doe</passengerName>
  <flightNumber>AA450</flightNumber>
  <travelDate>2026-04-15</travelDate>
  <specialAssistance>true</specialAssistance>
  <baggage>
    <normalBaggage>
      <id>B1</id>
      <weightKg>20</weightKg>
    </normalBaggage>
    <heavyBaggage>
      <id>B2</id>
      <weightKg>35</weightKg>
    </heavyBaggage>
  </baggage>
</pnrData>
```
