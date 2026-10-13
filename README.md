# Rifmoplet: a Rhyming Poetry Bot

A Telegram bot ([@Eng_Rifmoplet_bot](https://t.me/Eng_Rifmoplet_bot)) that writes short English poems with a given theme, meter, line length and rhyme word. Instead of generating text, it picks lines from a book or film script and combines them so that syllable counts and rhymes fit. Individual project, HSE University, 2023.

https://user-images.githubusercontent.com/90255314/227801810-6c228525-52a0-456f-a45d-0359c60198ca.MP4

## How it works

1. The user chooses a theme. Each theme is a source text: *Harry Potter and the Prisoner of Azkaban*, *The Godfather* or *Twilight*.
2. The user chooses a meter. The bot sends a picture explaining each one.
3. The user sends a seed word, says whether it should close line A or line B, and sets the minimum and maximum line length in syllables.
4. The bot returns a poem. Pressing "YES!" builds another one with the same settings.

Commands: `/start`, `/cancel`, `/help`.

## The phonetics behind it

All the linguistic work happens in `poem_creator.py`.

- Every word is looked up in the [CMU Pronouncing Dictionary](http://www.speech.cs.cmu.edu/cgi-bin/cmudict) (via NLTK). Vowel phonemes carry a stress mark from 0 to 2, so counting them gives the number of syllables.
- The rhyme of a line is the cluster "last vowel + following consonants" of its final word.

![CMUdict example](https://user-images.githubusercontent.com/90255314/227804831-87d32312-2a9b-4531-934c-0cbe9dea15f6.PNG)

## Preprocessing

`text_to_data.py` is run once before the bot. It splits each source text into sentences and stores every sentence with its syllable count and rhyme in an SQLite database (`hp.db`, `gf.db`, `tw.db`).

![Database](https://user-images.githubusercontent.com/90255314/227803446-566ec944-9c4b-458d-a26c-af49e2389061.PNG)

## Files

| File | What it does |
|---|---|
| `poem_creator.py` | `Line` (syllables, stress, rhyme) and `DataBase` classes, poem assembly |
| `text_to_data.py` | Converts source texts into SQLite databases |
| `Botik.py` | Async Telegram bot built with `python-telegram-bot` |
| `conf.py` | Bot configuration |
| `Files/` | Source texts |

## How to run

```bash
pip install python-telegram-bot nltk
python -c "import nltk; nltk.download('cmudict')"
python text_to_data.py   # only if you change the source texts
# put your bot token into conf.py (TOKEN), never commit it
python Botik.py
```
