# Quickstart



```shell
# 先配置虚拟环境
# Install Django
pip3 install django=1.8.2
# or
pip3 install django==1.8.2
# which one is correct, = or ==?
# Note: Do not use sudo pip, it will install the packages into the real environment 
# instead of the virtual environment!

# Create a project called test1
django-admin startproject test1

# A django project consists of multiple apps.
# Create an app with name booktest
python manage.py startapp booktest
# Question: what is the difference between the way above and below?
# python-admin startapp booktest
# The above one is used in doc: https://docs.djangoproject.com/en/4.0/intro/tutorial01/

# Register the app in the project
# Configure the INSTALLED_APPS in the settings.py file
INSTALLED_APPS = (
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'booktest', # register the app
)

# Launch the project
python manage.py runserver





# 编写好models.py文件后
# create migrations (migration files)
python manage.py makemigrations

# apply the migration files to the database
python manage.py migrate



# Django Shell
python3 manage.py shell






# sqlite3
sqlite3 db.sqlite3
# list all tables
.tables
# only print out 10 items
select * from cats_cat limit 10;
select count(*) from cats_breed;
# quit
.exit





# Install django-extensions
pip3 install django-extensions
```



