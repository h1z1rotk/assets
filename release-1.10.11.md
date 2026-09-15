# Assets 1.10.11 — Duos and queue recovery

Compose native friends/search/consent, group invitations and Duos death/spectate into both UIRoot copies. Include Combat Zone FULL state and cancellation recovery (#454), with V1 fallback while older servers await deployment.

Based on assets 1.10.10: retain Killer notifications (#451), spectator fonts, staff badges, Play Again, CZ death screen, voice and all unrelated entries. Only UIRoot changes in assets_x64_0.pack2 and ui_x64_0.pack2; rank_menu_ui also retains its unchanged ui_x64_2.pack2. All other feed owners are unchanged.

The source recipe is returnoftheking#455, using FFDec 26.2.1 with compile/decompile verification. Packs are rebuilt with unrelated-entry verification; the feed builder validates ZIP content, file ownership, size and SHA-256. Payloads use .payload to avoid the launcher's conventional ZIP overlay. Update the immutable server attestation policy after checking the published feed and release metadata, retaining the current enforcement setting and supported launcher roots.

Launcher synchronization downloads these assets on Play; no launcher executable update is required. Duos matchmaking additionally requires the server/API/web release, migrations and regional fleet installation described in returnoftheking/infra/game2/DUOS.md. Publishing assets does not open Duos or deploy game servers. Native full-match acceptance is separate from compile/protocol validation.
