# Heroku Demo App

This demo, performed during MotM-13, shows the creation of a simple app, deployed to Heroku.  

## Prerequisites

- `pipenv` is installed in your host Python 3 installation.
- You have Git installed.
- You have the Heroku CLI installed and logged into your account.
- You've created a separate empty directory (other than this one) and you're ready to build things up for yourself!

## Setup

1. Create a virtual environment with `pipenv`:

        $ pipenv shell

2. Install Django framework and Whitenoise:

        $ pipenv install Django whitenoise

    > See [this link](https://devcenter.heroku.com/articles/django-assets) for information on Django on Heroku and Static Assets like HTML, CSS and JS.

3. Create a new Django project:

        $ django-admin startproject demo .

4. Apply initial database migrations (to local SQLite file):

        $ python manage.py migrate

5. Preview the site (and then open http://localhost:8000 to verify):

        $ python manage.py runserver

6. Break out of the development server with `CTRL-C` and then create a `Procfile` with the contents below (or copy from this folder):

        web: python manage.py runserver 0.0.0.0:$PORT

    > The `$PORT` will allow Heroku to dynamically assign a port when launching the application in a _Dyno_.  Specifying `0.0.0.0` for the IP binding makes sure that it binds to any available interface within the application _Dyno_.

7. Add a `static` folder for holding static assets:

        $ mkdir static
        $ touch static/.empty

    > The `.empty` file is an empty file that exists in order to ensure that _git_ will create that directory on Heroku (since _git_ doesn't pay attention to empty directories).  I'd recommend running these and the preceding commands under _Git Bash_ if you're on Windows for compatibility reasons.

8. Add the following content to the end of the `settings.py` in the `demo` folder:

    ```python
    STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')

    # Extra places for collectstatic to find static files.
    STATICFILES_DIRS = (
        os.path.join(BASE_DIR, 'static'),
    )
    ```

9. While you're in the `settings.py` file, update the `ALLOWED_HOSTS` definition to look like below:

    ```python
    ALLOWED_HOSTS = ['*']
    ```

    > Since we're running on Heroku and the app name might be dynamic, we open this up to allow any host to run the Django app.

10. Copy in the `.gitignore` from this directory (taken from [this example](https://github.com/github/gitignore/blob/master/Python.gitignore) on GitHub)

11. Initialize a Git Repo:

        $ git init

12. Add all files to be tracked in Git:

        $ git add .

13. Commit!

        $ git commit -m "Initial commit"

14. Create a Heroku app:

        $ heroku create

15. Deploy the app:

        $ git push heroku master

16. Scale to at least 1 instance:

        $ heroku ps:scale web=1

17. Open the app (running now on Heroku at this point) with:

        $ heroku open

    > This is a convenient shortcut that will launch your web browser targeted against the URL for your application on Heroku.

## Next Steps

At this point in the demo, we discuss the ephemeral nature of a Heroku Dyno (where your application runs) and the next steps for persistent data storage for your applications.  The next steps would involve finishing hooking up to the [already available] Postgres instance and then actually creating your application!  Hope this was fun!