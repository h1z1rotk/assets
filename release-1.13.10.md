# Assets 1.13.10 — Duo map pins on the compass

Teammate map pins now remain visible on the compass even when team-player
indicators are hidden, including while spectating. The existing team color,
own pin, compass layout, rotation, POI and gas settings are preserved.

All three copies of `HudCompassWindow.gfx` are updated. The rebuilt selection
method passes 16 setting/spectator combinations; the previous interface
reproduces the missing teammate pin. The other 91 script classes and 12,784
pack assets are unchanged, as is the UI2 payload.

The launcher installer is checked in an isolated client against published
version 2.0.14. Close the game, update through the launcher and launch again.
No game-server reboot is needed. The server relay is already deployed for
newly created Duo lobbies; previously created games retain their loaded code.

Source: [returnoftheking#561](https://github.com/h1z1rotk/returnoftheking/pull/561).
Native visual confirmation follows the client update.
