---
permalink: /
title: ""
author_profile: true
redirect_from: 
  - /about/
  - /about.html
---

👋 Hey! I’m **Tanay Makharia**, a backend engineer with 3+ years building data systems at scale — payroll for 80k+ businesses, a computational-chemistry platform, and a 33M-user marketplace. I love clean code, backend chaos, and ReactJS that doesn’t break on refresh.

I specialize in zero-downtime PostgreSQL migrations, multi-tenant access control, and query-performance engineering — and I operate AI coding agents as leverage while owning every architecture decision myself.

---

## 🔥 Work Highlights

<style>
.exp-container {
  display: flex;
  flex-wrap: wrap;
  gap: 1.2rem;
  margin-top: 2rem;
}
.exp-logo {
  flex: 0 0 70px;
}
.exp-logo img {
  width: 70px;
  height: auto;
}
.exp-details {
  flex: 1 1 300px;
}
.exp-details .role {
  font-style: italic;
  display: flex;
  flex-wrap: wrap;
  align-items: baseline;
  column-gap: 1rem;
  row-gap: 0.15rem;
}
.exp-details .date {
  color: #888;
  font-style: normal;
  margin-left: auto;
  white-space: nowrap;
}
.exp-details ul {
  line-height: 1.55;
}
</style>

<div class="exp-container">
  <div class="exp-logo">
    <img src="{{ base_path }}/images/pagarbook_logo.webp" alt="Pagarbook Logo">
  </div>
  <div class="exp-details">
    <strong>Pagarbook</strong><br>
    <div class="role"><strong>Software Development Engineer II</strong><span class="date">Aug 2025 – Present</span></div>
    <ul>
      <li>Led a zero-downtime payroll-config migration, moving TDS, month-size & shift-hours to per-staff/per-template sources of truth with temporal snapshotting of settled pay cycles</li>
      <li>Architected a multi-tenant Business→Division access hierarchy spanning login, auth context, RBAC, and the data layer</li>
      <li>Cut a multi-million-row PostgreSQL backfill from hours to minutes (~8× smaller working set) and dropped another query from ~70s to ~15s via a partial index over a 96.9M-row scan</li>
      <li>Diagnosed and healed production organizationId drift across 7 tenant-scoped tables, fixing soft-delete/orphan-row bugs that had blocked customers for days</li>
      <li>Owned a breaking payroll API change across 5 codebases (NestJS, iOS, Android, Kotlin Multiplatform, 2 React apps), preempting a fleet-wide login lockout</li>
      <li>Overhauled the busiest attendance endpoint, collapsing per-staff N+1 queries from ≈600 → 5 for a 100-staff org and parallelizing 12 serial setup queries</li>
      <li>Expanded RBAC with granular payments/payroll/roster permissions and built a transactional bulk staff-deactivation pipeline with backoff-retry</li>
    </ul>
  </div>
</div>

<div class="exp-container">
  <div class="exp-logo">
    <img src="{{ base_path }}/images/sdgr-logo.png" alt="Schrödinger Logo">
  </div>
  <div class="exp-details">
    <strong>Schrödinger (D.E. Shaw)</strong><br>
    <div class="role"><strong>Software Development Engineer I → II</strong><span class="date">Oct 2023 – Jul 2025</span></div>
    <ul>
      <li>Built and owned 40+ Django REST APIs for ST3, Schrödinger's internal target-tracking platform, developed TDD-first with Pytest, and shipped the matching React features end-to-end, cutting time-to-insight by 35%</li>
      <li>Designed Celery + Redis async pipelines for structured-data processing, notifications, and audit trails, keeping long-running jobs off the request path</li>
      <li>Automated the refresh of ChEMBL and other external biology datasets via a scheduled job, Airbyte ingestion, and DBT reshaping, cutting data latency from ~1–2 hours to ~10–15 minutes</li>
      <li>Ran the team's containerized microservices on GCP (Docker, Kubernetes) and authored reusable Terraform modules adopted across 4–5 environments</li>
      <li>Interviewed backend candidates and mentored interns; promoted to SDE II in 11 months</li>
    </ul>
  </div>
</div>

<div class="exp-container">
  <div class="exp-logo">
    <img src="{{ base_path }}/images/olx-logo.png" alt="OLX Logo">
  </div>
  <div class="exp-details">
    <strong>OLX Group (Autos Division)</strong><br>
    <div class="role"><strong>Software Engineer Intern</strong> <span class="date">Jan 2023 – Jul 2023</span></div>
    <ul>
      <li>Improved OLX Autos' user sell journey (33M+ monthly users), increasing completion rate by 8%</li>
      <li>Raised front-end test coverage from 30% to 70% using React Testing Library</li>
      <li>Shipped UI improvements with cross-functional teams via GitLab CI/CD</li>
    </ul>
  </div>
</div>

---

## 🎓 Education

<div class="exp-container">
  <div class="exp-logo">
    <img src="{{ base_path }}/images/lnmiit-logo.png" alt="LNMIIT Logo">
  </div>
  <div class="exp-details">
    <strong>The LNM Institute of Information Technology (LNMIIT)</strong><br>
    <div class="role">B.Tech in Computer Science and Engineering <span class="date">Aug 2019 – May 2023</span></div>
    <ul>
      <li>Focused on systems, backend engineering, and cloud infrastructure</li>
      <li>Explored competitive programming and problem-setting</li>
      <li>Member of programming and tech clubs; organized Codeforces-rated contest</li>
    </ul>
  </div>
</div>

Fun fact: I survived 8 semesters, competitive programming addiction, and 2am bug hunts.

---

## 💡 What I Do

I love building things that work reliably, scale gracefully, and occasionally surprise me with a `200 OK`.

- 💻 **Languages** — TypeScript, JavaScript, Python, Java, SQL, C++
- 🧱 **Backend Engineering** — Node.js, NestJS, Django, DRF, PostgreSQL, Redis, Kafka, RabbitMQ, Celery... basically my playground  
- 🌐 **Frontend (when absolutely necessary)** — React, Next.js, Redux, TypeScript, and HTML/CSS magic  
- ⚙️ **DevOps & Infra** — Docker, Kubernetes, GCP, AWS, Terraform, GitHub Actions, Datadog, Git (with as few `--force` pushes as possible)  
- 📊 **Data Pipelines** — Airbyte, DBT, custom ETL flows, and SQL queries that don't need `EXPLAIN`... usually  
- 🧪 **Testing** — Pytest, Unittest, Swagger docs, and tests that actually run on CI  
- 🤖 **AI-Assisted Dev** — Cursor, Claude Code, GitHub Copilot as leverage, not autopilot

---

## 🏆 Nerd Stats

- CodeChef 4★ (1930) · Codeforces Specialist (1422)  
- Solved 1200+ problems across LeetCode, Codeforces, CodeChef  
- Authored 35+ problems on HackerEarth that stumped more people than I expected  
- Co-organized a Codeforces-rated contest at LNMIIT  

---

## 🚀 Outside Work

When I'm not shipping code or debugging my own brain:
- I'm usually designing side projects or playing with system design puzzles  
- Writing articles, creating tools, or helping juniors prep for tech roles  
- Secretly refreshing [AWS status page](https://status.aws.amazon.com/) during outages just for fun

---

🎉 Thanks for visiting!  
If you like what you see, feel free to explore my work using the sidebar. Or just say hi—I don’t bite (unless you `git push --force` on main).
