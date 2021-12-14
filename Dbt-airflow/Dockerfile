FROM apache/airflow:2.2.1-python3.7

USER root

RUN apt-get update && apt-get install -y --no-install-recommends \
    libpq-dev python-dev \
    unixodbc-dev gcc g++ \
    git-all 

RUN ["apt-get", "update"]
RUN ["apt-get", "install", "-y", "vim"]

COPY ./requirements.txt ./requirements.txt
RUN pip install --no-cache-dir -r ./requirements.txt

COPY ./scripts/ ./scripts/
COPY ./dbt-requirements.txt ./dbt-requirements.txt

RUN chmod a+x ./scripts/dbt_install.sh
RUN ./scripts/dbt_install.sh ./dbt-requirements.txt

# DAGs and DBT code will be accessible in the mounted fileshare
# COPY ./airflow/dags /opt/airflow/dags
# COPY ./dbt /opt/airflow/dbt

USER airflow

CMD [ "airflow-webserver" "&&" "airflow-scheduler" ]
