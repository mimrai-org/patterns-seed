# Select compilation by consumer needs

Each contract package deliberately chooses source-exported TypeScript, compiled output, or generated
artifacts. Source exports minimize configuration when every consumer transpiles them; compiled output
adds a cacheable build for heterogeneous JavaScript tools; generated artifacts support protocol-first
or cross-language consumers. TypeScript project references are optional compilation machinery and may
not become a second dependency graph that disagrees with package manifests.
