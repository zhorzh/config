from python:3.6.5-slim

# install dependencies
copy ./requirements.txt /srv/requirements.txt
workdir /srv
run pip3 install -r requirements.txt

# copy app code to container
copy /app /srv/app

# expose Flask port
expose 80
