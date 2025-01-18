// Copyright (c) Zefchain Labs, Inc.
// SPDX-License-Identifier: Apache-2.0

fn main() {
    cfg_aliases::cfg_aliases! {
        web: { all(target_arch = "wasm32", feature = "web") },

        with_fs: { all(not(target_arch = "wasm32"), feature = "fs") },
        with_metrics: { all(not(target_arch = "wasm32"), feature = "metrics") },
        with_testing: { any(test, feature = "test") },
        with_tokio_multi_thread: { not(target_arch = "wasm32") },
        with_wasmer: { feature = "wasmer" },
        with_wasmtime: { all(not(target_arch = "wasm32"), feature = "wasmtime") },

        // If you change this, don't forget to update `WasmRuntime` and
        // `WasmRuntime::default_with_sanitizer`
        with_wasm_runtime: { any(with_wasmer, with_wasmtime) },
    }
}
#!/bin/bash -e

cd `dirname $0`/..

(cd examples && cargo build --release)

cp examples/target/wasm32-unknown-unknown/release/counter_{contract,service}.wasm linera-execution/tests/fixtures
// Copyright (c) Zefchain Labs, Inc.
// SPDX-License-Identifier: Apache-2.0

fn main() {
    cfg_aliases::cfg_aliases! {
        with_testing: { any(test, feature = "test") },
        with_wasm_runtime: { any(feature = "wasmer", feature = "wasmtime") },
        with_integration_testing: {
            all(not(target_arch = "wasm32"), with_testing, with_wasm_runtime)
        },
    };
}
