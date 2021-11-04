````
git status
git add -A
git commit -m "New updates for Heroku deployment"
git push -u origin master
````

# Heroku configuration

Вы также должны уже иметь настройки учетной записи Heroku
установленные из главы 3.

Нам необходимо внести следующие изменения в наш проект доски
объявлений, чтобы
развернуть его в интернете:

• обновить Pipfile.lock
• новый Procfile
• установить gunicorn
• обновить settings.py

В вашем **Pipfile** укажите версию Python мы используем 3.9.
Добавьте эти две строки в конец файла.

```bash
# Pipfile

[requires]
python_version = 3.9
```

Запустите `pipenv lock` для создания соответствующего
**Pipfile.lock.**

```shell
pipenv lock
```

Затем создайте **Procfile**, который указывает **Heroku**, как
запустить удаленный сервер, где наш код будет работать.

```shell
touch Procfile
```

Теперь мы укажем **Heroku** использовать **gunicorn** в качестве
нашего рабочего сервера и посмотреть в наш **mb_project.wsgi** файл
для получения дальнейших инструкций.

```bash
cat > Procfile <<ZZZ
web: gunicorn mb_project.wsgi --log-file -
ZZZ
```

Далее установим gunicorn * который мы будем использовать для
production но при этом используя внутренний сервер Django для
локальной разработки.

Command Line

```shell
pipenv install gunicorn
```

Наконец, обновите `ALLOWED_HOSTS` в нашем файле **settings.py** file. Code

```python
mb_project/settings.py
ALLOWED_HOSTS = ['*']
```

```shell
git status
git add -A
git commit -m 'New updates for Heroku deployment'
git push -u origin master
```

## Heroku **развертывание**

Убедитесь, что вы вошли в свою учетную запись **Heroku.**

```shell
heroku login
```
app_name=my-app-easy-quest


```shell
heroku create $app_name
```

```shell
heroku git:remote -a $app_name
```

Укажите **Heroku** игнорировать статические файлы, которые мы
подробно рассмотрим при
развертывании нашего приложения блога в книге позже.

```shell
heroku config:set DISABLE_COLLECTSTATIC=1
```

Отправьте код в Heroku и добавьте бесплатное масштабирование, чтобы
он действительно
работал в интернете, иначе код просто сидит там.

```shell
git push heroku main
heroku ps:scale web=1
```

Если вы откроете новый проект `heroku open,` он автоматически
запустит новое окно браузера с URL вашего приложения. Мой находится
по адресу:
