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

---

### Test 2 — Invalid Credentials on Playground
| Field | Details |
|-------|---------|
| Endpoint | GET /api/v1/instances |
| Auth | @@@@@@@@ + @@@@@@@@ |
| Expected | 401 Unauthorized |
| Actual | 200 OK |
| Result | ✅ Confirmed — Intentional by Design |
| Note | Confirmed by AutoMQ contributor @johnluoyx: Playground is intentionally designed as an open demo environment. All data is mock/demo only and fully isolated from production systems. |
| Reference | github.com/AutoMQ/automq/issues/3365 |

---

### Test 3 — No Auth on Playground
| Field | Details |
|-------|---------|
| Endpoint | GET /api/v1/instances |
| Auth | None |
| Expected | 401 Unauthorized |
| Actual | 200 OK |
| Result | ✅ Confirmed — Intentional by Design |
| Note | Same as Test 2 — confirmed intentional. Playground APIs return 200 OK even with no credentials as it is a fully open demo environment. |
| Reference | github.com/AutoMQ/automq/issues/3365 |

---

### Test 4 — No Auth on Real Console
| Field | Details |
|-------|---------|
| Endpoint | GET /api/v1/instances |
| Base URL | console.automq.cloud |
| Auth | None |
| Expected | 401 Unauthorized |
| Actual | 401 Unauthorized ✅ |
| Result | PASS — Real auth works correctly |

---

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

---

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

### Finding 3 — Playground Authentication Behavior
Not Documented in Official Docs
| Field | Details |
|-------|---------|
| Type | Documentation Gap |
| Severity | Low |
| Details | Playground API is intentionally open without authentication. However this behavior is not mentioned anywhere in the official documentation at docs.automq.com. New users and testers may confuse this with a security oversight. |
| Suggestion | Add a note in Playground docs explaining that no authentication is required and all data is mock/demo only. |
| Confirmed By | @johnluoyx (AutoMQ Contributor) |
| Reference | github.com/AutoMQ/automq/issues/3365 |
| Status | 💡 Suggestion shared with maintainers |

---

## 📚 Key Learnings
- AutoMQ Playground is intentionally open —
  no auth required by design (confirmed by maintainer)
- All Playground data is mock/demo —
  fully isolated from production systems
- Real console (console.automq.cloud) correctly
  enforces authentication
- Service account keys are isolated per environment
- Topics and Groups are sub-resources of Instances
- Unknown routes return SPA HTML fallback
  instead of JSON 404
- Always confirm behavior with maintainers
  before documenting assumptions
- Always check documentation before suggesting
  improvements

---

## ✅ API Endpoints Discovered

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/v1/instances | List all instances |
| GET | /api/v1/instances/{id}/topics | List topics |
| GET | /api/v1/instances/{id}/groups | List consumer groups |

---

## ✅ Confirmed Findings

### Auth Behavior on Playground
| Field | Details |
|-------|---------|
| GitHub Issue | #3365 |
| Question | Is Playground API intentionally open without authentication? |
| Status | ✅ Confirmed by @johnluoyx (AutoMQ Contributor) |
| Answer | Intentionally open by design — demo environment |
| Date Confirmed | 20 May 2026 |
| Link | github.com/AutoMQ/automq/issues/3365 |
