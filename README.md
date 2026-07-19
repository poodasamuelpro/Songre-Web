# SONGRE Web — Application Web + BFF Sécurisé

Ce dépôt contient **3 composants** de la plateforme web SONGRE :

## 📁 Structure du dépôt

```
Songre-Web/
├── web-app/              # Build Flutter Web (déployable directement)
│   ├── index.html        # Point d'entrée
│   ├── flutter.js        # Runtime Flutter Web
│   ├── main.dart.js      # App compilée
│   └── assets/           # Fonts, images, icônes
│
├── bff-cloudflare/       # BFF Cloudflare Workers (TypeScript)
│   ├── src/
│   │   ├── auth/         # login, signup, logout, refresh, recover
│   │   ├── proxy/        # Proxy Supabase authentifié
│   │   ├── security/     # CSRF, headers, CSP
│   │   └── session/      # Gestion sessions KV
│   ├── wrangler.toml     # Config déploiement Cloudflare
│   └── package.json
│
├── bff-vercel/           # BFF Vercel Serverless (TypeScript)
│   ├── api/bff/          # Routes serverless
│   ├── lib/              # Session Upstash, CSRF, headers
│   └── vercel.json       # Config déploiement Vercel
│
├── GUIDE_WEB_BFF_MAJ.md              # Guide déploiement complet (684 lignes)
└── AUDIT_SECURITE_PENTEST_COMPLET.md # Rapport sécurité OWASP/CVSS
```

## 🚀 Déploiement rapide

### Option A — Cloudflare Workers
```bash
cd bff-cloudflare
npm install
wrangler secret put SESSION_SECRET
wrangler secret put CSRF_SECRET
wrangler secret put SUPABASE_URL
wrangler secret put SUPABASE_ANON_KEY
wrangler deploy
```

### Option B — Vercel
```bash
cd bff-vercel
npm install
vercel env add UPSTASH_REDIS_REST_URL
vercel env add UPSTASH_REDIS_REST_TOKEN
vercel env add SESSION_SECRET
vercel env add CSRF_SECRET
vercel --prod
```

### Déployer web-app/
Copier le contenu de `web-app/` sur n'importe quel hébergeur statique :
- Cloudflare Pages : `wrangler pages deploy web-app/`
- Vercel : `vercel deploy web-app/`
- Nginx : copier dans `/var/www/html/`

## 🔒 Sécurité
- Tokens Supabase jamais exposés au navigateur
- Cookies HttpOnly + Secure + SameSite=Strict
- CSRF double-submit HMAC-SHA256
- Rate limiting : 5 req/60s par IP
- CSP stricte avec `wasm-unsafe-eval` pour Flutter WASM

## 📱 Application mobile
Voir le dépôt principal : [poodasamuelpro/Songre-app](https://github.com/poodasamuelpro/Songre-app)
