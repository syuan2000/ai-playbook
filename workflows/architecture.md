# Architecture Workflow

> Think before you build the big thing. Right-size the decision — don't design a
> spaceship for a to-do app.

**Use when:** starting a new project, or making a decision that's expensive to
reverse later (data model, framework, auth, hosting, a core abstraction). For a
single feature, [feature.md](./feature.md) is enough.

**Solo-dev north star:** the best architecture is the simplest one that won't box
you in for the next few months. You can't predict year 3, so don't try to.

---

## 1. Frame the problem (PM + Architect hat)

- What is this project actually for? Who uses it, and roughly how many?
- What are the 3–5 core things it must do well? Ignore the rest for now.
- What are the real constraints? (Your time, budget, what you already know,
  hosting limits.) These decide more than any best practice.
- What's likely to change vs. stay fixed? Put your flexibility where change is
  likely; hardcode where it isn't.

## 2. Decide the shape

- **Boring by default.** Pick tech you know or that's well-documented. A new
  project is not the place to learn 4 new tools at once.
- **Start monolithic.** One app, one database. Do *not* reach for microservices,
  event buses, or k8s on a personal project — that's complexity you'll pay for
  and rarely need.
- Sketch the pieces: client, server, database, external services. One diagram
  (even hand-drawn) beats three paragraphs.
- Define the seams between pieces (APIs, module boundaries). These are what let
  you swap parts later.

## 3. The data model is the foundation

- Model the core entities and their relationships first — this is the hardest
  thing to change once there's real data.
- Get the nouns and their keys right. UI and endpoints can churn; schema migrations
  hurt.
- Plan for a migration path (even a simple one) from day one.

## 4. Cross-cutting decisions (decide once, early)

- **Auth**: use a library/service (Clerk, Auth0, Supabase, etc.) — don't roll your
  own. Decide this early; it touches everything.
- **Config & secrets**: env vars, never committed. One documented place.
- **Errors & logging**: a consistent way to surface and record failures.
- **State/data flow**: pick one pattern and apply it everywhere for consistency.

## 5. Write it down (a lightweight ADR)

For each significant decision, jot 4 lines somewhere in the repo (`/docs/decisions`):

```
# <decision>
Context:  what forced this choice
Decision: what I picked
Why:      the 1–2 reasons
Revisit:  what would make me change my mind
```

Future-you will forget *why*. This is 2 minutes now that saves an hour later.

## 6. Guard against over-engineering

Ask before every abstraction: **do I need this now, or am I guessing?**

- **YAGNI** — build for today's requirements, not imagined ones.
- Add abstraction when you feel the pain (2–3 real cases), not in anticipation.
- Prefer deleting code over adding config options.
- It's fine to leave `TODO: revisit when we have >X users`.

---

## Quick checklist

- [ ] Purpose, users, and the 3–5 core capabilities are clear
- [ ] Real constraints (time/skill/budget) named and respected
- [ ] Tech chosen is boring/known; not learning 4 new things at once
- [ ] Starting monolithic — no premature microservices/infra
- [ ] Core data model + relationships designed first
- [ ] Auth/config/errors decided with libraries, not hand-rolled
- [ ] Significant decisions written as short ADRs
- [ ] No abstraction added "just in case" (YAGNI)
