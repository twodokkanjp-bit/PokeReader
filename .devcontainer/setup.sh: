#!/bin/bash
set -e
git submodule update --init --recursive 2>/dev/null || true

# PokeReader's Makefile pins a specific dated Rust nightly (e.g. nightly-2024-03-21).
# This detects whatever's currently pinned and installs exactly that.
PIN=$(grep -ohE 'nightly-[0-9]{4}-[0-9]{2}-[0-9]{2}' Makefile 2>/dev/null | head -n1)
if [ -n "$PIN" ]; then
  echo "Installing pinned toolchain: $PIN"
  rustup toolchain install "$PIN"
  rustup component add rust-src --toolchain "$PIN"
else
  echo "No dated nightly pin found — using default nightly"
fi
echo "Setup finished. Run: make"
