# set base image (host OS)
FROM python:rc-alpine

RUN apk add mosquitto
RUN apk add curl
RUN apk add --update npm
RUN npm install -g serve
RUN apk add git
RUN apk add tzdata
RUN apk add musl-utils
RUN apk add xsel
RUN apk add redis
RUN apk add npm
RUN apk add nginx && mkdir -p /run/nginx

# set the working directory in the container
WORKDIR /app

# copy the dependencies file to the working directory
COPY requirements.txt .
RUN pip install -r requirements.txt

COPY givtcp-vuejs/package.json ./config_frontend/package.json

#RUN cd /app/config_frontend && npm install
#COPY givtcp-vuejs ./config_frontend
#RUN cd /app/config_frontend && npm run build

COPY ingress.conf /etc/nginx/http.d/
RUN rm /etc/nginx/http.d/default.conf

# copy the content of the local src directory to the working directory
COPY GivTCP/ ./GivTCP
COPY WebDashboard/ ./WebDashboard
COPY givenergy_modbus/ /usr/local/lib/python3.10/site-packages/givenergy_modbus

COPY startup.py startup.py
COPY startup_3.py startup_3.py
COPY redis.conf redis.conf
COPY settings.json /app/settings.json
COPY index.html /app/index.html

ENV NUMINVERTORS=1
ENV INVERTOR_IP_1=""
ENV NUMBATTERIES_1=1
ENV INVERTOR_AIO_1=False
ENV INVERTOR_AC_1=True
ENV INVERTOR_IP_2=""
ENV NUMBATTERIES_2=1
ENV INVERTOR_AIO_2=False
ENV INVERTOR_AC_2=True
ENV INVERTOR_IP_3=""
ENV NUMBATTERIES_3=1
ENV INVERTOR_AIO_3=False
ENV INVERTOR_AC_3=True
ENV MQTT_OUTPUT=True
ENV MQTT_ADDRESS=""
ENV MQTT_USERNAME=""
ENV MQTT_PASSWORD=""
ENV MQTT_TOPIC=""
ENV MQTT_TOPIC_2=""
ENV MQTT_TOPIC_3=""
ENV MQTT_PORT=1883
ENV MQTT_RETAIN=False
ENV LOG_LEVEL="Info"
ENV PRINT_RAW=True
ENV SELF_RUN=True
ENV SELF_RUN_LOOP_TIMER=30
ENV INFLUX_OUTPUT=False
ENV INFLUX_URL=""
ENV INFLUX_TOKEN=""
ENV INFLUX_BUCKET=""
ENV INFLUX_ORG=""
ENV HA_AUTO_D=True
ENV HADEVICEPREFIX="GivTCP"
ENV HADEVICEPREFIX_2="GivTCP2"
ENV HADEVICEPREFIX_3="GivTCP3"
ENV PYTHONPATH="/app"
ENV DAYRATE=0.395
ENV NIGHTRATE=0.155
ENV EXPORTRATE=0.04
ENV DYNAMICTARIFF=False
ENV DAYRATESTART="04:30"
ENV NIGHTRATESTART="00:30"
ENV TZ="Europe/London"
ENV WEB_DASH=False
ENV WEB_DASH_PORT=3000
ENV CACHELOCATION="/config/GivTCP"
ENV DATASMOOTHER="medium"
ENV QUEUE_RETRIES=2

ENV SMARTTARGET=False
ENV GEAPI=""
ENV SOLCASTAPI=""
ENV SOLCASTSITEID=""
ENV SOLCASTSITEID2=""
ENV PALM_WINTER="01,02,03,10,11,12"
ENV PALM_SHOULDER="04,05,09"
ENV PALM_MIN_SOC_TARGET=25
ENV PALM_MAX_SOC_TARGET=45
ENV PALM_BATT_RESERVE=4
ENV PALM_BATT_UTILISATION=0.85
ENV PALM_WEIGHT=35
ENV LOAD_HIST_WEIGHT="1"

ENV EVC_ENABLE=False
ENV EVC_IP_ADDRESS=""
ENV EVC_SELF_RUN_TIMER=5


EXPOSE 1883 6379 8099

CMD ["python3", "/app/startup.py"]
