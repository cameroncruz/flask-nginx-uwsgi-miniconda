FROM cameroncruz/flask-nginx-uwsgi-miniconda:python3.6

# Create conda env and install dependencies
RUN conda create --name myenv python=3.6
RUN /bin/bash -c ". activate myenv && \
    conda config --add channels conda-forge && \
    conda install -y \
        scikit-learn \
        numpy \
        scipy \
        flask \
        uwsgi"
# NOTE: Make sure you install flask and uwsgi in your conda env!

# Copy custom supervisor config
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Copy application files
# NOTE: The file that launches your Flash app needs to be copied to /app/main.py
COPY server/server.py /app/main.py
