# API Response Pattern

## Purpose
Defines the standard API response shape for all endpoints.

## Use When
- Any Express-based API in this playbook

## Rules
- All successful API responses use a consistent response wrapper.
- Collections always include count metadata.
- Paginated responses include `total`, `page`, and `pageSize`.
- Never return raw arrays at the top level — always wrap.

## Patterns

Standard response shape:

```ts
{
  success: boolean;
  records: T;
  meta: {
    count: number;
    total?: number;
    page?: number;
    pageSize?: number;
  };
}
```

Single record example:
```json
{
  "success": true,
  "records": { "id": 1, "name": "Acme" },
  "meta": { "count": 1 }
}
```

Collection example:
```json
{
  "success": true,
  "records": [{ "id": 1 }, { "id": 2 }],
  "meta": { "count": 2, "total": 50, "page": 1, "pageSize": 20 }
}
```

Error response (handled by middleware):
```json
{
  "success": false,
  "error": {
    "code": "NOT_FOUND",
    "message": "User not found."
  }
}
```

## Anti-Patterns
- Returning raw arrays: `res.json([...])`.
- Inconsistent field names across endpoints (`data` vs `records` vs `result`).
- Omitting `success` field — it enables simple client-side branching.

## Related Files
- backend/express-sequelize.md
- backend/request-error-pattern.md
