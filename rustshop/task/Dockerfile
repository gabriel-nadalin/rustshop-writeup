FROM rust:slim

WORKDIR /app

COPY ./server .

ENV FLAG="corctf{we_d0_s0me_s3rde_shen4nigans}"

RUN cargo build --release

CMD ["./target/release/rustshop"]
