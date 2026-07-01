# Health & Nutrition Tracker using Gemini

This is a Flask-based web application designed for tracking health and nutrition. It allows users to sign up, manage their profiles, and upload images of food for analysis. The application leverages the Gemini 2.5 Flash model to provide nutritional information and recommendations based on the uploaded images. It features a clean, modern user interface and a secure backend. PoC Developed with (Jules, Gemini, Self effort) :
### Khurram Nazir
### Khurram.deutsch@yahoo.com
### 18.12.2025
###-----------------------------------
## Features

- **User Authentication:** Secure signup, login, and logout functionality for regular users.
- **Admin Dashboard:** A dedicated dashboard for the admin user to view all registered users, delete users, and change user passwords.
- **Profile Management:** Users can update their personal information (age, gender, weight, known diseases) and change their passwords.
- **File Uploads:** Users can upload images (PNG, JPG, JPEG) to their own unique, private directory, organized by date. Users can also delete their uploaded images.
- **AI-Powered Nutrition Analysis:** The application uses the Gemini 2.5 Flash model to analyze uploaded food images.
- **View User Uploads (Admin):** The admin can view the uploaded images for each user.

## Project Structure

```
.
├── app.py              # Main Flask application file
├── init_db.py          # Initializes the database
├── requirements.txt    # Project dependencies
├── run_tests.py        # Test runner
├── .env                # Environment variables (for GEMINI_API_KEY)
├── .gitignore
├── instance
│   └── database.db     # SQLite database file
├── static
│   └── style.css       # CSS stylesheet
├── templates           # HTML templates
└── users               # Directory for user-uploaded files
```

## Database Schema

The application uses a SQLite database with two tables: `users` and `uploads`.

### `users` table

| Column         | Type    | Description                                  |
|----------------|---------|----------------------------------------------|
| id             | INTEGER | Primary Key, Auto-incrementing               |
| name           | TEXT    | User's full name                             |
| username       | TEXT    | User's unique username                       |
| password       | TEXT    | Hashed password                              |
| age            | INTEGER | User's age                                   |
| gender         | TEXT    | User's gender                                |
| weight         | REAL    | User's weight in kg                          |
| known_diseases | TEXT    | Comma-separated list of known diseases       |
| other_disease  | TEXT    | Additional disease information if "Others" is selected |

### `uploads` table

| Column      | Type    | Description                                  |
|-------------|---------|----------------------------------------------|
| id          | INTEGER | Primary Key, Auto-incrementing               |
| user_id     | INTEGER | Foreign key referencing the `users` table    |
| filename    | TEXT    | Name of the uploaded file                    |
| filepath    | TEXT    | Path to the uploaded file                    |
| upload_date | TEXT    | Date of the upload (YYYY-MM-DD)              |

## AI-Powered Nutrition Analysis

The application uses the Gemini 2.5 Flash model to analyze the nutritional content of food from images. The `analyze_image` function in `app.py` sends the uploaded images to the Gemini API along with a detailed prompt.

The prompt is engineered to provide a comprehensive report tailored to the user's health profile (age, weight, gender, and known diseases). The model is instructed to act as a nutrition expert and provide the following information in Markdown format:

-   **Identified Food Items and Quantities**
-   **Nutritional Content**
-   **Comparison: Daily Intake vs Recommended**
-   **Recommendations: To Pick/Drop**

This allows the user to get personalized and actionable insights into their diet.

## Getting Started

Follow these instructions to get a local copy of the application up and running.

### Prerequisites

- Python 3.x
- `pip` for installing dependencies

### Setup and Installation

1.  **Clone the repository:**
    ```bash
    git clone <repository-url>
    cd <repository-directory>
    ```

2.  **Create and activate a virtual environment (recommended):**
    ```bash
    python -m venv venv
    source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
    ```

3.  **Install the dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

4.  **Set up the Gemini API Key:**
    To use the AI analysis feature, you need to obtain a Gemini API key from Google AI Studio and set it as an environment variable. Create a `.env` file in the root of the project and add your API key:
    ```
    GEMINI_API_KEY='your_api_key_here'
    ```

### Database Initialization

Before running the application for the first time, you need to initialize the SQLite database. This will create the `database.db` file and set up the necessary tables, including the default admin user.

Run the following command in your terminal:
```bash
python init_db.py
```

### Running the Application

Once the database is initialized, you can start the Flask development server.

Run the following command:
```bash
python app.py
```

The application will be available at `http://127.0.0.1:5000` in your web browser.

## Usage

1.  **Sign up** for a new account or **log in** with existing credentials.
2.  Update your **profile** with your age, gender, weight, and any known diseases.
3.  Go to the **Upload** page to upload images of your meals.
4.  After uploading, you can view your uploads on your **profile** page, grouped by date.
5.  Click on a date to view the images for that day.
6.  Click the **Analyze** button to get a nutritional report for the images from that day.
7.  As an admin, you can log in with the credentials below to view and manage users.

## Testing

The project uses `pytest` for testing. To run the tests, execute the following command:

```bash
python run_tests.py
```

## Admin Credentials

A default admin user is created when you initialize the database. You can use these credentials to log in to the admin dashboard.

-   **Username:** `admin`
-   **Password:** `adminpwd`

## Dependencies

- Flask
- MarkupSafe
- Werkzeug
- Flask-WTF
- google-generativeai
- Pillow
- bleach
- pytest-mock
- markdown
- python-dotenv

## Deployment

This application is configured for a development environment. For a production deployment, consider the following:

-   Use a production-ready WSGI server like Gunicorn or uWSGI instead of Flask's built-in development server.
-   Configure a more robust database like PostgreSQL or MySQL.
-   Set the `FLASK_ENV` environment variable to `production`.
-   Disable debug mode.
