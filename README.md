# @yaghmori/messaging-contracts

![stable](https://img.shields.io/badge/status-stable-brightgreen)

**Shared** Zod helpers / cross-cutting schemas. Prefer **per-service client SDKs** for app integration:

| Service | npm client | Docker |
|---------|------------|--------|
| email | [`@yaghmori/email-service`](https://www.npmjs.com/package/@yaghmori/email-service) | `ghcr.io/yaghmori/email-service` |
| storage | [`@yaghmori/storage-service`](https://www.npmjs.com/package/@yaghmori/storage-service) | `ghcr.io/yaghmori/storage-service` |

Those packages ship ports, TCP patterns, Kafka topics, and Nest helpers so consumers never hardcode event names.

## Install (shared layer)

```bash
pnpm add @yaghmori/messaging-contracts zod
```

Optional Nest helpers (interceptors / middleware / RPC filters):

```ts
import { RequestContextMiddleware, RpcLoggingInterceptor } from '@yaghmori/messaging-contracts/nest';
```

## Integrate in 10 minutes

1. Import constants instead of hard-coding topics/ports:

```ts
import {
  SERVICE_PORTS,
  KAFKA_TOPICS,
  MESSAGE_PATTERNS,
  sendEmailRequestSchema,
} from '@yaghmori/messaging-contracts';
```

2. Publish email send requests on `KAFKA_TOPICS.EMAIL_SEND_REQUESTED` with payload matching `EmailSendRequestedPayloadSchema`, **or** call TCP `MESSAGE_PATTERNS.EMAIL.SEND_EMAIL` on port `SERVICE_PORTS.EMAIL_SERVICE_TCP`.

3. Upload assets via storage HTTP; use TCP `MESSAGE_PATTERNS.STORAGE.*` on port `SERVICE_PORTS.STORAGE_SERVICE_TCP` for metadata / signed URL / delete.

## Explicit non-goals

- No Allyfe / Parslinks product domain types
- Notification workflow catalogs belong in consumer apps (see notification Phase B)
- Deployment artifact registry is **not** part of storage V1

## License

MIT
