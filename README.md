Anleitung zur Einrichtung eines Arma-3-Servers mit Docker

Diese Anleitung beschreibt die Schritte zur Einrichtung eines Arma-3-Servers unter Verwendung von Docker. Der Server unterstützt die folgenden DLCs und Mods:

DLCs:

    Apex
    Jets
    Helicopters
    Karts
    Marksman
    Malden
    Zeus
    Tanks
    Global Mobilization - Cold War Germany
    Western Sahara
    Art of War Charity Pack

Mods:

    @3CB Factions
    @ace
    @Animate - Rewrite
    @Animated Grenade Throwing
    @Antistasi Ultimate - Mod
    @Better Inventory
    @Blastcore Murr Edited
    @Brighter Flares
    @Brighter Flares (ACE Compatibility Patch)
    @CBA_A3
    @CE - MAIN
    @Enhanced Map Ace Version
    @Enhanced Movement
    @Enhanced Soundscape
    @Immerse
    @Improved Game Sounds
    @Integrated AI Voice Control System - Control AI with your voice!
    @Real Lighting and Weather
    @RHSAFRF
    @RHSGREF
    @RHSSAF
    @RHSUSAF
    @SigSound ArmA 3
    @Speshal Core
    @Suppress

Der Server ist für 16 Spieler konfiguriert und verwendet die Karte Tanoa mit der Mission "Antistasi Ultimate Tanoa".
Voraussetzungen

    Docker: Installieren Sie Docker entsprechend der Anleitung für Ihr Betriebssystem.
    Docker Compose: Installieren Sie Docker Compose gemäß der offiziellen Dokumentation.

Verzeichnisstruktur

Erstellen Sie ein Hauptverzeichnis für Ihren Arma-3-Server und darin die folgenden Unterverzeichnisse:

arma3-server/
├── configs/
├── mpmissions/
├── mods/
├── servermods/
└── keys/

Konfigurationsdateien
1. .env-Datei

Legen Sie im Hauptverzeichnis eine Datei namens .env mit folgendem Inhalt an:

# Steam-Anmeldeinformationen
STEAM_USER=IhrSteamBenutzername
STEAM_PASSWORD=IhrSteamPasswort

# Server-Konfiguration
ARMA_CONFIG=server.cfg
ARMA_PROFILE=server
ARMA_WORLD=Tanoa
ARMA_PORT=2302

# DLCs
ARMA_CDLC="gm;ws"

# Mods
ARMA_MODS="@3cb_factions;@ace;@animate_rewrite;@animated_grenade_throwing;@antistasi_ultimate_mod;@better_inventory;@blastcore_murr_edited;@brighter_flares;@brighter_flares_ace_compat;@cba_a3;@ce_main;@enhanced_map_ace;@enhanced_movement;@enhanced_soundscape;@immerse;@improved_game_sounds;@integrated_ai_voice_control;@real_lighting_and_weather;@rhsafrf;@rhsgref;@rhssaf;@rhsusaf;@sigsound_arma3;@speshal_core;@suppress"

Hinweis: Ersetzen Sie IhrSteamBenutzername und IhrSteamPasswort durch Ihre tatsächlichen Steam-Anmeldedaten.
2. docker-compose.yml

Erstellen Sie im Hauptverzeichnis eine Datei namens docker-compose.yml mit folgendem Inhalt:

version: '3.8'

services:
  arma3server:
    image: ghcr.io/brettmayson/arma3server:latest
    ports:
      - "${ARMA_PORT}:${ARMA_PORT}/udp"
      - "$((ARMA_PORT+1)):$((ARMA_PORT+1))/udp"
      - "$((ARMA_PORT+2)):$((ARMA_PORT+2))/udp"
      - "$((ARMA_PORT+3)):$((ARMA_PORT+3))/udp"
    volumes:
      - ./configs:/arma3/configs
      - ./mpmissions:/arma3/mpmissions
      - ./mods:/arma3/mods
      - ./servermods:/arma3/servermods
      - ./keys:/arma3/keys
    environment:
      - STEAM_USER=${STEAM_USER}
      - STEAM_PASSWORD=${STEAM_PASSWORD}
      - ARMA_CONFIG=${ARMA_CONFIG}
      - ARMA_PROFILE=${ARMA_PROFILE}
      - ARMA_WORLD=${ARMA_WORLD}
      - ARMA_CDLC=${ARMA_CDLC}
      - MODS_LOCAL=true
      - ARMA_MODS=${ARMA_MODS}
    restart: unless-stopped

3. server.cfg

Legen Sie im Verzeichnis configs eine Datei namens server.cfg mit folgendem Inhalt an:

// Server-Konfigurationsdatei

hostname           = "Arma 3 Server";
password           = "";
passwordAdmin      = "AdminPasswort";
maxPlayers         = 16;
persistent         = 1;
verifySignatures   = 2;
battleye           = 1;
allowedFilePatching= 1;
voteThreshold      = 0.33;
voteMissionPlayers = 1;
vonCodecQuality    = 30;
disableVoN         = 0;
kickDuplicate      = 1;
logFile            = "server_console.log";

class Missions
{
    class Mission1
    {
        template = "Antistasi_Ultimate.Tanoa";
        difficulty = "Custom";
    };
};

Hinweis: Ersetzen Sie AdminPasswort durch ein sicheres Passwort Ihrer Wahl.
Installation der Mods und Missionen

    Mods: Laden Sie die gewünschten Mods herunter und platzieren Sie sie im Verzeichnis mods. Achten Sie darauf, dass die Ordnernamen keine Leerzeichen enthalten und in Kleinbuchstaben geschrieben sind.

    Mission: Laden Sie die Mission "Antistasi Ultimate Tanoa" herunter und platzieren Sie die .pbo-Datei im Verzeichnis mpmissions.

Hinweis: Stellen Sie sicher, dass die Mods und Missionen kompatibel sind und korrekt installiert wurden.
Starten des Servers

Navigieren Sie im Terminal zum Hauptverzeichnis arma3-server und führen Sie folgenden Befehl aus:

docker-compose up -d

