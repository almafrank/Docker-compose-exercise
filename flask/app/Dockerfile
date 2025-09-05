# version av Dockerfile-syntax som används-python alpine container
FROM --platform=$BUILDPLATFORM python:3.10-alpine AS builder

#Ställer in /app som arbetskatalog inuti containern.Alla COPY och RUN-kommandon sker i /app
WORKDIR /app
# Kopiera beroenden och installera:kopierar filen som innehåller Python-bibliotek.
#Installerar Python-paket med pip och cachar nedladdade paket för snabbare build nästa gång.
COPY requirements.txt /app
RUN --mount=type=cache,target=/root/.cache/pip \
    pip3 install -r requirements.txt

#Kopierar hela projektmappen in i /app
COPY . /app

#anger att alla kommandon startas med python3
ENTRYPOINT ["python3"]
#standardfil som körs när containern startar
CMD ["app.py"]

FROM builder as dev-envs

#Installera paket
RUN <<EOF
apk update
apk add git
EOF

#Skapa användare
RUN <<EOF
addgroup -S docker
adduser -S --shell /bin/bash --ingroup docker vscode
EOF

# install Docker tools (cli, buildx, compose)
COPY --from=gloursdocker/docker / /
