# ALLERIA FILMY

Prywatna platforma wideo dla społeczności [Alleria.pl](https://alleria.pl) z uwierzytelnianiem Discord/TeamSpeak 3, zarządzaniem filmami, tagami oraz panelami administracyjnymi.

## Funkcjonalności

### Uwierzytelnianie
- **Discord OAuth2** — logowanie przez Discord, sprawdzanie ról, automatyczne przypisywanie uprawnień (member/admin/dev)
- **TeamSpeak 3** — logowanie przez ServerQuery, dopasowanie po IP klienta, sprawdzanie grup serwera

### Filmy
- **YouTube/Embed** — wklejanie linków YouTube z auto-konwersją na embed i thumbnailami
- **Tagi** — system tagów z autocompletem

### Panel Redaktora (Admin)
- Tabela filmów z kolumnami: ID, tytuł, autor, data, akcje
- Dodawanie, edycja i usuwanie filmów
- Zarządzanie tagami

### Dev Panel (Dev only)
- Konsola SQL — bezpośrednie zapytania na bazie danych z wynikami w tabeli
- Tworzenie użytkowników ręcznie
- Czyszczenie bazy danych

## Architektura

```
┌────────────────────────────────────────┐
│  Frontend (React + Tailwind + Vite)    │
│  SPA — sidebar layout, responsive     │
├────────────────────────────────────────┤
│  Backend (Express.js + SQLite)         │
│  REST API, session auth                │
└────────────────────────────────────────┘
```

## Wymagania

- Docker + Docker Compose
- Discord Application (OAuth2 + Bot Token)
- Domena z HTTPS (Cloudflare Tunnel / nginx / traefik)
- TeamSpeak 3 z włączonym ServerQuery

## Instalacja

### 1. Klonowanie

```bash
git clone https://github.com/Alleria-PL/alleria-filmy-platform-2.git
cd alleria-filmy-platform-2
```

### 2. Konfiguracja

```bash
cp .env.example .env
nano .env
```

Wypełnij co najmniej:
- `DISCORD_CLIENT_ID`, `DISCORD_CLIENT_SECRET`, `DISCORD_BOT_TOKEN`
- `DISCORD_REDIRECT_URI` — musi zgadzać się z Discord Developer Portal
- `DISCORD_GUILD_ID`, `DISCORD_MEMBER_ROLE_ID`, `DISCORD_ADMIN_ROLE_ID`, `DISCORD_DEV_ROLE_ID`
- `SESSION_SECRET` — losowy string

### 3. Uruchomienie

```bash
docker compose up -d --build
```

Aplikacja domyślnie na porcie `3000`.

### 4. Discord OAuth2

W [Discord Developer Portal](https://discord.com/developers/applications):

1. Utwórz aplikację → OAuth2
2. Dodaj Redirect URI: `https://twoja-domena.com/auth/discord/callback`
3. Bot → włącz Server Members Intent
4. Dodaj bota na serwer z uprawnieniami do odczytu członków

## Konfiguracja `.env`

### Discord
| Zmienna | Opis |
|---------|------|
| `DISCORD_CLIENT_ID` | ID aplikacji Discord |
| `DISCORD_CLIENT_SECRET` | Secret aplikacji |
| `DISCORD_REDIRECT_URI` | URL callback (z `/auth/discord/callback`) |
| `DISCORD_BOT_TOKEN` | Token bota Discord |
| `DISCORD_GUILD_ID` | ID serwera Discord |
| `DISCORD_MEMBER_ROLE_ID` | ID roli dającej dostęp do platformy |
| `DISCORD_ADMIN_ROLE_ID` | ID roli admina/redaktora |
| `DISCORD_DEV_ROLE_ID` | ID roli developera |

### TeamSpeak 3
| Zmienna | Opis |
|---------|------|
| `TS_SERVER_HOST` | IP serwera TS |
| `TS_QUERY_PORT` | Port ServerQuery (domyślnie: 10011) |
| `TS_USERNAME` | Użytkownik query (domyślnie: serveradmin) |
| `TS_PASSWORD` | Hasło do ServerQuery |
| `TS_SERVER_ID` | ID wirtualnego serwera (domyślnie: 1) |
| `TS_MEMBER_GROUP_ID` | ID grupy dającej dostęp |
| `TS_ADMIN_GROUP_ID` | ID grupy admina |

## Baza danych (SQLite)

| Tabela | Opis |
|--------|------|
| `users` | Użytkownicy (Discord/TS/manual), role |
| `videos` | Filmy, źródła |
| `tags`, `video_tags` | System tagów |
| `sessions` | Sesje express-session |

## Technologie

- **Frontend**: React, Tailwind CSS, Vite, Lucide icons
- **Backend**: Express.js, better-sqlite3, express-session
- **Deploy**: Docker, Docker Compose, Cloudflare Tunnel

## Licencja

Projekt prywatny dla społeczności Alleria.pl.

---

© 2025 Alleria.pl | built by [Matthew](https://github.com/mrfroncu)