# flask-nginx-uwsgi-miniconda
Docker image for creating web applications in Python using Flask, uWGSI, Nginx and miniconda.

Miniconda is included for package and environment management intended for data scientists and machine learning engineers.

Currently supports Python 3.6, building on top of [tiangolo/uwsgi-nginx-flask](https://github.com/tiangolo/uwsgi-nginx-flask-docker).

## Usage
```sh
FROM cameroncruz/flask-nginx-uwsgi-miniconda:python3.6
```
Creating a conda env and installing dependencies (you must install Flask and uWSGI to your env as well):
```sh
RUN conda create --name myenv python=3.6
RUN /bin/bash -c ". activate myenv && \
    conda config --add channels conda-forge && \
    conda install -y \
        scikit-learn \
        numpy \
        scipy \
        flask \
        uwsgi"
```

You must create a custom supervisor config, replacing "myenv" with the name of your conda environment:
```sh
# suporvisord.conf
[program:uwsgi]
environment=PATH='/opt/conda/envs/myenv/bin'
command=/opt/conda/envs/myenv/bin/uwsgi --ini /etc/uwsgi/uwsgi.ini --die-on-term --need-app
```

Copy custom supervisor config:
```sh
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
```

Finally copy application files:
```sh
# NOTE: The file that launches your Flash app needs to be copied to /app/main.py
COPY server/server.py /app/main.py
```

Building and running the container:
```sh
docker build -t my_image .
docker run -p 80:80 -t my_image
```

Refer to the example in this repo for further clarification.
