# Learnify — E-Learning Website (HTML + CSS + JavaScript)

A college project: a multi-page e-learning website built with **HTML, CSS and vanilla JavaScript** (no frameworks, no npm).

> **Important:** this is a **frontend-only** project. There is no backend or database, so login, sign-up, forgot-password, the contact form and course progress are **demonstrations**. They show how the interface and JavaScript work, but they are **not real authentication** and nothing is sent to a server.

## How to run

The course list is loaded with `fetch()`, which browsers block on `file://`, so use a small local server:

```bash
cd learnify
python -m http.server 8000      # or: python3 -m http.server 8000
```

Then open **http://localhost:8000** (VS Code "Live Server" also works).
If you open `index.html` by double-clicking, static pages still work, but the Courses / Course Details / Dashboard pages will show a "could not load" message with a **Try again** button, which is the intended error state.

## Demo credentials

| Email | Password |
|---|---|
| `demo@learnify.com` | `Demo@1234` |

(They are displayed on the login page on purpose. Passwords are never stored in localStorage.)

## Project structure

```
learnify/
├── index.html            Home (popular courses, stats)
├── courses.html          Course list with search + category filter
├── course-details.html   One template for every course (?id=python)
├── instructors.html      Instructors
├── pricing.html          Pricing plans
├── about.html            About Us
├── contact.html          Contact form (validated, demo)
├── login.html            Demo login + CAPTCHA + forgot password
├── signup.html           Demo registration + password strength
├── dashboard.html        Student dashboard (demo data)
├── profile.html          Editable demo profile
├── css/style.css         All styling (your original stylesheet + additions)
├── js/script.js          ALL JavaScript (one file, one section per feature)
└── data/courses.json     Course data loaded with fetch()
```

## Implemented features

| Feature | Where | Type |
|---|---|---|
| Course search (case-insensitive, by course name) | courses.html | Frontend (real) |
| Category filter + "All" + search combined, empty-result message | courses.html | Frontend (real) |
| Courses loaded from JSON with loading / success / empty / error states | courses, details, dashboard | Frontend (real `fetch`) |
| Dynamic course details page for every course | course-details.html | Frontend (real) |
| Login validation, show/hide password, error + success feedback | login.html | **Demo** |
| Maths CAPTCHA with refresh, blocks login when wrong | login.html | **Demo** (not secure) |
| Forgot-password panel (email validation, honest "no email sent" message) | login.html | **Demo** |
| Sign-up validation, confirm-password match, strength meter | signup.html | **Demo** |
| Logout → home page + "You are successfully logged out." | navbar / sidebar | **Demo** |
| Dashboard & profile hidden after logout (login gate) | dashboard, profile | **Demo** |
| Contact form validation + success message | contact.html | **Demo** (nothing is sent) |
| Dashboard: progress, "Complete next lesson", remove, reset, activity log, empty state | dashboard.html | Demo data in localStorage |
| Enroll button (needs demo login) | course-details.html | Demo data in localStorage |
| Profile editing with validation and confirmation | profile.html | Demo data in localStorage |
| Mobile hamburger menu, auth-aware navbar, smooth scrolling | all pages | Frontend (real) |

**What would need a real backend:** genuine login/registration (password hashing, sessions/tokens), a server-verified CAPTCHA, sending reset emails, delivering contact messages, saving enrolment/progress per user, and payments.

## How to test the main things

1. **Search/filter** – open Courses, type `python` (also try `PYTHON`, `zzz`), click chips, combine chip + search, press "Show all courses".
2. **Login** – submit empty (errors), wrong CAPTCHA (blocked), wrong password (failure message), then correct credentials + CAPTCHA → dashboard. Use "Show" to reveal the password and "↻ New" to refresh the CAPTCHA.
3. **Forgot password** – click "Forgot password?", try an invalid email, then a valid one.
4. **Sign-up** – try a short password, mismatched confirmation, unticked terms; then a valid form (goes to login with a demo notice).
5. **Logout** – click Logout (navbar or sidebar): you land on Home with the green message; open `dashboard.html` again and you get the login gate.
6. **Dashboard** – click "Complete next lesson", "Remove", then "Reset demo data". Refresh: progress is remembered (localStorage).
7. **Profile** – clear a field (error), change values, Save, reload the page.
8. **Error state** – rename `data/courses.json` temporarily and reload Courses: you see the error + "Try again".
9. **Mobile** – narrow the browser (or DevTools device mode): hamburger menu, stacked cards, dashboard bar.

## Where the JavaScript concepts are used (`js/script.js`)

- **Variables/constants, functions, conditionals** – everywhere (`STORAGE`, `DEMO_USER`, `validators`, `passwordStrength()`).
- **Arrays & objects, `filter()` / `map()` / `find()` / `reduce()`** – `getVisibleCourses()` (search + category), dashboard stats and rows, `courses.find(...)` on the details page.
- **DOM selection & manipulation** – `$()` / `$$()` helpers, `innerHTML` templates, `textContent`, `classList`, `hidden`.
- **Event listeners** – chips, search input/submit, form `submit`/`blur`/`input`, password toggle, hamburger, CAPTCHA refresh, delegated clicks for Logout and dashboard buttons.
- **Form validation & string methods** – `validators` object (`trim()`, `toLowerCase()`, `includes()`, regular expressions, `padStart()`).
- **Template literals** – course cards, messages, dashboard rows.
- **localStorage / sessionStorage** – non-sensitive demo state only (profile, progress, remembered email; the demo login flag lives in sessionStorage). Passwords are never stored.
- **Promises** – `delay()` returns a `new Promise(...)`; `demoAuthenticate()` is a Promise that resolves/rejects to simulate a server login.
- **async/await + try/catch + fetch()** – `loadCourses()` fetches `data/courses.json`; `initCoursesPage()`, `initCourseDetails()`, `initDashboardPage()` `await` it and handle loading, success, empty and error states. Form submit handlers `await delay(...)` to simulate server time (clearly labelled demo).
- **setTimeout** – inside `delay()` only (simulated latency).
- **Separation of concerns** – each page has its own `initXxxPage()`; `DOMContentLoaded` runs only the code for `<body data-page="...">`, so a missing element never throws.

## Honest notes

- The CAPTCHA answer is held in JavaScript in the page, so anyone can read it. It demonstrates the idea only.
- Session state can be edited in browser DevTools; it is for the demo UI, not security.
- Dashboard numbers are demo data that you can reset; they are not real student records.
- Instructor/student statistics on the home page are the placeholder figures from the original project.
