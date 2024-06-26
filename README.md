# Django Poll App

This is a simple web-based poll application built using Django. This application allows users to create polls with multiple choices, and it includes features for managing the polls through the Django admin interface.

## Features

- Create and manage polls with multiple choices.
- Customize the admin interface to improve usability.
- Serve static files like CSS and images.
- Add search and filter functionality to the admin change list.

## Requirements

- Python 3.x
- Django 5.0

## Installation

1. **Clone the repository:**

    ```sh
    git clone https://github.com/Khushal-Me/Poll-App-Utilising-Django.git
    cd django-poll-app
    ```

2. **Create a virtual environment and activate it:**

    ```sh
    python -m venv env
    source env/bin/activate  # On Windows, use `env\Scripts\activate`
    ```

3. **Install the required packages:**

    ```sh
    pip install -r requirements.txt
    ```

4. **Apply migrations:**

    ```sh
    python manage.py migrate
    ```

5. **Create a superuser:**

    ```sh
    python manage.py createsuperuser
    ```

6. **Run the development server:**

    ```sh
    python manage.py runserver
    ```

7. **Access the application:**

    Open a web browser and go to `http://localhost:8000/polls/`.

## Usage

### Customizing the Admin Interface

1. **Reorder fields on the edit form:**

    In `polls/admin.py`:

    ```python
    from django.contrib import admin
    from .models import Question

    class QuestionAdmin(admin.ModelAdmin):
        fields = ["pub_date", "question_text"]

    admin.site.register(Question, QuestionAdmin)
    ```

2. **Add fieldsets:**

    ```python
    class QuestionAdmin(admin.ModelAdmin):
        fieldsets = [
            (None, {"fields": ["question_text"]}),
            ("Date information", {"fields": ["pub_date"]}),
        ]

    admin.site.register(Question, QuestionAdmin)
    ```

3. **Inline Choices with Questions:**

    ```python
    from .models import Choice

    class ChoiceInline(admin.TabularInline):
        model = Choice
        extra = 3

    class QuestionAdmin(admin.ModelAdmin):
        fieldsets = [
            (None, {"fields": ["question_text"]}),
            ("Date information", {"fields": ["pub_date"], "classes": ["collapse"]}),
        ]
        inlines = [ChoiceInline]

    admin.site.register(Question, QuestionAdmin)
    ```

4. **Customize the change list:**

    ```python
    class QuestionAdmin(admin.ModelAdmin):
        list_display = ["question_text", "pub_date", "was_published_recently"]
        list_filter = ["pub_date"]
        search_fields = ["question_text"]

    admin.site.register(Question, QuestionAdmin)
    ```

### Adding Static Files

1. **Create a static directory:**

    ```sh
    mkdir -p polls/static/polls
    ```

2. **Add a stylesheet:**

    Create `polls/static/polls/style.css`:

    ```css
    li a {
        color: green;
    }
    ```

    Reference it in `polls/templates/polls/index.html`:

    ```html
    {% load static %}
    <link rel="stylesheet" href="{% static 'polls/style.css' %}">
    ```

3. **Add a background image:**

    Create `polls/static/polls/images/` and add your image (e.g., `background.png`). Update `polls/static/polls/style.css`:

    ```css
    body {
        background: white url("images/background.png") no-repeat;
    }
    ```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
