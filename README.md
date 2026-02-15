# 🏀 NBA Assistant z PydanticAI

Inteligentny agent konwersacyjny wykorzystujący **PydanticAI**, **HuggingFace** oraz rzeczywiste dane z **NBA API** do odpowiadania na pytania dotyczące ligi NBA. System umożliwia wyszukiwanie zawodników, statystyk, wyników meczów oraz tabel ligowych z pełną walidacją danych za pomocą modeli Pydantic.

## 📋 Spis treści

- [Funkcjonalności](#-funkcjonalności)
- [Technologie](#-technologie)
- [Wymagania](#-wymagania)
- [Instalacja](#-instalacja)
- [Konfiguracja](#-konfiguracja)
- [Uruchomienie](#-uruchomienie)
- [Dostępne zapytania](#-dostępne-zapytania)
- [Architektura](#-architektura)
- [Struktura projektu](#-struktura-projektu)
- [Przykłady użycia](#-przykłady-użycia)

## ✨ Funkcjonalności

- 🔍 **Wyszukiwanie zawodników** — pełne składy drużyn NBA z pozycjami i numerami
- 📊 **Statystyki zawodników** — punkty, asysty, zbiórki per mecz
- 🗓️ **Wyniki meczów** — wszystkie spotkania z wybranej daty
- 🏆 **Tabele konferencji** — aktualne rankingi East i West
- ✅ **Walidacja danych** — wykorzystanie modeli Pydantic do strukturyzacji odpowiedzi
- 💬 **Interfejs czatu** — przyjazny UI w Gradio z historią rozmów
- 🤖 **Inteligentny agent** — automatyczny wybór odpowiednich narzędzi na podstawie pytania

## 🛠️ Technologie

- **[PydanticAI](https://github.com/pydantic/pydantic-ai)** — framework do budowy agentów AI z walidacją typów
- **[HuggingFace](https://huggingface.co/)** — model językowy `moonshotai/Kimi-K2-Instruct`
- **[NBA API](https://github.com/swar/nba_api)** — oficjalne dane z NBA
- **[Gradio](https://gradio.app/)** — interfejs użytkownika
- **[Pydantic](https://docs.pydantic.dev/)** — walidacja i modelowanie danych
- **[PyTorch](https://pytorch.org/)** — backend dla modeli HuggingFace

## 📦 Wymagania

- Python 3.8+
- CUDA 11.8+ (opcjonalnie, dla GPU)
- Klucz API do Together AI


## ⚙️ Konfiguracja

### Klucz API

Edytuj plik notebooka i zastąp wartość `api_key` w konfiguracji modelu:

```python
model = HuggingFaceModel(
    'moonshotai/Kimi-K2-Instruct',
    provider=HuggingFaceProvider(
        provider_name='together', 
        api_key='TWOJ_KLUCZ_API'  # ← Wstaw tutaj swój klucz
    )
)
```

## ▶️ Uruchomienie

1. Otwórz plik `PydanticAI_8.ipynb` w Jupyter Notebook lub VS Code
2. Uruchom wszystkie komórki w kolejności
3. Po uruchomieniu ostatniej komórki pojawi się link do interfejsu Gradio
4. Otwórz link w przeglądarce i zacznij zadawać pytania!

## Wygląd
<img width="1063" height="603" alt="Zrzut ekranu 2026-02-15 o 11 44 53" src="https://github.com/user-attachments/assets/de1ded2c-4dd3-41e6-bb82-bd8f0aa34ccf" />
<img width="1063" height="559" alt="Zrzut ekranu 2026-02-15 o 11 45 10" src="https://github.com/user-attachments/assets/54b8146e-34fa-49a0-9680-579ef863b50a" />
<img width="1115" height="575" alt="Zrzut ekranu 2026-02-15 o 11 45 24" src="https://github.com/user-attachments/assets/39c1d5ca-2a37-4ca0-b4ee-004c7cc819f5" />
<img width="1095" height="623" alt="Zrzut ekranu 2026-02-15 o 11 45 34" src="https://github.com/user-attachments/assets/9403dded-50b3-4826-a9bb-d5049d79de05" />



## 🎯 Dostępne zapytania

### Przykłady:

```
✅ "Pokaż zawodników Los Angeles Lakers"
✅ "Mecze z dnia 2025-01-20"
✅ "Statystyki LeBron James"
✅ "Tabela konferencji East"
✅ "Zawodnicy Golden State Warriors"
✅ "Stats Stephen Curry"
✅ "Tabela West"
```

## 🏗️ Architektura

### Modele Pydantic

System wykorzystuje zestaw modeli do walidacji danych:

- **`PlayerInfo`** — informacje o zawodniku (ID, nazwa, drużyna, pozycja, numer)
- **`PlayerStats`** — statystyki zawodnika (PPG, APG, RPG, GP)
- **`MatchInfo`** — dane o meczu (drużyny, data, wynik, status)
- **`StandingInfo`** — pozycja w tabeli (wygrane, przegrane, procent)
- **`NBASearchResult`** — główny model odpowiedzi agenta
- **`NoResultsFound`** — obsługa braku wyników

### Narzędzia (Tools)

Agent dysponuje czterema narzędziami łączącymi się z NBA API:

| Narzędzie | Opis | Parametr |
|-----------|------|----------|
| `get_players_by_team()` | Pobiera skład drużyny | `team_name: str` |
| `get_matches_by_date()` | Zwraca mecze z danej daty | `date_str: str` (YYYY-MM-DD) |
| `get_player_stats()` | Statystyki zawodnika | `player_name: str` |
| `get_standings_by_conference()` | Tabela konferencji | `conference: str` ("East"/"West") |

### Przepływ danych

```
Użytkownik → Gradio UI → Agent PydanticAI → Model HF → Wybór narzędzia
                ↑                                              ↓
                └──────────── Format & Display ←───── NBA API
```

## 📁 Struktura projektu

```
PydanticAI_8.ipynb
├── Instalacja zależności
├── Konfiguracja modelu HuggingFace
├── Modele Pydantic (PlayerInfo, Stats, MatchInfo, etc.)
├── Funkcje pomocnicze (Tools) - NBA API
│   ├── get_players_by_team()
│   ├── get_matches_by_date()
│   ├── get_player_stats()
│   └── get_standings_by_conference()
├── Definicja agenta PydanticAI
├── Formatowanie wyników
└── Interfejs Gradio
```

## 💡 Przykłady użycia

### Zapytanie o zawodników

**Wejście:** `"Pokaż zawodników Los Angeles Lakers"`

**Wyjście:**
```
🏀 Los Angeles Lakers

#23 LeBron James — F
# 3 Anthony Davis — F-C
#15 Austin Reaves — G
...

📝 Podsumowanie: Znaleziono 15 zawodników drużyny Lakers.
```

### Zapytanie o statystyki

**Wejście:** `"Statystyki Stephen Curry"`

**Wyjście:**
```
📊 Stephen Curry

Sezon: 2024-25 | Drużyna: Golden State Warriors
GP: 45 | PPG: 24.8 | APG: 6.2 | RPG: 5.1
```

### Zapytanie o tabelę

**Wejście:** `"Tabela East"`

**Wyjście:**
```
🏆 Tabela East

 1. Cleveland Cavaliers      35W-6L (0.854)
 2. Boston Celtics          32W-10L (0.762)
 3. New York Knicks         28W-16L (0.636)
...
```

## 🔒 Zasady działania agenta

Agent NBA Assistant działa zgodnie z następującymi regułami:

1. ✅ **ZAWSZE używa narzędzi** — nie generuje danych z pamięci modelu
2. ✅ **Nie wymyśla informacji** — zwraca tylko dane z NBA API
3. ✅ **Obsługuje błędy** — jeśli brak wyników, zwraca `NoResultsFound` z sugestią
4. ✅ **Waliduje dane** — każda odpowiedź zgodna z modelem Pydantic
5. ✅ **Automatyczny wybór narzędzi** — na podstawie intencji użytkownika


## 👤 Autor

Franciszek Łasiński

---

**Uwaga:** Projekt wykorzystuje dane z oficjalnego NBA API. Szanuj limity zapytań i korzystaj z API zgodnie z ich polityką użytkowania.



## 🔗 Przydatne linki

- [PydanticAI Documentation](https://ai.pydantic.dev/)
- [NBA API GitHub](https://github.com/swar/nba_api)
- [Gradio Documentation](https://www.gradio.app/docs)
- [HuggingFace Models](https://huggingface.co/models)

---

⭐ **Jeśli projekt Ci się podoba, zostaw gwiazdkę na GitHubie!**
