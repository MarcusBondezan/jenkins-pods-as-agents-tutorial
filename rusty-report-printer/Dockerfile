FROM rustlang/rust:nightly-alpine as build

RUN apk add --no-cache musl-dev=1.2.2-r7

ENV CARGO_UNSTABLE_SPARSE_REGISTRY=true

WORKDIR /app
COPY ./Cargo* .
COPY src ./src
RUN cargo build --locked --release

FROM alpine:3.15

WORKDIR /app

COPY --from=build /app/target/release/rusty-report-printer .
COPY ./resources ./resources

RUN apk add --no-cache tini=0.19.0-r0
RUN adduser -D -H -s /bin/sh -g "" report

USER report

ENTRYPOINT ["tini", "--"]
CMD ["./rusty-report-printer"]
