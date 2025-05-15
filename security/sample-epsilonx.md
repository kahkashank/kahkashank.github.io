---
layout: default
title: Samples
---


# Cybersecurity Documentation Samples  
**Author:** Kay Kazmi  
**Role:** Senior Technical Writer | Content Strategist | API & Cloud Documentation Expert  

---

## Sample 1: Developer Guide – EpsilonX Threat Intelligence API

### Overview  
The EpsilonX Threat Intelligence API allows users to programmatically interact with EpsilonX's global threat database. It supports retrieving threat intelligence based on file hashes, and submitting suspicious files for analysis.

### Base URL  
```
https://api.epsilonx.io/v1
```

---

### Authentication  

All endpoints require a valid API key via the `Authorization` header.

```http
Authorization: Bearer YOUR_API_KEY
```

---

### Endpoint: `GET /threats/hash/{sha256}`  

Retrieve threat intelligence for a specific file using its SHA-256 hash.

#### Request  
```bash
curl -X GET \
  https://api.epsilonx.io/v1/threats/hash/abc123... \
  -H "Authorization: Bearer YOUR_API_KEY"
```

#### Response  
```json
{
  "sha256": "abc123...",
  "threat_level": "High",
  "malware_family": "Emotet",
  "first_seen": "2024-04-22T14:02:00Z",
  "last_seen": "2025-01-12T09:15:00Z"
}
```

#### Status Codes  
- `200 OK`: Successful query  
- `404 Not Found`: Hash not found  
- `401 Unauthorized`: Invalid or missing token  

---

### Endpoint: `POST /threats/report`  

Submit a suspicious file hash for review and analysis.

#### Request  
```json
{
  "sha256": "abc123...",
  "filename": "invoice.pdf",
  "notes": "Suspicious attachment from unknown sender"
}
```

#### Response  
```json
{
  "message": "Submission received. Thank you for contributing."
}
```

---

### Rate Limits  
- 1000 requests/day  
- 10 requests/second  
- 429 returned on throttling  

---

## Sample 2: Integration Guide – Connecting EpsilonX to Splunk

### Purpose  
This guide walks through the steps to integrate EpsilonX alerts with **Splunk SIEM** using **HTTP Event Collector (HEC)** to support centralized log aggregation and alerting.

---

### Prerequisites  

- Admin access to EpsilonX Console  
- Splunk HEC endpoint and token  
- Internet access between EpsilonX and Splunk  

---

### Step 1: Configure Splunk HEC

1. In Splunk, go to **Settings > Data Inputs > HTTP Event Collector**
2. Create a new token (e.g., `EpsilonX_Token`)
3. Note the endpoint (e.g., `https://splunk.company.com:8088/services/collector`) and token

---

### Step 2: Set Up Webhook in EpsilonX

1. Navigate to **Settings > Integrations > Webhooks**
2. Click **Add Webhook**
3. Fill out:
   - **Name**: `Splunk Alert Hook`
   - **URL**: Your Splunk HEC endpoint
   - **Token**: The HEC token from Splunk
   - **Payload Format**: `JSON`

---

### Step 3: Define Payload Template

```json
{
  "event": {
    "alert_id": "{{alert.id}}",
    "host": "{{device.hostname}}",
    "threat": "{{threat.name}}",
    "severity": "{{threat.severity}}",
    "timestamp": "{{alert.timestamp}}"
  }
}
```

---

### Step 4: Test Integration

- Trigger a test alert from EpsilonX  
- Use Splunk search:  
  ```spl
  index="main" source="EpsilonX"
  ```

---

### Troubleshooting Table

| Issue                  | Resolution                                    |
|------------------------|-----------------------------------------------|
| 400 Bad Request        | Check JSON format and token                   |
| Webhook timeout        | Ensure Splunk endpoint is accessible          |
| Data not arriving      | Confirm HEC is enabled and input index exists |
| Payload malformed      | Validate token and template placeholders      |

---



## About the Author

**Kay Kazmi**  
Senior Technical Writer with 15+ years of experience producing developer, API, and enterprise product documentation across cloud, SaaS, cybersecurity, and telecom domains.

---

> *Note: All product names and use cases are fictional and for demonstration purposes only.*
