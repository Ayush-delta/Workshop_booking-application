# **Workshop Booking**

> This website is for coordinators to book a workshop(s), they can book a workshop based on instructors posts or can propose a workshop date based on their convenience.

### Features
* Statistics
    1. Instructors Only
        * Monthly Workshop Count
        * Instructor/Coordinator Profile stats
        * Upcoming Workshops
        * View/Post comments on Coordinator's Profile
    2. Open to All
        * Workshops taken over Map of India
        * Pie chart based on Total Workshops taken to Type of Workshops.

* Workshop Related Features
    > Instructors can Accept, Reject or Delete workshops based on their preference, also they can postpone a workshop based on coordinators request.

### Setup Instructions
1. **Clone the Repository**:
   ```
   git clone <repository-url>
   cd workshop_booking
   ```

2. **Create a Virtual Environment** (Recommended):
   ```
   python -m venv venv
   # On Windows:
   venv\Scripts\activate
   # On macOS/Linux:
   source venv/bin/activate
   ```

3. **Install Dependencies**:
   Copy `.sampleenv` to `.env` and configure any environment variables (e.g., database settings, secret key).
   ```
   pip install -r requirements.txt
   ```

4. **Database Setup**:
   ```
   python manage.py migrate
   ```

5. **Create a Superuser** (for admin access):
   ```
   python manage.py createsuperuser
   ```

6. **Collect Static Files** (if needed for production):
   ```
   python manage.py collectstatic
   ```

7. **Run the Development Server**:
   ```
   python manage.py runserver
   ```
   Access the site at `http://127.0.0.1:8000/`.

8. **Additional Notes**:
   - Ensure you have Python 3.8+ installed.
   - For email functionality (e.g., password resets), configure SMTP settings in `.env`.
   - Check `docs/Getting_Started.md` for model diagrams and advanced configuration.

### Recent Updates
- **Fixed JavaScript Syntax Errors in `statistics_app/templates/statistics_app/workshop_stats.html`**:
  - Refactored Django template data insertions (e.g., `{{ workshop_count|safe }}`, `{{ workshoptype_count|safe }}`, `{{ india_map|safe }}`) into standalone JavaScript variables (e.g., `var workshopCount = {{ workshop_count|safe }};`) before using them in Chart.js configurations. *Why*: Direct interpolation inside object literals/arrays caused JavaScript parser confusion, leading to syntax errors like "Declaration or statement expected" and unbalanced braces, especially with the closing `]` at line 270. This ensures valid JS structure regardless of data format.
  
- **Corrected Invalid HTML Table Structure**:
  - Consolidated multiple `<tbody>` tags (one per loop iteration in upcoming workshops) into a single `<tbody>` wrapping the entire `{% for %}` loop; removed unnecessary `{% csrf_token %}` inside the table and placeholder dummy rows. *Why*: Multiple `<tbody>` elements violate HTML standards, potentially causing rendering inconsistencies in browsers; the token was redundant as the form already includes one, and dummies cluttered production code.

- **Resolved Undefined `dateFormat` in jQuery Datepicker**:
  - Added `var dateFormat = "yy-mm-dd";` inside the `getDate` function. *Why*: The function called `$.datepicker.parseDate(dateFormat, ...)` without defining `dateFormat`, resulting in a ReferenceError at runtime.

- **Adjusted Datepicker Defaults for Better UX**:
  - Set `from` defaultDate to `dateToday` (yesterday) with maxDate `dateToday`; `to` defaultDate to `upto` (next year) with minDate `dateToday`. *Why*: Original defaults (`+1w` future) conflicted with max/min dates, confusing users selecting past/current ranges for statistics filtering.

- **Updated Chart.js Scales Configuration**:
  - Changed from v2 `scales: { yAxes: [{ ticks: { beginAtZero: true } }] }` to v3 `scales: { y: { ticks: { beginAtZero: true } } }`. *Why*: The project uses Chart.js 3.9.1; v2 syntax is deprecated and causes runtime errors in v3+.

- **Set Initial Visibility of Visualization Div**:
  - Changed style from `display: block` to `display: none`. *Why*: The div is toggled visible only for the India map mode; starting hidden prevents unnecessary initial rendering and ensures consistent behavior with chart modes.

These changes resolve all detected linter errors, improve code maintainability, and enhance user experience without altering functionality.

### Contribution Guidelines
We welcome contributions to improve the Workshop Booking system! Please follow these guidelines to ensure a smooth collaboration.

#### How to Fork, Branch, and Submit Pull Requests (PRs)
1. **Fork the Repository**:
   - Go to the repository on GitHub and click "Fork" to create a copy under your account.

2. **Clone Your Fork**:
   ```
   git clone https://github.com/your-username/workshop_booking.git
   cd workshop_booking
   git remote add upstream https://github.com/original-owner/workshop_booking.git
   ```

3. **Create a Feature Branch**:
   - Base your branch off `main`:
   ```
   git checkout -b feature/your-feature-name
   ```
   - Use descriptive names (e.g., `fix/js-syntax-errors`, `feat/add-user-auth`).

4. **Make Your Changes**:
   - Implement your feature or fix, commit with clear messages:
   ```
   git add .
   git commit -m "Fix: Resolve JS syntax in workshop_stats.html"
   git push origin feature/your-feature-name
   ```

5. **Submit a Pull Request**:
   - Open a PR on GitHub from your branch to the upstream `main`.
   - Describe the changes, reference issues (e.g., "Fixes #123"), and include screenshots if UI-related.
   - Ensure the PR passes CI checks (if set up) and tests.

6. **Sync with Upstream** (if needed):
   ```
   git fetch upstream
   git merge upstream/main
   git push origin feature/your-feature-name
   ```

#### Coding Conventions and Best Practices
- **Python/Django**:
  - Follow [PEP 8](https://peps.python.org/pep-0008/) style guide (use tools like Black or autopep8).
  - Use Django's ORM efficiently; avoid raw SQL unless necessary.
  - Write DRY (Don't Repeat Yourself) code; leverage class-based views where appropriate.
  - Secure against common vulnerabilities (e.g., CSRF, XSS) – always use `{% csrf_token %}` in forms.

- **JavaScript/HTML/CSS**:
  - Use ES6+ syntax; lint with ESLint for consistency.
  - Ensure semantic HTML5; validate with W3C tools.
  - For charts/maps, prefer modular data handling (as in recent JS refactors) to avoid template interpolation issues.
  - Responsive design with Bootstrap; test on mobile.

- **General**:
  - Commit atomic changes; one feature/fix per PR.
  - Update documentation (e.g., this README, inline comments) for non-obvious code.
  - Use type hints in Python where possible.

#### Testing and Local Setup for Developers
- **Local Setup**: Follow the "Setup Instructions" above. For development:
  ```
  # Run tests
  python manage.py test

  # Run with debug mode (settings.py DEBUG=True)
  python manage.py runserver

  # Lint code
  pip install flake8  # For Python
  flake8 ./
  ```

- **Testing**:
  - Write unit tests in `tests.py` files using Django's TestCase.
  - Cover models, views, forms; aim for >80% coverage (use coverage.py).
  - For frontend: Manual browser testing; consider adding Jest for JS.
  - Run migrations before testing: `python manage.py makemigrations && python manage.py migrate`.

- **Debugging**:
  - Use Django debug toolbar (`pip install django-debug-toolbar`).
  - Check console for JS errors; use browser dev tools.

