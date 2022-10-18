=====
AWSGI
=====

AWSGI allows you to use WSGI-compatible middleware and frameworks like Flask and Django with the `AWS API Gateway/Lambda proxy integration <https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-set-up-simple-proxy.html>`_.

Replaced by an improved fork `awsgi2 <https://gitlab.com/advian-oss/python-awsgi2>`_.

Installation
------------

``awsgi2`` is available from PyPI as ``awsgi2``::

    pip install awsgi2

Examples
--------

Flask
^^^^^

.. code-block:: python

    import awsgi2
    from flask import (
        Flask,
        jsonify,
    )

    app = Flask(__name__)


    @app.route('/')
    def index():
        return jsonify(status=200, message='OK')


    def lambda_handler(event, context):
        return awsgi2.response(app, event, context, base64_content_types={"image/png"})

Django
^^^^^^

.. code-block:: python

    import os
    import awsgi2

    from django.core.wsgi import get_wsgi_application

    # my_app_directory/settings.py is a vanilla Django settings file, created by "django-admin startproject".
    os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'my_app_directory.settings')
    # In the settings.py file, you may find it useful to include this setting to remove
    # Django's need for SQLite, which is currently (2020-11-17) outdated in the Lambda runtime image
    # DATABASES = { 'default': { 'ENGINE': 'django.db.backends.dummy', } }

    application = get_wsgi_application()

    def lambda_handler(event, context):
        return awsgi2.response(application, event, context, base64_content_types={"image/png"})

