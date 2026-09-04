# 🔗 Trimrr — URL Shortener

Trimrr is a full-stack URL shortener built with React and Supabase. Paste in a long URL, get a short, shareable link with an auto-generated QR code, and track clicks by location and device — all from a personal dashboard.

![Trimrr banner](public/banner1.jpg)

## ✨ Features

- **Instant URL shortening** — turn any long URL into a short `trimrr.in/xxxxx` link
- **Custom aliases** — optionally pick your own custom short link instead of a random one
- **QR codes** — every short link gets an auto-generated, downloadable QR code
- **Click analytics** — track total clicks, visitor location (city/country), and device type (mobile/desktop) per link
- **Authentication** — email/password sign up and login (with profile picture) powered by Supabase Auth
- **Personal dashboard** — search/filter your links, view stats, copy links, or delete them
- **Responsive UI** — built with Tailwind CSS and shadcn/ui components

## 🛠️ Tech Stack

**Frontend**
- [React 18](https://react.dev/) + [Vite](https://vitejs.dev/)
- [React Router v7](https://reactrouter.com/) for routing
- [Tailwind CSS](https://tailwindcss.com/) + [shadcn/ui](https://ui.shadcn.com/) (Radix primitives) for styling/components
- [Recharts](https://recharts.org/) for analytics charts
- [react-qrcode-logo](https://www.npmjs.com/package/react-qrcode-logo) for QR code generation
- [ua-parser-js](https://www.npmjs.com/package/ua-parser-js) for device detection
- [Yup](https://github.com/jquense/yup) for form validation

**Backend / Infrastructure**
- [Supabase](https://supabase.com/) — Postgres database, Auth, and Storage (for QR codes and profile pictures)
- [ipapi.co](https://ipapi.co/) — IP-based geolocation for click tracking

## 📂 Project Structure

```
src/
├── components/       # UI components (create-link, link-card, login, signup, stats, etc.)
│   └── ui/           # shadcn/ui primitives (button, card, input, dialog, tabs, ...)
├── context.jsx        # Global auth/user context (UrlProvider / UrlState)
├── db/                 # Supabase client + API helpers
│   ├── supabase.js     # Supabase client setup
│   ├── apiAuth.js       # Login, signup, logout, session
│   ├── apiUrls.js       # Create/fetch/delete short URLs
│   └── apiClicks.js     # Record and fetch click analytics
├── hooks/
│   └── use-fetch.js     # Generic async data-fetching hook
├── layouts/
│   └── app-layout.jsx   # Shared layout (header + outlet)
├── pages/
│   ├── landing.jsx       # Home page with the shorten form
│   ├── auth.jsx          # Login/signup page
│   ├── dashboard.jsx     # Authenticated user's links dashboard
│   ├── link.jsx          # Single link detail + analytics page
│   └── redirect-link.jsx # Resolves a short URL and redirects
└── lib/utils.js         # Shared utility helpers
```

## 🚀 Getting Started

### Prerequisites

- Node.js (v18+ recommended)
- npm or yarn
- A free [Supabase](https://supabase.com/) project

### 1. Clone the repository

```bash
git clone https://github.com/aadeshvish15/URL_shotener.git
cd URL_shotener
```

### 2. Install dependencies

```bash
npm install
# or
yarn install
```

### 3. Set up Supabase

Create a new project at [supabase.com](https://supabase.com/) and set up:

**Database — a `urls` table** with columns such as:

| Column         | Type      | Notes                        |
| -------------- | --------- | ---------------------------- |
| `id`           | uuid      | primary key                  |
| `created_at`   | timestamp | default `now()`              |
| `title`        | text      |                               |
| `user_id`      | uuid      | references `auth.users`      |
| `original_url` | text      | the long URL                 |
| `custom_url`   | text      | nullable                     |
| `short_url`    | text      | auto-generated short code    |
| `qr`           | text      | public URL of the QR image   |

**Database — a `clicks` table** with columns such as:

| Column       | Type      | Notes                          |
| ------------ | --------- | ------------------------------- |
| `id`         | uuid      | primary key                     |
| `created_at` | timestamp | default `now()`                 |
| `url_id`     | uuid      | references `urls.id`            |
| `city`       | text      |                                  |
| `country`    | text      |                                  |
| `device`     | text      | e.g. `mobile` / `desktop`       |

**Storage — two public buckets:**
- `qrs` — stores generated QR code images
- `profile_pic` — stores user profile pictures

### 4. Configure environment variables

Create a `.env` file in the project root:

```env
VITE_SUPABASE_URL=your-supabase-project-url
VITE_SUPABASE_KEY=your-supabase-anon-key
```

### 5. Run the app

```bash
npm run dev
```

The app will be available at `http://localhost:5173`.

### Other scripts

```bash
npm run build     # production build
npm run preview   # preview the production build locally
npm run lint      # run ESLint
```

## 🧭 App Routes

| Route         | Description                                  |
| ------------- | --------------------------------------------- |
| `/`           | Landing page with the "shorten a URL" form    |
| `/auth`       | Login / sign up                               |
| `/dashboard`  | Authenticated user's links (protected)        |
| `/link/:id`   | Analytics for a single link (protected)       |
| `/:id`        | Resolves a short/custom URL and redirects     |

## 🤝 Contributing

Contributions, issues, and feature requests are welcome. Feel free to fork the repo and open a pull request.

## 📄 License

No license has been specified for this project yet. Consider adding one (e.g. MIT) if you plan to share or accept contributions.

