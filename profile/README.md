# 🐵 OpenApe

**Security layer for AI agents on the Agentic Web.**

OpenApe provides identity, authentication, and privilege management for autonomous AI agents — built on open standards (WebAuthn/FIDO2, OIDC, DNS).

## Why OpenApe?

As AI agents become autonomous participants on the web, they need the same security infrastructure humans have — but adapted for machine-to-machine interaction. OpenApe is both **Enabler** (legitimate agents get identity and access) and **Gatekeeper** (unauthorized bots are blocked).

- **Passkeys-only authentication** (WebAuthn/FIDO2) — no passwords, NIS2 compliant
- **DDISA DNS discovery** — find any domain's identity provider via `_ddisa.` TXT records
- **Grant-based privilege elevation** — agents request permissions, humans approve (`once` / `ttl` / `always`)
- **Dual accountability** — every action traces back to both agent owner and human approver
- **`act` claim in JWT** — SPs can explicitly distinguish `human` vs `agent` actors

## Packages

### Core

| Package | npm | Description |
|---|---|---|
| [`@openape/core`](https://github.com/openape-ai/core) | [![npm](https://img.shields.io/npm/v/@openape/core)](https://www.npmjs.com/package/@openape/core) | DNS discovery, crypto, JWT, PKCE utilities |
| [`@openape/auth`](https://github.com/openape-ai/auth) | [![npm](https://img.shields.io/npm/v/@openape/auth)](https://www.npmjs.com/package/@openape/auth) | WebAuthn + OIDC for Identity & Service Providers |
| [`@openape/grants`](https://github.com/openape-ai/grants) | [![npm](https://img.shields.io/npm/v/@openape/grants)](https://www.npmjs.com/package/@openape/grants) | Grant lifecycle, approval flows & AuthZ-JWT |

### Nuxt Modules (Drop-in)

| Package | npm | Description |
|---|---|---|
| [`@openape/nuxt-auth-idp`](https://github.com/openape-ai/nuxt-auth-idp) | [![npm](https://img.shields.io/npm/v/@openape/nuxt-auth-idp)](https://www.npmjs.com/package/@openape/nuxt-auth-idp) | Identity Provider module |
| [`@openape/nuxt-auth-sp`](https://github.com/openape-ai/nuxt-auth-sp) | [![npm](https://img.shields.io/npm/v/@openape/nuxt-auth-sp)](https://www.npmjs.com/package/@openape/nuxt-auth-sp) | Service Provider module |
| [`@openape/nuxt-grants`](https://github.com/openape-ai/nuxt-grants) | [![npm](https://img.shields.io/npm/v/@openape/nuxt-grants)](https://www.npmjs.com/package/@openape/nuxt-grants) | Grant management UI & API |

### Tools

| Package | Description |
|---|---|
| [`openape-sudo`](https://github.com/openape-ai/sudo) | Privilege elevation CLI — agents request root via grant approval (Rust) |
| [`openape-proxy`](https://github.com/openape-ai/proxy) | Application-level HTTP proxy — grant-based access control for agent traffic |

## Get Started

**→ [examples](https://github.com/openape-ai/examples)** — Full working IdP + SP with deployment guide

**→ [docs.openape.at](https://docs.openape.at)** — Documentation

**→ [openape.at](https://openape.at)** — Project website

## Quick Example

```bash
# Install the Nuxt modules
pnpm add @openape/nuxt-auth-idp  # for your Identity Provider
pnpm add @openape/nuxt-auth-sp   # for your Service Provider
pnpm add @openape/nuxt-grants    # for grant management
```

```ts
// nuxt.config.ts (IdP)
export default defineNuxtConfig({
  modules: ['@openape/nuxt-auth-idp', '@openape/nuxt-grants'],
  openapeIdp: {
    rpID: 'id.example.com',
    rpOrigin: 'https://id.example.com',
    issuer: 'https://id.example.com',
  }
})
```

```ts
// nuxt.config.ts (SP)
export default defineNuxtConfig({
  modules: ['@openape/nuxt-auth-sp'],
  openapeSp: {
    spId: 'sp.example.com',
    // Leave empty for DDISA DNS discovery:
    openapeUrl: '',
  }
})
```

## Architecture

```
User (Passkey)  ──►  Identity Provider  ◄──  DDISA DNS (_ddisa.example.com)
                           │
                     JWT (sub, act, iss)
                           │
                    Service Provider  ◄──►  Agent (Ed25519)
                           │
                      Grant Request
                           │
                    Human Approval  ──►  AuthZ-JWT (once/ttl/always)
```

## Standards & Compliance

- **WebAuthn/FIDO2** — Passwordless authentication
- **OIDC** — Authorization code flow with PKCE
- **DDISA** — DNS-based Identity Discovery for the Agentic Web
- **NIS2** (EU), **NIST CSF 2.0**, **EO 14028** (US) — Regulatory compliance

## License

[MIT](https://github.com/openape-ai/examples/blob/main/LICENSE)
