<h1 align="center">KPLC Token Lookup Tool</h1>
<p align="center">An Open API for retrieving Kenya Power (KPLC) prepaid token purchase history and insights</p>

<p align="center">
  <img src="https://github.com/MurageKabui/KPLC-Tokens-LookUp-Tool/blob/main/preview.png?raw=true"><br>
</p>


> ⚠️ **DISCLAIMER**: This project is **not affiliated with, endorsed by, or connected to** Kenya Power and Lighting Company (KPLC).
> It exists for educational and utility purposes only.


- Web interface:  
  https://denniskabui.com/projects/my-kplc-token/
- API reference:  
  https://denniskabui.com/projects/my-kplc-token/docs.html

<hr>
  
## Overview
A self hosted web interface and public API for retrieving Kenya Power (KPLC) prepaid token purchase history and transaction insights.
Through analysis of the official KPLC Android application's network interactions, this project provides an accessible REST API and interactive dashboard for meter token data.

## Features

- Retrieve full token purchase history for a given meter number
- Cost and unit breakdown per transaction
- AI-powered break down of cost components
- Public REST API for integrations
- Browser persistence for saved meter numbers
- Responsive UI suitable for desktop and mobile

## API Reference

### Base URL

```
https://denniskabui.com/projects/my-kplc-token
```

### 1. Get Token History

Returns a list of historical token transactions for a specific meter.

**Endpoint**

```
GET /api/tokens/:meter
````

- `meter` - String (required): - Prepaid meter number (e.g., `54601446543`)

**Example Request**

```bash
curl -X GET "https://denniskabui.com/projects/my-kplc-token/api/tokens/54601446543" \
     -H "Accept: application/json"
````

**Example Response**

```json
{
  "success": true,
  "data": [
    {
      "trnTimestamp": 1709280000000,
      "trnAmount": "500.00",
      "trnUnits": "18.50",
      "tokenNo": "1234-5678-9012-3456-7890",
      "recptNo": "RCT123456",
      "pMethod": "Cash",
      "concepts": [
        { "codConcept": "RESSTEP0", "amount": 300.00 },
        { "codConcept": "RESSTEP0TAX", "amount": 80.00 }
      ]
    }
  ]
}
```

### 2. Generate AI Explanation

Analyze a specific transaction's cost breakdown and get a plain-language explanation.

**Endpoint**

```
POST /api/explain
```

**Example Request**

```bash
curl -X POST "https://denniskabui.com/projects/my-kplc-token/api/explain" \
     -H "Content-Type: application/json" \
     -d '{
           "amount": "500",
           "units": "18.5",
           "date": "15/12/2024",
           "breakdown": [
             { "codConcept": "RESSTEP1", "amount": 300 },
             { "codConcept": "RESSTEP0TAX", "amount": 80 }
           ]
         }'
```

**Example Response**

```json
{
  "success": true,
  "explanation": "Mwananchi, you paid KES 500. About 40% of this went to taxes. Verdict: High Tax/Deductions."
}
```

## Quick Start

1. Open the live portal in a browser.
2. Enter a valid 11 digit prepaid meter number.
3. Review purchase history and visual breakdowns.
4. Use the API for automation or integration.

## Front End Tech Stack

* HTML5, CSS3, JavaScript
* jQuery for DOM handling and requests
* Chart.js for data visualization
* Remix Icons for UI symbols
* Google Fonts (Outfit, JetBrains Mono)
* LocalStorage for persistent client data

## Data Handling and Privacy

* No user accounts or server-side personal data storage
* Meter numbers saved locally in the user's browser
* The API operates in read-only mode, relaying publicly accessible information

## Limitations

* Dependent on upstream KPLC services
* Responses are limited to what data is available through the reverse-engineered sources
* The API may break if KPLC changes implementation details
