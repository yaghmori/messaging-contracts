# @yaghmori/messaging-contracts

![stable](https://img.shields.io/badge/status-stable-brightgreen)

Shared **ports**, **Kafka topics**, **TCP message patterns**, and **Zod DTOs** for public platform services:

| Service | TCP port | Role |
|---------|----------|------|
| email-service | **4003** | Transactional email (template key + data) |
| storage-service | **4002** | Platform object storage (avatars, docs, images) |
| notification-service | **4004** | In-app inbox + prefs + fan-out (Phase B) |

## Install

```bash
pnpm add @yaghmori/messaging-contracts zod
```

Published from **`main`** to [npmjs.com](https://www.npmjs.com/package/@yaghmori/messaging-contracts) via **CD (main)**.  
Requires npm user **`yaghmori`** + repo secret **`NPM_TOKEN`** (Automation / CI token with 2FA bypass).  
**`dev`** runs **CI (dev)** only (build/typecheck, no publish). Bump `version` in `package.json` before merging to `main` when you want a new release.

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
