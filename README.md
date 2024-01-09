# Rust on BBB Blink an LED

### Build Setup

Didn't have luck with the `cross` tool directly so I had to clone the `cross-rs` repo (and submodules).

> Have to have docker running

```bash
git clone git@github.com:cross-rs/cross.git
cd cross
git submodule update --init --remote
```

Setup xtask
```bash
cargo xtask configure-crosstool
```

Then run the build-docker-image (note: tagged local)
```bash
cargo build-docker-image armv7-unknown-linux-gnueabihf --tag local
```

Then went back into this project dir...

Made a [Cross.toml](./Cross.toml) with the following (note local tag):
```toml
[target.armv7-unknown-linux-gnueabihf]
image = "ghcr.io/cross-rs/armv7-unknown-linux-gnueabihf:local"
```

So that finally the cross build command would work:
```bash
cross build --release --target armv7-unknown-linux-gnueabihf
```

Could then scp the binary across:
```bash
scp target/armv7-unknown-linux-gnueabihf/release/bbb-rust-led debian@beaglebone.local:~/
```