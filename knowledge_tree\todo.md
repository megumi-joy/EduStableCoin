Below is a detailed proposal for your educational app, tailored to your specific requirements. I’ve broken it down into discrete steps with explicit instructions for a less intelligent sub-LLM to follow, ensuring it builds incrementally and stops for feedback at each step. The approach uses Python, includes automated tests, and aligns with your vision of a social network-style educational app with a Skyrim-like skill tree based on a school curriculum from 20 years ago (circa 2005). I’ll assume a basic tech stack but leave room for your existing codebase to integrate.
Proposal: Educational Social Network App with Skyrim-Style Skill Tree
General Approach
We’ll create a Python-based web application using Flask (lightweight and flexible) for the backend, SQLite for the database (simple to start), and a basic frontend with HTML/CSS/JavaScript for interactivity (e.g., the skill tree and 3D chat). The app will feature:
Social Network Features: User profiles, posts, messaging, blog walls, and groups.
Skill Tree: A navigable tree of school subjects (e.g., Language, Math, Physics) from grades 1-9, styled like Skyrim’s skill tree, with XP earned via assigned content and exercises.
XP System: Points for consuming knowledge (small), completing assigned exercises (high), and inventing exercises (higher). Levels tied to “speech richness” and planetary themes (12 levels per year).
Virtual 3D Chat: A simple 3D environment for interaction (using Three.js for the frontend).
Testing: Automated tests with unittest to avoid manual click-testing until the end.
The project will be modular, with discrete steps to build and test incrementally. Each step will produce a working intermediate product, and the sub-LLM will stop for your feedback before proceeding.
Full Prompt for Sub-LLM
Instructions for Sub-LLM: You are a coding assistant building an educational app in Python. Follow these steps EXACTLY as written. Do NOT skip ahead or combine steps unless explicitly told to by the user. At the end of each step, STOP and wait for feedback. Be very specific in your code and explanations, as I (the user) know what I’m doing and have a reason for every requirement. Use Python 3.11, Flask, SQLite, and basic HTML/CSS/JavaScript. Write automated tests with unittest for every step. Assume a school curriculum from 2005 (grades 1-9) with subjects like Language, Math, Physics, etc. Do NOT assume modern frameworks unless specified. Here are the discrete steps:
Step 1: Project Setup and Basic Flask App
Goal: Set up the project structure and a minimal Flask app with a homepage.
Instructions:
Create a project folder called edu_social_app.
Inside it, create:
app.py (main Flask app)
templates/ (for HTML files)
static/ (for CSS, JS, images)
tests/ (for automated tests)
Install dependencies: flask, sqlite3 (standard library).
In app.py, write a Flask app with one route (/) that renders a simple index.html saying "Welcome to EduSocial".
In templates/index.html, create a basic HTML page with that welcome message.
In tests/test_app.py, write a unittest test to check if the / route returns a 200 status code and contains "Welcome to EduSocial".
Run the app (python app.py) and the tests (python -m unittest tests/test_app.py).
STOP and wait for feedback.
Sample Code:
python
# app.py
from flask import Flask, render_template

app = Flask(__name__)

@app.route('/')
def home():
    return render_template('index.html')

if __name__ == '__main__':
    app.run(debug=True)
html
<!-- templates/index.html -->
<!DOCTYPE html>
<html>
<head><title>EduSocial</title></head>
<body>
    <h1>Welcome to EduSocial</h1>
</body>
</html>
python
# tests/test_app.py
import unittest
from app import app

class TestApp(unittest.TestCase):
    def setUp(self):
        self.app = app.test_client()

    def test_home(self):
        response = self.app.get('/')
        self.assertEqual(response.status_code, 200)
        self.assertIn(b'Welcome to EduSocial', response.data)

if __name__ == '__main__':
    unittest.main()
Step 2: Database Setup with Users and Profiles
Goal: Add a SQLite database with a users table for profiles.
Instructions:
In app.py, import sqlite3 and create a init_db() function to set up a users table with columns: id (integer, primary key), username (text, unique), password (text), bio (text).
Call init_db() once when the app starts (use a check to avoid re-creation).
Add a /register route (POST) to add users to the database.
Create templates/register.html with a form for username, password, and bio.
Update index.html with a link to /register.
In tests/test_app.py, add tests for:
Database creation (check if users table exists).
Registering a user (POST to /register and check database).
STOP and wait for feedback.
Step 3: Social Features (Posts and Messaging)
Goal: Add tables and routes for posts and messaging.
Instructions:
In init_db(), add tables:
posts: id, user_id (foreign key), content, timestamp.
messages: id, sender_id (foreign key), receiver_id (foreign key), content, timestamp.
Add routes:
/post (POST): Create a post (assume logged-in user with user_id=1 for now).
/messages (GET): Show all messages for the logged-in user (mock user_id=1).
Create templates: post.html (form), messages.html (list messages).
Update index.html with links to /post and /messages.
Add tests for posting and retrieving messages.
STOP and wait for feedback.
Step 4: Skill Tree Structure
Goal: Create a database and JSON structure for the skill tree (subjects from grades 1-9).
Instructions:
In init_db(), add a skills table: id, subject (e.g., "Math"), grade (1-9), parent_id (for tree hierarchy, nullable), xp_required (integer).
Populate it with a sample tree based on a 2005 curriculum (e.g., Language → Grammar → Sentences; Math → Algebra → Equations).
Add a /skills route (GET) that returns the tree as JSON.
In static/skills.js, write JavaScript to fetch and log this JSON (link it in a new templates/skills.html).
Add tests for the /skills route and database population.
STOP and wait for feedback.
Step 5: Skyrim-Style Skill Tree UI
Goal: Render the skill tree visually like Skyrim’s constellation system.
Instructions:
In static/skills.css, style the skill tree with CSS (nodes as circles, lines connecting them).
Update skills.js to dynamically render the tree from the /skills JSON, mimicking Skyrim’s navigation (e.g., clickable nodes).
Update skills.html to include this UI.
Add a test to ensure the /skills route still works.
STOP and wait for feedback.
Step 6: XP System and Levels
Goal: Implement XP earning and leveling logic.
Instructions:
Add a user_skills table: user_id, skill_id, xp (integer).
Add routes:
/learn (POST): Add small XP (e.g., 5) for consuming assigned content.
/exercise (POST): Add high XP (e.g., 50) for assigned exercises, higher (e.g., 100) for self-invented ones.
Create a levels table: user_id, year (1-9), level (1-12), speech_richness (text, e.g., "Basic", "Planetary").
Add logic in app.py to update levels based on total XP per year (e.g., 600 XP = 12 levels).
Add tests for XP assignment and level progression.
STOP and wait for feedback.
Step 7: Virtual 3D Chat
Goal: Add a basic 3D chat using Three.js.
Instructions:
In static/chat.js, use Three.js to create a simple 3D scene (e.g., a cube room) with a text input for messages.
Add a /chat route and templates/chat.html to load this scene.
Link messages from the messages table to display in the 3D chat.
Add a test for the /chat route.
STOP and wait for feedback.
Step 8: Groups and Blog Walls
Goal: Add groups and blog walls.
Instructions:
Add a groups table: id, name, description.
Add a group_posts table: id, group_id, user_id, content.
Add routes: /groups (GET), /group/<id> (GET/POST for blog wall).
Create templates: groups.html, group.html.
Add tests for group creation and posting.
STOP and wait for feedback.
Step 9: Final Integration and Polish
Goal: Tie everything together with navigation and basic styling.
Instructions:
Update index.html with a navbar linking all features.
Add basic CSS in static/style.css for consistency.
Test the full app with unittest for all routes.
STOP and wait for feedback.
Notes for You
This assumes your codebase can integrate with Flask/SQLite. If you have a different stack (e.g., Django), let me know after Step 1.
The 2005 curriculum is approximated (e.g., Language, Math, Physics); provide specifics if needed.
The 3D chat is basic; enhance it later if desired.
Automated tests ensure stability without manual clicking, per your request.
Let me know if you want to tweak this before passing it to the sub-LLM!