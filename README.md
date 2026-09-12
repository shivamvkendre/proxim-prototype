# PROXIM

**Discover Opportunities Around You.**

> ⚠️ **This is a Design Thinking project and a functional high-fidelity prototype.**
> It is **not** a launched startup, a validated business, or a production recruitment
> system. It exists to test one hypothesis (below) and to make that idea
> understandable. Nothing here should be read as evidence of traction, revenue,
> partnerships, or confirmed hiring.

---

## The idea in one line

> Today, students search for **vacancies**. Proxim helps them discover **opportunities around them**.

A vacancy-first search only surfaces organizations that advertised. Proxim changes
the starting question from *"which companies are hiring?"* to
*"which relevant organizations are around me, and are they worth approaching?"*

## The problem

Students and freshers search LinkedIn, Naukri, Internshala, career pages, college
groups and personal networks. Those channels work, but a vacancy-first search can
miss organizations that have a small or temporary requirement, recruit through
networks, are open to student approaches, have an emerging need, or simply have not
advertised anything.

We call the distance between *what exists around a student* and *what they can
actually discover through normal vacancy search* the **Opportunity Visibility Gap**.
A related idea is the **Local Opportunity Gap** — students often live near many
organizations but know only a handful.

**What we deliberately do not claim:** that unadvertised organizations are hiring,
that every organization is a "hidden job", or any statistic about how many jobs are
hidden. No research findings are invented anywhere in this project.

## A simple example

A B.Com student in Electronic City wants an Excel, operations or analytics
opportunity within 5 km.

| Today | With Proxim |
|---|---|
| Searches *"operations internship Electronic City"*. If no suitable vacancy is listed, the search often ends. | Selects Electronic City → 5 km → sees nearby organizations → filters by category, skill or status → opens one → reads what is actually known → saves it or approaches it via a public route. |

Proxim does **not** promise anyone a job. It helps a student discover organizations
and decide whether approaching them makes sense.

---

## Core product principle: not a hiring / not-hiring binary

| Status | Meaning |
|---|---|
| 🟢 **Hiring** | A current opportunity is confirmed. |
| 🟡 **Open to Student Approaches** | The organization has confirmed relevant students can approach. |
| 🟠 **Potential Opportunity** | There is a signal or possibility — **not** a confirmed vacancy. |
| ⚪ **Status Unknown** | The organization exists, but no opportunity status has been confirmed. |

> **A map pin means "this organization exists here." It does not mean "this organization is hiring."**

## Trust and verification

Four layers, shown explicitly in the UI so information is never presented above the
confidence at which it was gathered:

1. **Discovered** — basic organization and location information has been found.
2. **Community Verified** — checked by a community contributor.
3. **Organization Verified** — the organization has claimed or confirmed its profile.
4. **Opportunity Verified** — a specific opportunity or status has been confirmed.

---

## Features

- **Map-based local discovery** with 1 / 3 / 5 / 10 / 25 km radius
- **Area presets** — Electronic City (default), Koramangala, HSR Layout, Whitefield, Bellandur, Indiranagar, Jayanagar
- **Search** across name, category, area, skills and opportunity type
- **Filters** for status, category, opportunity type, skill and verification — all genuinely narrow results
- **Map / Grid** views with map–list synchronisation and fly-to-selected
- **Organization profile** — status, trust tier, why it may be worth approaching, skills, types, public contact routes
- **Approach flow** — generates an editable outreach message, explicitly framed as an enquiry
- **Save** organizations, persisted locally
- **Student profile** — education, skills, interests, preferred types, radius and area
- **Add organization** and **Claim organization** prototype workflows
- **Resilient map** — falls back through tile providers, then to a schematic view, without breaking the app
- Responsive from 390 px mobile to desktop; keyboard-navigable with visible focus states

## Tech stack

| Layer | Choice | Why |
|---|---|---|
| Framework | React 19 + TypeScript | Types catch schema drift across a 23-field organization model |
| Build | Vite | Fast dev server, simple static output |
| Map | Leaflet + React-Leaflet + marker clustering | Mature, keyless, works on static hosting |
| Routing | React Router (`HashRouter`) | GitHub Pages has no server rewrites; hash routing survives refresh and deep links |
| State | React state + `localStorage` hooks | No backend needed for a prototype |

No backend, no authentication, no payment processing — deliberately out of scope.

## Run locally

```bash
npm install
npm run dev      # http://localhost:5173
```

Build and preview the production bundle:

```bash
npm run build
npm run preview
```

## Environment variables

**None.** This project uses only keyless map tile providers, so there are no secrets,
no `.env` file, and nothing to configure before running it. If you later swap in a
provider that requires a key (Mapbox, Google Maps, MapTiler), put it in `.env` as
`VITE_MAP_KEY`, read it via `import.meta.env.VITE_MAP_KEY`, and never commit the file —
`.env` is already in `.gitignore`.

## Map reliability

Map tiles are the least reliable part of any map app, so failure is handled in three
stages, configured in `src/map/tileProviders.ts`:

1. **Esri World Street Map** (default, keyless)
2. **OpenStreetMap** (fallback, keyless)
3. **Schematic fallback map** — real coordinates are still projected onto a plain
   canvas, so pins, radius, selection and filtering keep working with no tiles at all

The application never shows a broken map, and tile failure never blocks the rest of
the UI. Note that public tile servers often refuse requests from `file://` pages —
serve the build over HTTP (`npm run preview`) or deploy it.

## Deploy to GitHub Pages

`vite.config.ts` sets `base: './'`, so the build works under `/<repo>/` without
further configuration.

1. Push this repository to GitHub with the default branch named `main`.
2. In the repo: **Settings → Pages → Build and deployment → Source → GitHub Actions**.
3. Push to `main`. The included workflow (`.github/workflows/deploy.yml`) builds and
   publishes automatically.
4. Your demo URL will be `https://<username>.github.io/<repo>/`.

Any static host works equally well (Netlify, Vercel, Cloudflare Pages) — drop the
`dist/` folder in. No server-side runtime is required.

---

## Data disclaimers — please read

The dataset in `src/data/organizations.ts` holds **240 records of two clearly
separated kinds**, and the UI badges every organization with which kind it is:

**1. Real listings (24 records, Electronic City) — badge: "Real listing"**
Name, category, address and coordinates were sourced from public Google Maps data.
These are real businesses. **Their opportunity status is therefore always
"Status Unknown"**, because nobody has confirmed anything about their hiring
intentions. No contact details are stored for them — the only route offered is a link
to their public Google Maps listing. Nothing about their hiring is claimed, implied
or invented.

**2. Illustrative prototype data (216 records) — badge: "Illustrative data"**
These organizations are **fictional**, created to demonstrate the interface. Their
names, descriptions, statuses, skills and contact routes (all on `example.com`) are
invented. They exist so the four-status model and the filters can be demonstrated.
They are not real businesses and must never be presented as market evidence.

**No user research is claimed anywhere in this application.** There are no interview
counts, survey percentages, customer quotes, revenue figures, paying customers,
partnerships or traction metrics — because none have been validated.

## Business model — an untested hypothesis

Students use Proxim free. Organizations are the potential paying side, with a basic
listing free and paid features (enhanced profile, verification, better local
visibility, opportunity-status management) hypothesised at roughly **₹499/month or
₹6,000/year**.

> We believe organizations are more likely to be paying customers because they
> receive business value from visibility and access to relevant student talent.
> **This is an assumption we would need to validate.** Willingness to pay has not
> been tested, and no payment processing exists in this prototype.

## The hypothesis this prototype exists to test

> *Students may find it useful to discover relevant nearby organizations first and
> then evaluate whether those organizations are worth approaching, instead of relying
> only on advertised vacancies.*

## Project structure

```
src/
  components/   common/   badges, empty states, toast
                explore/  toolbar, filter panel, results list
                layout/   top bar
                org/      card, drawer, approach modal, claim modal
  data/         organizations.ts, statusModel.ts, areaPresets.ts, studentProfile.ts
  hooks/        usePersistedState, useSavedOrgs
  map/          MapView, FallbackMap, tileProviders
  pages/        Home, Explore, Saved, Profile, AddOrganization
  services/     orgService (filtering), approachService (message generation)
  styles/       tokens.css, global.css
  types.ts
public/assets/  proxim-logo.png
```

Filtering and message generation live in `services/` as pure functions, separate from
presentation, so they can be tested and changed independently of the UI.

## Licence

Coursework prototype. The Proxim name, logo and branding belong to the project author.
