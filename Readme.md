ZAN — Premium Sunshades
A modern e-commerce skeleton for ZAN, a premium sunshade atelier. Dark-mode-first with glassmorphism, cinematic Hero, AppShell navigation, and an Express + SQLite (Drizzle ORM) backend.

Stack

Runtime: Bun (used for both frontend tooling and backend)
Frontend: React 18 + Vite
UI: Mantine v7 (only) + @tabler/icons-react (only)
State: Zustand (cart persisted to localStorage, auth, UI)
Routing: React Router v6
Backend: Express, served by Bun
Database: SQLite via Bun's native bun:sqlite
ORM: Drizzle ORM (drizzle-orm/bun-sqlite) + Drizzle Kit
Project structure

.
├── server/
│   ├── db/
│   │   ├── schema.js         Drizzle schema (products, users, orders)
│   │   ├── index.js          DB client
│   │   ├── migrate.js        Runs Drizzle migrations
│   │   └── seed.js           Seeds the catalog
│   └── index.js              Express API
├── src/
│   ├── components/
│   │   ├── ui/               Atoms (GlassSurface, GoldText, Logo)
│   │   ├── common/           Layout, Header, Footer, MobileNav
│   │   └── features/         Hero, ProductCard, CartDrawer, AuthModal, …
│   ├── pages/                HomePage, ShopPage, ProductPage, CartPage, AboutPage
│   ├── store/                useCart.js, useAuth.js, useUI.js
│   ├── theme/                Mantine theme override + global.css
│   ├── lib/api.js            Frontend API client (graceful fallback)
│   ├── data/products.js      Local fallback catalog
│   ├── App.jsx               Routes
│   └── main.jsx              Entry
├── drizzle.config.js
├── vite.config.js
└── postcss.config.cjs
Setup

# 1. install dependencies
bun install
1. Run the Drizzle migrations to set up the SQLite DB

The SQLite file lives at server/db/zan.db. Generate migrations from the schema, then apply them:

# generate the SQL migration files into server/db/migrations
bun run db:generate

# apply migrations to the SQLite database
bun run db:migrate

# (optional) seed the catalog with a few sample products
bun run db:seed
2. Start the backend server

bun run server
# → ZAN API ready on http://localhost:4000
Endpoints:

GET / — service banner
GET /api/health
GET /api/products
GET /api/products/:id
POST /api/orders
Note: hitting http://localhost:4000/ directly returns the service banner. The actual storefront is served by Vite at http://localhost:5173.
3. Start the frontend dev environment

In a second terminal:

bun run dev
# → http://localhost:5173
The Vite dev server proxies /api/* to http://localhost:4000, so you can run both side-by-side. If the API is offline, the storefront still renders using the local fallback catalog in src/data/products.js.

Notes

Dark mode is forced at the Mantine provider level — the entire experience is designed dark-first.
The cart is persisted to localStorage under the zan-cart key.
Glassmorphism utilities (.zan-glass, .zan-glass-strong, .zan-gold-text) live in src/theme/global.css.
Only Mantine components and @tabler/icons-react icons are used in the UI.
Roadmap

Admin dashboard (deliberately not built yet — the brief is to polish the customer-facing GUI first).
Auth backend (currently stub: AuthModal writes to the useAuth Zustand store; wire to /api/auth/* next).
Checkout flow + Stripe.