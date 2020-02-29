
데이터베이스 전체 초기화

To solve it I did this (on Ubuntu, you'll need to adapt the commands for Windows):

1. Remove all the migration files

find . -path "*/migrations/*.py" -not -name "__init__.py" -delete

find . -path "*/migrations/*.pyc"  -delete
2. Delete db.sqlite3

rm db.sqlite3
3. Create and run the migrations:

python manage.py makemigrations
python manage.py migrate
4. Sync the database:

manage.py migrate --run-syncdb

