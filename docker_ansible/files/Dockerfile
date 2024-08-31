FROM python:3.12 AS base
COPY ./pyproject.toml ./
RUN apt-get update && apt-get install -y build-essential git python3-dev libffi-dev vim systemd
RUN pip install poetry
RUN poetry config virtualenvs.create false
RUN poetry install --no-dev

FROM python:3.12
ARG APP_VERSION
ENV APP_VERSION=${APP_VERSION}
LABEL MAINTAINER=Ori_Nahum VERSION=${APP_VERSION}
WORKDIR /home/app
COPY --from=base /usr/local/lib/python3.12/site-packages /usr/local/lib/python3.12/site-packages
COPY --from=base /usr/local/bin /usr/local/bin
COPY . /home/app/
EXPOSE 8000
# EXPOSE 465
CMD [ "gunicorn", "--config", "/home/app/gunicorn_conf.py", "src.details:app" ]
