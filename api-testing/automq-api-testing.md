# AutoMQ API Testing Report

**Tester:** Meraj Alam (@merajalamwork-hue)
**Platform:** AutoMQ Playground (playground.automq.cloud)
**Tool:** Postman
**Date:** 20 May 2026

---

## 🎯 Objective
To test the AutoMQ Cloud REST API endpoints for
correct functionality, authentication behavior,
and error handling.

---

## 🔐 Authentication Testing

### Test 1 — Valid Credentials on Playground
| Field | Details |
|-------|---------|
| Endpoint | GET /api/v1/instances |
| Auth | Valid AccessKey + SecretKey |
| Expected | 200 OK |
| Actual | 200 OK ✅ |
| Result | PASS |

### Test 2 — Invalid Credentials on Playground
| Field | Details |
|-------|---------|
| Endpoint | GET /api/v1/instances |
| Auth | @@@@@@@@ + @@@@@@@@ |
| Expected | 401 Unauthorized |
| Actual | 200 OK |
| Result | ⚠️ Needs Clarification |
| Note | Playground returns 200 OK with invalid 
credentials. Requires confirmation from 
maintainers whether this is intentional 
for demo purposes or a security oversight. |

### Test 3 — No Auth on Playground
| Field | Details |
|-------|---------|
| Endpoint | GET /api/v1/instances |
| Auth | None |
| Expected | 401 Unauthorized |
| Actual | 200 OK |
| Result | ⚠️ Needs Clarification |
| Note | Playground returns 200 OK with zero 
authentication. Same as Test 2 — 
awaiting maintainer confirmation. |

### Test 4 — No Auth on Real Console
| Field | Details |
|-------|---------|
| Endpoint | GET /api/v1/instances |
| Base URL | console.automq.cloud |
| Auth | None |
| Expected | 401 Unauthorized |
| Actual | 401 Unauthorized ✅ |
| Result | PASS — Real auth works correctly |

### Test 5 — Playground Keys on Console
| Field | Details |
|-------|---------|
| Endpoint | GET /api/v1/instances |
| Base URL | console.automq.cloud |
| Auth | Playground AccessKey + SecretKey |
| Expected | 401 Unauthorized |
| Actual | 401 Unauthorized ✅ |
| Result | PASS — Keys correctly isolated per environment |

---

## 📊 Endpoint Testing

### Test 6 — List Instances
| Field | Details |
|-------|---------|
| Method | GET |
| Endpoint | /api/v1/instances |
| Expected | 200 JSON list of instances |
| Actual | 200 OK — 5 instances returned ✅ |
| Result | PASS |

**Response Fields Verified:**
- pageNum ✅
- pageSize ✅
- total ✅
- list of instances with instanceId, name, state ✅

---

### Test 7 — List Topics
| Field | Details |
|-------|---------|
| Method | GET |
| Endpoint | /api/v1/instances/{instanceId}/topics |
| Expected | 200 JSON list of topics |
| Actual | 200 OK — topics returned ✅ |
| Result | PASS |

---

### Test 8 — List Consumer Groups
| Field | Details |
|-------|---------|
| Method | GET |
| Endpoint | /api/v1/instances/{instanceId}/groups |
| Expected | 200 JSON list of groups |
| Actual | 200 OK ✅ |
| Result | PASS |

---

## ⚠️ Findings & Observations

### Finding 1 — Invalid Instance ID Returns HTML
| Field | Details |
|-------|---------|
| Endpoint | /api/v1/instances/fakeid123/topics |
| Expected | 404 JSON error response |
| Actual | 200 OK with HTML page |
| Type | API Design Issue |
| Severity | Low |
| Recommendation | Return proper JSON error response for invalid IDs |

### Finding 2 — Wrong Endpoint Returns HTML
| Field | Details |
|-------|---------|
| Endpoint | /api/v1/instances/{id}/consumer-groups |
| Expected | 404 JSON error |
| Actual | 200 OK with HTML (SPA fallback) |
| Type | API Design Issue |
| Severity | Low |
| Recommendation | Return JSON 404 for unknown API routes |

---

## 📚 Key Learnings

- AutoMQ Playground is intentionally open —
  no auth required by design
- Real console (console.automq.cloud) correctly
  enforces authentication
- Service account keys are isolated per environment
- Topics and Groups are sub-resources of Instances
- Unknown routes return SPA HTML fallback
  instead of JSON 404

---

## ✅ API Endpoints Discovered

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/v1/instances | List all instances |
| GET | /api/v1/instances/{id}/topics | List topics |
| GET | /api/v1/instances/{id}/groups | List consumer groups |


## 🔄 Pending Confirmations

### Auth Behavior on Playground
- GitHub Issue: #3365
- Question: Is Playground API intentionally 
  open without authentication?
- Status: Awaiting maintainer response
- Date Asked: 20 May 2026
- Link: github.com/AutoMQ/automq/issues/3365
