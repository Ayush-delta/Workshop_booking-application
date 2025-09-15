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

__NOTE__: Check docs/Getting_Started.md for more info.
