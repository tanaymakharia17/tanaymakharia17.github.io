---
layout: archive
title: "CV"
permalink: /cv/
---
{% include base_path %}


<div style="text-align: center; margin-top: 20px;">
  <a
    href="{{ site.author.resume_download_url }}"
    target="_blank"
    download
    style="
      background-color: #1a73e8;
      color: #fff;
      font-size: 16px;
      font-weight: 600;
      padding: 12px 28px;
      border-radius: 30px;
      text-decoration: none;
      display: inline-block;
      box-shadow: 0 2px 6px rgba(0, 0, 0, 0.15);
      transition: all 0.3s ease;
    "
    onmouseover="this.style.backgroundColor='#1967d2'; this.style.boxShadow='0 4px 12px rgba(0, 0, 0, 0.2)'"
    onmouseout="this.style.backgroundColor='#1a73e8'; this.style.boxShadow='0 2px 6px rgba(0, 0, 0, 0.15)'"
  >
    📄 Download Resume
  </a>
</div>

<iframe
  src="{{ site.author.resume_preview_url }}"
  width="100%"
  height="1000px"
  style="
    border: none;
    border-radius: 10px;
    margin: 20px 0;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  "
  allow="autoplay"
  loading="lazy"
>
  Your browser does not support embedded PDFs.
  <a href="{{ site.author.resume_preview_url }}" target="_blank">Download the resume instead</a>.
</iframe>



<!-- ---
layout: archive
title: "CV"
permalink: /cv/
author_profile: true
redirect_from:
  - /resume
---

{% include base_path %}

Education
======
* **THE LNM INSTITUTE OF INFORMATION TECHNOLOGY**, 2019 – 2023  
  Bachelor of Technology, Computer Science and Engineering  
  GPA: 7.91 / 10  
  Jaipur, India

Work experience
======
* **Schrödinger (D. E. Shaw)** — Hyderabad, India  
  **Software Developer 2** (July 2024 – Present)  
  **Software Developer** (Oct 2023 – June 2024)  
  *Tech:* Django, ReactJS, PostgreSQL, DRF, Celery, Redis, GCP, Kubernetes, Terraform, DBT, Airbyte, Swagger, Pytest  
  * Highlights:
    * Developed 40+ Django REST APIs with TDD
    * Built full-stack features (Django + ReactJS), reducing time-to-insight by 35%
    * Designed async job flows (Celery + Redis) with real-time notifications
    * Built ingestion pipelines (Airbyte + DBT), reducing ETL latency by 90%
    * Managed microservices on GCP with Docker + Kubernetes
    * Created reusable Terraform modules and templates
    * Mentored interns and conducted backend system design interviews

* **OLX Group (Autos Division)** — Gurugram, India  
  **Software Engineer Intern** (Jan 2023 – July 2023)  
  *Tech:* ReactJS, Redux, JavaScript, React Testing Library  
  * Highlights:
    * Enhanced car-seller UI flows (33M+ monthly users)
    * Increased user flow completion by 8% via A/B-tested UI changes
    * Boosted test coverage from 30% → 70%
    * Worked in Agile teams with CI/CD using GitLab

Projects
======
* **[FoodOnline](https://github.com/tanaymakharia17/foodApp)** — Online Food Ordering Website  
  *Tech:* Django, PostgreSQL, Bootstrap  
  * Created customer + vendor dashboards  
  * Integrated Razorpay and PayPal payments  
  * Features: order tracking, revenue stats, menu management

Skills
======
* **Languages**: Python, Java, JavaScript, TypeScript, C++, SQL, HTML/CSS  
* **Frameworks**: Django, DRF, Spring Boot, ReactJS, Redux, Celery  
* **Data & Infra**:
  * Databases: PostgreSQL, Redis, MySQL  
  * Data: DBT, Airbyte  
  * Cloud/DevOps: GCP, AWS, Docker, Kubernetes, Terraform  
* **Testing/APIs**: Pytest, Unittest, Swagger, React Testing Library

Achievements
======
* CodeChef Rating: 1930 (4-Star)  
* Codeforces Rating: 1422 (Specialist)  
* Solved 1200+ algorithmic problems across LeetCode, Codeforces, and CodeChef  
* Organized Codeforces-rated contest at LNMIIT — tested and validated problems  
* Authored 35+ advanced HackerEarth problems

Publications
======
  <ul>{% for post in site.publications reversed %}
    {% include archive-single-cv.html %}
  {% endfor %}</ul>

Talks
======
  <ul>{% for post in site.talks reversed %}
    {% include archive-single-talk-cv.html  %}
  {% endfor %}</ul>

Teaching
======
  <ul>{% for post in site.teaching reversed %}
    {% include archive-single-cv.html %}
  {% endfor %}</ul>

Service and leadership
======
* Mentored interns at Schrödinger on backend architecture and engineering best practices  
* Organized and supported DSA contests at LNMIIT -->

