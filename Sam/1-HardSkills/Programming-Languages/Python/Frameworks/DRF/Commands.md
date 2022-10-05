```
python3 -m venv venv
source /venv/bin/activate

pip install django
pip install djangorestframework

django-admin startproject core .
django-admin startapp [app_name]

./manage.py migrate
```