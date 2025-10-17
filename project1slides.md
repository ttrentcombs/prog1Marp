---
marp: true
title: Concert REST API
paginate: true
---
# Concert Tickets REST API
### ASE Project 1
**Trent Combs**

---
## Overview
This project simulates a concert ticket system with user accounts, venues, concerts, and ticket bookings.

---

## Tools Used
- XAMPP
- phpMyAdmin 
- Git Bash 
- Visual Studio Code 
- Marp/pdf

---
## Database Design
**Tables:**
- `users` – login info and tokens  
- `venues` – concert locations  
- `concerts` – concert events  
- `bookings` – ticket orders

Each table is linked using foreign keys for relationships.

---
## API Endpoints
| Endpoint | Method | Description | Auth |
|-----------|--------|--------------|------|
| /register.php | POST | Create a new user | ❌ |
| /login.php | POST | Login and get token | ❌ |
| /venues.php | GET | List venues | ❌ |
| /venues.php | POST | Add venue | ✅ |
| /concerts.php | GET | List concerts | ❌ |
| /concerts.php | POST | Add concert | ✅ |
| /bookings.php | GET | View bookings | ✅ |
| /bookings.php | POST | Book a concert | ✅ |
| /profile.php | GET | View profile | ✅ |
| /health.php | GET | API check | ❌ |

---
## Register User Example
**Request**
```bash
curl -X POST -H "Content-Type: application/json" -d '{"username":"trent","password":"test123"}' http://localhost/api/register.php
```
**Response**
```json
{"message":"User created successfully"}
```

---
## Login and Token Example
**Request**
```bash
curl -X POST -H "Content-Type: application/json" -d '{"username":"trent","password":"test123"}' http://localhost/api/login.php
```
**Response**
```json
{"token":"a7c8e9d4f2..."}
```

---
## Using Bearer Token

```bash
curl -H "Authorization: Bearer a7c8e9d4f2..." http://localhost/api/profile.php
```


---
## Add a Venue
**Request**
```bash
curl -X POST -H "Content-Type: application/json" -H "Authorization: Bearer TOKEN" -d '{"name":"Downtown Arena","city":"Orlando"}' http://localhost/api/venues.php
```
**Response**
```json
{"message":"Venue added","id":1}
```

---
## List All Venues
```bash
curl http://localhost/api/venues.php
```
**Response**
```json
[{"id":1,"name":"Downtown Arena","city":"Orlando"}]
```

---
## Add a Concert
**Request**
```bash
curl -X POST -H "Content-Type: application/json" -H "Authorization: Bearer TOKEN" -d '{"title":"Rock Night","venue_id":1,"date":"2025-11-01","price":35.00}' http://localhost/api/concerts.php
```
**Response**
```json
{"message":"Concert created","id":1}
```

---
## List Concerts
```bash
curl http://localhost/api/concerts.php
```
**Response**
```json
[{"id":1,"title":"Rock Night","venue":"Downtown Arena","price":35.00}]
```

---
## Book a Concert
**Request**
```bash
curl -X POST -H "Content-Type: application/json" -H "Authorization: Bearer TOKEN" -d '{"concert_id":1}' http://localhost/api/bookings.php
```
**Response**
```json
{"message":"Booking confirmed","id":1}
```

---
## List My Bookings
```bash
curl -H "Authorization: Bearer TOKEN" http://localhost/api/bookings.php
```
**Response**
```json
[{"concert":"Rock Night","date":"2025-11-01"}]
```

---
## Health Check
```bash
curl http://localhost/api/health.php
```
**Response**
```json
{"status":"ok","time":"2025-10-15T12:00:00Z"}
```
---

# 🗄️ SQL Database Schema

- Database Name: **concerts_db**
- User: **root**
- Tables:
  - `venues` — stores venue information
  - `concerts` — stores concert details
  - `tickets` — stores ticket data linked to concerts

---


## Testing the API
- **HTML Test Page:** `code/html_tests/html_test.html`  
- **cURL Script:** `code/curl_tests/cURL_test.sh`  





---
## NGINX Deployment (Tutorial)
1. Copy `/api` folder to `/var/www/html/combsProject1`  
2. Edit NGINX config:
   ```bash
   server {
       listen 8081;
       root /var/www/html;
       index index.php;
       location ~ \.php$ {
           include snippets/fastcgi-php.conf;
           fastcgi_pass unix:/run/php/php8.3-fpm.sock;
       }
   ```
3. Restart NGINX and visit `http://localhost:8081/venues.php`

---

## Thank You

