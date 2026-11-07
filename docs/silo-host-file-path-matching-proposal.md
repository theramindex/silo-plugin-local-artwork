# Local metadata matching: resolved proposal

Status: superseded by Silo's built-in local NFO tier. Reconciled on 2026-11-06.
No additional host change is proposed by this document.

The original draft requested a path-aware local attempt before remote matching,
including acceptance of a title-only adjacent NFO. Silo now implements this in
its built-in NFO provider. That provider owns text metadata and identity;
Local Artwork supplies adjacent artwork.

## Current contract

- Silo's local NFO tier runs before remote providers. Locked user fields retain
  precedence, then local values, remote values, and filename fallbacks.
- A title-only NFO can yield a matched local item with a deterministic local
  content ID. A local title alone does not declare a trusted remote identity.
  Remote search candidates without such an identity cannot overwrite it.
- The host forwards `GetMetadata.file_path` to plugins. Local Artwork uses that
  exact path to find adjacent images. Its search also accepts explicit
  `_filepath`, `file_path`, or `filepath` provider-ID hints when supplied;
  this does not mean every title-search request includes a path.
- Posters and backdrops alone do not establish a movie's textual identity.
  Use Silo's NFO support for curated titles, descriptions, dates, and IDs.
- A cold artwork lookup containing only an opaque provider ID still needs a
  file-path hint or a previously resolved path. That limitation is not a
  request to duplicate the host's library database inside the plugin.

## Evidence

Silo source inspected at commit
[`4d3dbca0f5396f7dec0c7a1ca603085c0fa45c57`](https://github.com/theramindex/silo-server/commit/4d3dbca0f5396f7dec0c7a1ca603085c0fa45c57):

- [Local NFO architecture and title-only matching](https://github.com/theramindex/silo-server/blob/4d3dbca0f5396f7dec0c7a1ca603085c0fa45c57/docs/architecture/local-nfo-metadata.md)
- [Plugin metadata adapter, including file-path forwarding](https://github.com/theramindex/silo-server/blob/4d3dbca0f5396f7dec0c7a1ca603085c0fa45c57/internal/metadata/plugin_provider.go)

Plugin contract: [README](../README.md) and
[`provider/provider.go`](../provider/provider.go). This closes the old Discord
draft; it does not claim a live server deployment or configuration check.
