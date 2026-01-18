Add endpoints in @app/main.py:
- "start-prediction": should create new "app_database.db" file in "app" folder (instead of each time reusing existing one). Then start separate thread that will process samples sent to the service. Function to predict in this thread in in @app/background.py . And change some variable that contains state of the service to "STARTED".
- "stop-prediction": Somehow stop thread what was started when called "start-prediction" endpoint. But do it gently. Stop prediction loop execution right after current sample was processed and commited in the database. Don't interrupt loop execution forcefully as it can leat to race conditions and other problems.
Change endpoints:
- "reset": add action to remove "app_database.db" file from "app" folder
Add basic docstrings for these endpoints (if absent). Such that endpoint users understand what this endpoints do.
Adjust documentation in @app/DOCS.md to include these endpoints (if necessary).