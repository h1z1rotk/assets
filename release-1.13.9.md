# Assets 1.13.9

- Keep Exit Match available while spectating an enemy after the Duo is eliminated.
- Remove the Twitch icon from Duo player rows.
- Change the special green voice and killfeed badge to violet.

This update combines the current 1.13.8 client assets with the spectator Exit
fix from returnoftheking#531 and the visual changes from returnoftheking#559.
The separate server/admin update controls who receives the violet badge.

Three payload archives update assets_x64_0, assets_x64_1 and rank_menu_ui.
The latter retains its unchanged ui_x64_2 payload. All six mirrored interface
copies agree; 12,781 unrelated pack entries remain byte-identical. Compiled Exit
transitions and an isolated upgrade using launcher 2.0.14 pass. The second sync
downloads nothing. Native in-game click and rendering acceptance remain pending.

No launcher code update or game-server restart is needed for these assets.
Close the game, let the launcher update, then launch it again.
