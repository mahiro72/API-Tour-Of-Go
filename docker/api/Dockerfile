# develop
FROM golang:1.19 AS develop

WORKDIR /go/src/github.com/mahiro72/API-Tour-Of-Go

COPY go.mod ./
COPY go.sum ./

RUN go mod download

COPY . .

RUN go install github.com/cosmtrek/air@latest

COPY docker/api/.air.toml /air/.air.toml

CMD ["air", "-c", "/air/.air.toml"]

## Build
FROM golang:1.19 AS build

WORKDIR /go/src/github.com/mahiro72/API-Tour-Of-Go

COPY go.mod ./
COPY go.sum ./

RUN go mod download

COPY . .

RUN go build -o /server ./cmd/api

## Deploy
FROM gcr.io/distroless/base-debian10

WORKDIR /

COPY --from=build /server /server

EXPOSE 8080

USER nonroot:nonroot

ENTRYPOINT ["/server"]