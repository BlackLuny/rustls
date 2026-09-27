# ZFC patch to rustls 0.23.44

This branch starts from the upstream `v/0.23.44` tag (`64ad386785c718c74262b79e6893813db381adfe`) with a narrow patch for ZFC issue #650. The patch changes only `rustls/src/conn.rs` and `rustls/src/msgs/deframer/buffers.rs`.

`ConnectionCommon::set_incoming_record_buffer_reuse(true)` opts a complete-record caller into a one-slot, bounded thread-local cache for the incoming deframer allocation. Empty buffers are recycled after `process_new_packets`; partial records stay with their connection. The default remains rustls's original growth and shrink policy. ZFC enables the option only in `RustlsOuterStream` after its server handshake.

When updating rustls, compare the two changed files against the new upstream release, then rerun the VLESS TLS correctness and Worker20 CPU/RSS tests recorded in ZFC's `docs/testing/issue650-tls-hotspot-report.md`.
