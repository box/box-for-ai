# TypeScript and Node SDK

- Inspect `package.json`, lockfile, and installed `box-node-sdk` declarations before using examples. Box Node SDK v10 is the generated interface for new applications; v4 supports gradual migration from the older interface. The separate `box-typescript-sdk-gen` artifact is deprecated. Do not mix v3/v4 manual-client calls with v10 generated calls.
- Follow the project's module format and TypeScript strictness. Import methods and request types from the installed package, pass typed option objects as declared, and check whether a method returns a stream, response wrapper, or parsed resource before piping it.
- Keep server credentials and token refresh on the server. For browser code, use a user-scoped authorization flow and verify CORS and the SDK's browser support; do not bundle a client secret or service-account key.
- Use one shared limiter for requests under the same Box actor, including calls made through different route handlers or background workers where possible. Await streams through completion and handle aborts, cleanup, and partial files.
- Verify with `tsc` or the project's build and a focused call-shape test. For transfer code, test streaming and interruption with a small file; for bulk code, test multiple pages and a 429 with `Retry-After`.

Sources: [Node SDK versions and usage](https://github.com/box/box-node-sdk), [migration guide](https://github.com/box/box-node-sdk/blob/main/migration-guides/from-box-node-sdk-to-sdk-gen.md).
