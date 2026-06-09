# AutoMQ API Testing Report

**Tester:** Meraj Alam (@merajalamwork-hue)
**Platform:** AutoMQ Playground (playground.automq.cloud)
**Tool:** Postman
**Dates:** 20–21 May 2026

---

## 🎯 Objective
To test the AutoMQ Cloud REST API endpoints for correct functionality,
authentication behavior, and error handling.

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

## 📊 Endpoint Testing (Day 1 — 20 May 2026)

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

## 📊 Endpoint Testing (Day 2 — 21 May 2026)

### Test 9 — Get Single Instance Detail
| Field | Details |
|-------|---------|
| Method | GET |
| Endpoint | /api/v1/instances/kf-cezs05iok5xreimol |
| Expected | 200 JSON with full instance object |
| Actual | 200 OK ✅ |
| Result | PASS |

**Response Fields Verified:**
- instanceId, name, state ✅
- statistics (topicCount, partitionCount, consumerGroupCount) ✅
- spec (nodeConfig, networks, dataBuckets) ✅
- features (walMode, security, metricsExporter) ✅

---

### Test 10 — Invalid Instance ID
| Field | Details |
|-------|---------|
| Method | GET |
| Endpoint | /api/v1/instances/fakeid123 |
| Expected | 404 JSON error |
| Actual | 404 JSON with error code ✅ |
| Result | PASS |

**Note:** Yesterday this returned `200 OK + HTML`. Today returns proper `404 + JSON`. Bug confirmed fixed by AutoMQ team. ✅

---

### Test 11 — Invalid Instance ID with Sub-resource
| Field | Details |
|-------|---------|
| Method | GET |
| Endpoint | /api/v1/instances/fakeid123/topics |
| Expected | 404 JSON error |
| Actual | 404 JSON with error code ✅ |
| Result | PASS |

**Note:** This was Finding 1 from Day 1 — now fixed. ✅

---

### Test 12 — Wrong Endpoint (/consumer-groups)
| Field | Details |
|-------|---------|
| Method | GET |
| Endpoint | /api/v1/instances/{id}/consumer-groups |
| Expected | 404 JSON error (wrong endpoint) |
| Actual | 200 OK — returns valid consumer group data |
| Result | ⚠️ Observation — Undocumented Alias |

**Note:** Both `/groups` and `/consumer-groups` return the same data. This is an undocumented route alias not mentioned in official docs.

---

### Test 13 — Pagination (pageSize=2)
| Field | Details |
|-------|---------|
| Method | GET |
| Endpoint | /api/v1/instances?pageNum=1&pageSize=2 |
| Expected | 200 with 2 items, total=5, totalPage=3 |
| Actual | 200 OK — exactly as expected ✅ |
| Result | PASS |

---

### Test 14 — Out of Range Page (pageNum=999)
| Field | Details |
|-------|---------|
| Method | GET |
| Endpoint | /api/v1/instances?pageNum=999&pageSize=10 |
| Expected | 200 with empty list |
| Actual | 200 OK — empty list, total=5 ✅ |
| Result | PASS |

---

### Test 15 — Invalid pageSize (String)
| Field | Details |
|-------|---------|
| Method | GET |
| Endpoint | /api/v1/instances?pageNum=1&pageSize=abc |
| Expected | 400 with clean error message |
| Actual | 400 — but exposes java.lang.String internally |
| Result | 🔴 BUG — Reported as Issue #3366 |

---

### Test 16 — Invalid pageSize (Negative)
| Field | Details |
|-------|---------|
| Method | GET |
| Endpoint | /api/v1/instances?pageNum=1&pageSize=-1 |
| Expected | 400 with clean error message |
| Actual | 400 — exposes internal fromIndex/toIndex terms |
| Result | 🔴 BUG — Added to Issue #3366 |

---

### Test 17 — Invalid pageNum (String)
| Field | Details |
|-------|---------|
| Method | GET |
| Endpoint | /api/v1/instances?pageNum=abc&pageSize=10 |
| Expected | 400 with clean error message |
| Actual | 400 — exposes java.lang.String internally |
| Result | 🔴 BUG — Added to Issue #3366 |

---

### Test 18 — No Query Params (Default Values)
| Field | Details |
|-------|---------|
| Method | GET |
| Endpoint | /api/v1/instances |
| Expected | 200 with sensible defaults |
| Actual | 200 OK — defaults to pageNum=1, pageSize=10 ✅ |
| Result | PASS |

---

## ✅ Bugs Fixed Since Day 1

| Finding | Day 1 | Day 2 |
|---------|-------|-------|
| Invalid ID returns HTML | 200 + HTML ❌ | 404 + JSON ✅ Fixed |
| Invalid sub-resource returns HTML | 200 + HTML ❌ | 404 + JSON ✅ Fixed |

---

## ⚠️ Findings & Observations (Day 2)

### Finding 1 — Internal Java Details Leaked in Error Messages
| Field | Details |
|-------|---------|
| Endpoints | GET /api/v1/instances with invalid integer params |
| Type | Security — Improper Error Handling |
| Severity | Low |
| GitHub Issue | #3366 |
| Status | 🔴 Open — Awaiting maintainer confirmation |
| Reference | OWASP Improper Error Handling, CWE-248 |
| Details | When invalid values are passed to integer query parameters (pageSize, pageNum), the API exposes internal Java/Spring class names (java.lang.String, fromIndex, toIndex) in error messages. These should never be visible to API consumers. |
| Recommendation | Replace raw Spring exception messages with clean user-friendly error messages. Log full details server-side only. |

---

### Finding 2 — Undocumented Route Alias
| Field | Details |
|-------|---------|
| Endpoint | /api/v1/instances/{id}/consumer-groups |
| Type | Documentation Gap |
| Severity | Low |
| Details | Both /groups and /consumer-groups return identical data. This alias is not mentioned in official documentation and may cause confusion. |
| Recommendation | Document the alias or remove it to avoid ambiguity. |

---

### Finding 3 — pageSize=0 Accepted Silently
| Field | Details |
|-------|---------|
| Endpoint | GET /api/v1/instances?pageNum=1&pageSize=0 |
| Type | API Validation Issue |
| Severity | Low |
| Details | Passing pageSize=0 returns 200 OK with empty list instead of 400 Bad Request. Zero is not a valid page size and should be rejected with a clear error. |
| Recommendation | Validate pageSize > 0 and return 400 with a clear message. |

---

## ✅ API Endpoints Discovered

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/v1/instances | List all instances |
| GET | /api/v1/instances/{id} | Get single instance detail |
| GET | /api/v1/instances/{id}/topics | List topics |
| GET | /api/v1/instances/{id}/groups | List consumer groups |
| GET | /api/v1/instances/{id}/consumer-groups | Undocumented alias for /groups |

---

## 📚 Key Learnings
- AutoMQ Playground is intentionally open — no auth required by design (confirmed by maintainer)
- All Playground data is mock/demo — fully isolated from production systems
- Real console (console.automq.cloud) correctly enforces authentication
- Service account keys are isolated per environment
- Topics and Groups are sub-resources of Instances
- API uses smart defaults: pageNum=1, pageSize=10 when no params provided
- totalPage is dynamically calculated based on pageSize per request
- Always confirm behavior with maintainers before documenting assumptions
- Always validate findings against official references (OWASP) before reporting

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

### Internal Error Message Leakage
| Field | Details |
|-------|---------|
| GitHub Issue | #3366 |
| Status |✅ Closed — Acknowledged by maintainer |
|Answer| Improvement planned for future release|
| Date Reported | 21 May 2026 |
| Link | github.com/AutoMQ/automq/issues/3366 |
