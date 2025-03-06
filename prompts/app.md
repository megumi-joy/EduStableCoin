Below is a clean prompt and database design for your educational social networking app, crafted with precision and clarity to meet your requirements. This solution assumes you want a detailed specification for development (the "prompt") and a well-structured database schema, optimized for an app built in Python with a web frontend, automated testing, and deployment on Replit.
Prompt for Development
Develop an educational social networking app with the following features:
User Authentication and Profiles: Secure login system with customizable user profiles.
Posts and Blog Walls: Users can create and view posts on a shared wall.
Private Messaging: Direct, private communication between users.
Groups with Membership Management: Users can create and join groups with defined memberships.
Skill Tree: A hierarchical system based on school subjects from 20 years ago (grades 1-9), where users earn experience points by completing activities.
Levels: Progression tied to the richness of users' speech, analyzed from their posts and messages.
Virtual Classroom: A space for real-time interaction among users.
Technical Requirements:
Backend: Build in Python using a framework like Flask or Django.
Frontend: Create a web-based interface with HTML, CSS, and JavaScript.
Database: Use SQLite for simplicity and compatibility with Replit.
Real-Time Features: Implement the virtual classroom with WebSockets (e.g., via Flask-SocketIO) for text, audio, or video chat capabilities.
NLP Analysis: Use a library like spaCy to analyze speech richness for levels, based on posts and messages.
Testing: Write comprehensive automated tests with pytest to cover all major functionalities, minimizing manual testing.
Hosting: Deploy and test the app on Replit.
Feature Details:
Skill Tree: Structure it hierarchically (subjects → topics → subtopics). Link activities to skill nodes, awarding experience points upon completion. Activities include types like "reading" (self-completed) and "exercise" (teacher-validated).
Levels: Periodically analyze users' posts and messages to calculate a speech richness score (e.g., vocabulary diversity, sentence complexity). Map scores to level numbers (e.g., level_number = floor(score / 100)).
Activity Validation: Teachers validate exercise-type activities, ensuring educational integrity.
Development Plan:
Setup: Initialize a Python project with Flask/Django, SQLite, and pytest. Configure Replit hosting.
Database: Implement the schema below and test connectivity.
Core Features: Build authentication, profiles, posts, messaging, and groups.
Skill Tree: Create the hierarchical structure and activity system with experience tracking.
Levels: Integrate NLP analysis for speech richness and update levels periodically.
Virtual Classroom: Add real-time interaction with WebSockets.
Testing: Write and run automated tests for all features.
Frontend: Develop a user-friendly HTML/CSS/JavaScript interface.
Deployment: Deploy to Replit and verify functionality.
Database Design
Below is the SQLite database schema, designed to support all app features efficiently. Each table includes essential fields and relationships.
1. Users Table
Stores user account information and roles.
user_id INTEGER PRIMARY KEY
username TEXT NOT NULL UNIQUE
password_hash TEXT NOT NULL
email TEXT NOT NULL UNIQUE
role TEXT NOT NULL (e.g., 'student', 'teacher')
2. Profiles Table
Holds user profile details.
profile_id INTEGER PRIMARY KEY
user_id INTEGER FOREIGN KEY REFERENCES Users(user_id)
name TEXT NOT NULL
grade INTEGER
subjects TEXT (JSON or comma-separated string of enrolled subjects)
3. Posts Table
Manages user posts on the blog wall.
post_id INTEGER PRIMARY KEY
user_id INTEGER FOREIGN KEY REFERENCES Users(user_id)
content TEXT NOT NULL
timestamp DATETIME DEFAULT CURRENT_TIMESTAMP
4. Messages Table
Stores private messages between users.
message_id INTEGER PRIMARY KEY
sender_id INTEGER FOREIGN KEY REFERENCES Users(user_id)
receiver_id INTEGER FOREIGN KEY REFERENCES Users(user_id)
content TEXT NOT NULL
timestamp DATETIME DEFAULT CURRENT_TIMESTAMP
5. Groups Table
Defines groups within the app.
group_id INTEGER PRIMARY KEY
name TEXT NOT NULL
description TEXT
6. Group Members Table
Tracks group memberships (many-to-many relationship).
group_id INTEGER FOREIGN KEY REFERENCES Groups(group_id)
user_id INTEGER FOREIGN KEY REFERENCES Users(user_id)
PRIMARY KEY (group_id, user_id)
7. Skill Nodes Table
Represents the skill tree hierarchy (subjects, topics, subtopics).
node_id INTEGER PRIMARY KEY
subject TEXT NOT NULL (e.g., 'Math', 'Science')
topic TEXT NOT NULL (e.g., 'Algebra', 'Biology')
grade_level INTEGER NOT NULL
parent_id INTEGER FOREIGN KEY REFERENCES Skill_Nodes(node_id) (NULL for root nodes)
8. Activities Table
Contains educational activities tied to skill nodes.
activity_id INTEGER PRIMARY KEY
node_id INTEGER FOREIGN KEY REFERENCES Skill_Nodes(node_id)
type TEXT NOT NULL (e.g., 'reading', 'exercise')
content TEXT NOT NULL (e.g., reading text or exercise prompt)
points INTEGER NOT NULL (points awarded upon completion)
9. User Activities Table
Tracks users' progress on activities, including validation.
user_activity_id INTEGER PRIMARY KEY
user_id INTEGER FOREIGN KEY REFERENCES Users(user_id)
activity_id INTEGER FOREIGN KEY REFERENCES Activities(activity_id)
status TEXT NOT NULL (e.g., 'completed', 'submitted', 'validated')
answer TEXT (user’s response for exercises, NULL for reading)
validated_by INTEGER FOREIGN KEY REFERENCES Users(user_id) (teacher who validated, NULL if not applicable)
timestamp DATETIME DEFAULT CURRENT_TIMESTAMP
10. User Skills Table
Records experience points per user per skill node.
user_id INTEGER FOREIGN KEY REFERENCES Users(user_id)
node_id INTEGER FOREIGN KEY REFERENCES Skill_Nodes(node_id)
experience_points INTEGER DEFAULT 0
PRIMARY KEY (user_id, node_id)
11. Levels Table
Tracks user levels based on speech richness.
user_id INTEGER FOREIGN KEY REFERENCES Users(user_id)
level_number INTEGER NOT NULL
speech_richness_score REAL NOT NULL
PRIMARY KEY (user_id)
Notes on Implementation
Skill Tree Progression: When a user completes an activity (status becomes 'validated' for exercises or 'completed' for reading), add the activity’s points to the corresponding experience_points in the User Skills table.
Speech Richness: Use spaCy to analyze content from Posts and Messages tables. Calculate a score (e.g., based on unique words or sentence complexity) and update the Levels table periodically.
Scalability: SQLite is chosen for simplicity, but for larger scale, consider migrating to PostgreSQL.
This prompt and database design provide a robust foundation for your app, balancing functionality, clarity, and ease of development. Let me know if you need further refinements!