# ShotMaster - Claude Code Development Guide

## Project Overview
ShotMaster is a Flask web application for managing AI-driven image-to-video filmmaking workflows. It provides structured organization, versioning, and annotation of generated stills and videos with a drag-and-drop interface.

**Original:** Forked from [albozes/shotbuddy](https://github.com/albozes/shotbuddy)  
**Current:** [C47Joe/shotmaster](https://github.com/C47Joe/shotmaster)

## Quick Start Commands

### Development Server
```bash
source venv/bin/activate
python run.py
```
Server runs on http://127.0.0.1:5001

### Testing & Quality
```bash
# Currently no specific test/lint commands configured
# TODO: Add test framework and linting setup
```

### Dependencies
```bash
pip install -r requirements.txt
```

## Project Structure

```
shotbuddy/                 # Main application directory (legacy name)
├── app/
│   ├── __init__.py       # Flask app factory
│   ├── config/
│   │   └── constants.py  # App constants and environment variables
│   ├── routes/
│   │   ├── project_routes.py  # Project management endpoints + file browser
│   │   └── shot_routes.py     # Shot management endpoints
│   ├── services/
│   │   ├── project_manager.py # Project CRUD operations
│   │   ├── shot_manager.py    # Shot operations and file handling
│   │   ├── file_handler.py    # File operations and thumbnails
│   │   └── prompt_importer.py # Prompt management
│   ├── static/
│   │   ├── css/main.css      # All styling including file browser
│   │   ├── js/main.js        # All JavaScript including file browser
│   │   └── icons/            # UI icons
│   ├── templates/
│   │   └── index.html        # Single page application template
│   └── utils.py              # Utility functions
├── venv/                     # Python virtual environment
├── shotmaster.cfg            # Server configuration
├── run.py                    # Application entry point
└── requirements.txt          # Python dependencies
```

## Key Features Implemented

### File Path Selector (Custom Feature)
- **Location:** `app/routes/project_routes.py:163` - `/api/browse` endpoint
- **Frontend:** `app/templates/index.html` - File browser modal
- **JavaScript:** `app/static/js/main.js:8-101` - File browser functions
- **Styling:** `app/static/css/main.css:60-168` - File browser CSS

**Functionality:**
- Visual directory navigation instead of manual path typing
- Security checks to prevent unauthorized directory access
- Highlights existing ShotMaster projects in directory listings
- Parent directory navigation and breadcrumb display

### Project Management
- Create/open projects with structured folder layout
- Recent projects tracking in `projects.json`
- Automatic project structure creation (`shots/`, `wip/`, `latest_images/`, etc.)

### Shot Management
- Drag & drop asset upload
- Automatic file organization with shot naming (SH001, SH002, etc.)
- Version tracking and thumbnail generation
- Prompt/notes annotation per shot

## Configuration

### Environment Variables
```bash
SHOTMASTER_HOST=127.0.0.1        # Server host (default: 127.0.0.1)
SHOTMASTER_PORT=5001             # Server port (default: 5001)
SHOTMASTER_DEBUG=0               # Debug mode (0/1)
SHOTMASTER_UPLOAD_FOLDER=uploads # Upload directory
```

### Config File: `shotmaster.cfg`
```ini
[server]
host = 0.0.0.0
port = 5001
```

## Development Notes

### Recent Changes
1. **File Path Selector** - Replaced manual path input with visual directory browser
2. **Application Rename** - Complete rebrand from "Shotbuddy" to "ShotMaster"
3. **Fork Setup** - Migrated to personal GitHub repository

### Code Conventions
- Flask blueprints for route organization
- Service layer pattern for business logic  
- Static file serving for thumbnails and assets
- JSON API responses with consistent error handling
- Single-page application with vanilla JavaScript

### Known Issues & TODOs
- [ ] No test framework configured
- [ ] No linting/code quality tools set up
- [ ] Legacy folder name "shotbuddy" still used (complex to rename safely)
- [ ] Could benefit from TypeScript for better JS maintainability

## Deployment Notes

### Development
The app auto-opens browser on startup via threading in `run.py`. Uses Flask development server.

### Production Considerations
- Currently uses Flask dev server (not production ready)
- Would need WSGI server (gunicorn, uwsgi) for production
- Static file serving should use nginx or similar in production
- Database storage could replace JSON file persistence

## API Endpoints

### Project Management
- `GET /` - Main application interface
- `GET /api/project/current` - Get current project info
- `GET /api/project/recent` - Get recent projects list
- `POST /api/project/open` - Open existing project by path
- `POST /api/project/create` - Create new project

### File Browser (Custom)
- `GET /api/browse?path=<path>` - Browse directory contents for path selection

### Shot Management
- Various shot endpoints in `shot_routes.py` (see file for details)

## Dependencies
- **Flask 3.1.1** - Web framework
- **Flask-CORS 6.0.1** - Cross-origin requests
- **Pillow 11.2.1** - Image processing and thumbnails
- **pytest** - Testing framework (not currently used)

## Git Workflow
Repository is configured with upstream tracking to original project:
- `origin` → Your fork at C47Joe/shotmaster
- `upstream` → Original at albozes/shotbuddy

Use standard GitHub flow for feature development.