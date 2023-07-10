FROM rust:1.70-slim-buster as builder

# Install compilation dependencies
RUN apt-get update \
 && apt-get -y install --no-install-recommends libssl-dev pkg-config libpq-dev git \
 && rm -rf /var/lib/apt/lists/*

WORKDIR /app

# comma-seperated list of features to enable
ARG CARGO_BUILD_FEATURES=default

# This can be set to release using --build-arg
ARG RUST_RELEASE_MODE="release"

COPY . .

# Build the project

# Debug mode build
RUN --mount=type=cache,target=/app/target \
    if [ "$RUST_RELEASE_MODE" = "debug" ] ; then \
      echo "pub const VERSION: &str = \"$(git describe --tag)\";" > "crates/utils/src/version.rs" \
      && echo "Building Lemmy $(git describe --tag), Cargo Target: $(rustc -vV | sed -n 's|host: ||p'), Mode: $RUST_RELEASE_MODE" \
      && cargo build --features ${CARGO_BUILD_FEATURES} \
      && cp ./target/$RUST_RELEASE_MODE/lemmy_server /app/lemmy_server; \
    fi

# Release mode build
RUN \
    if [ "$RUST_RELEASE_MODE" = "release" ] ; then \
      echo "pub const VERSION: &str = \"$(git describe --tag)\";" > "crates/utils/src/version.rs" \
      && echo "Building Lemmy $(git describe --tag), Cargo Target: $(rustc -vV | sed -n 's|host: ||p'), Mode: $RUST_RELEASE_MODE" \
      && cargo build --features ${CARGO_BUILD_FEATURES} --release \
      && cp ./target/$RUST_RELEASE_MODE/lemmy_server /app/lemmy_server; \
    fi

# The Debian runner
FROM debian:buster-slim as lemmy

# Install libpq for postgres
RUN apt-get update \
 && apt-get -y install --no-install-recommends postgresql-client libc6 libssl1.1 ca-certificates \
 && rm -rf /var/lib/apt/lists/*

RUN addgroup --gid 1000 lemmy
RUN useradd --no-create-home --shell /bin/sh --uid 1000 --gid 1000 lemmy

# Copy resources
COPY --chown=lemmy:lemmy --from=builder /app/lemmy_server /app/lemmy

RUN chown lemmy:lemmy /app/lemmy
USER lemmy

CMD ["/app/lemmy"]
