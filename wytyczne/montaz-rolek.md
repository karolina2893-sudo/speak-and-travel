---
name: "montaz-rolek"
description: "Montuje rolki Karoliny (Angielski z Karoliną) z surowego nagrania z telefonu w jej aktualnym stylu „Styl 2” (jak rolka „Boisz się błędu”), bez naklejek i z nietkniętym dźwiękiem głosu. Użyj, gdy Karolina wrzuca nagranie i prosi o montaż, napisy albo „zrób rolkę”."
---

# Montaż rolek — „Angielski z Karoliną”

Karolina wrzuca surowe nagranie (pionowe, z telefonu, zwykle .mov), a dostaje gotową rolkę 1080×1920.

**Aktualny styl to „Styl 3” (jak @trenernauczycielionline).** 29.10.2026 Karolina wybrała go i poprosiła, żeby go zapisać i tak montować. Opis jest na dole pliku. „Styl 2” (opis niżej) jest zapasowy, a stary styl z 20.10 (naklejki-emoji, WIELKIE napisy z obrysem, zoom/błysk) jest NIEAKTUALNY. Innych stylów nie używaj, chyba że Karolina sama o nie poprosi.

## Zasady stałe (zawsze)
- **Dźwięk głosu zostaje bez zmian.** Żadnego odszumiania (afftdn), loudnorm, highpass, kompresji, EQ ani zmiany częstotliwości. Kiedyś przez to dźwięk się pogorszył i Karolina się skarżyła. Audio w plikach pośrednich trzymaj jako PCM (pcm_s24le w .mkv), a dopiero na końcu koduj do AAC 256 kb/s z oryginalną częstotliwością. Na koniec sprawdź, czy fragment wyjścia ma korelację ≈1,0 z oryginałem.
- **Cięcia:** wycinaj pauzy, powtórki, przewijanie promptera i „stop” (jej sygnał pomyłki: wytnij fragment przed nim i samo „stop”). Segmenty z przerwą < 0,6 s łącz w jeden klip, żeby mowa się nie dublowała. Przejścia i crossfade dawaj tylko na ciszy (pad ≤ połowa przerwy).
- **Jasno i ostro:** Karolina nie lubi ciemnych ani mglistych rolek. Grade „wariant B”: `eq=gamma=1.28:contrast=1.04,curves=master='0/0 0.25/0.33 0.6/0.7 1/1',colorchannelmixer=rr=0.98:bb=1.02` + odrobina kontrastu i lekki `unsharp`. Przy jaśniejszym nagraniu stosuj go łagodniej. Nie przesadzaj z ciepłem, bo zęby robią się żółte.
- **Napisy sprawdzaj ręcznie:** model myli polskie słowa i angielskie wstawki. Niepewne miejsca wypisz Karolinie przy wysyłce.
- Muzyki nie dodajemy (Karolina dodaje ją w Instagramie). Wysyłaj każdą rolkę od razu, jak jest gotowa. Pisz po polsku, krótko.
- Rolki do kalendarza adwentowego mają być wyraźnie świąteczne.

## Styl 2: jak wygląda
- **Kadr:** pełny, bez przybliżeń i bez naklejek. Karty i etykiety są nad głową, napisy na wysokości klatki piersiowej.
- **Napisy:** Inter Bold ok. 72 px, normalna pisownia (nie WIELKIE litery), białe z miękkim cieniem. Fraza ma 2–4 słowa. Słowa pojawiają się po kolei, w rytm mowy (fade + przejście z rozmycia do ostrości, ok. 150 ms). Blok frazy jest wyśrodkowany, ok. y 1200–1400.
- **Słowo kluczowe:** w osobnej linii pod frazą, Inter Black ok. 150 px, różowe (#F07AA0 / #E8638A). Wchodzi z rozmyciem i lekkim powiększeniem (np. „długopis,”, „gramatyki.”, „błędem,”, „dogadać.”).
- **Etykieta wątku (pigułka):** u góry na środku (y ok. 150–190), ciemne rozmyte szkło `rgba(26,12,20,0.7)`, zaokrąglona, z różową kropką. Tekst WIELKIMI literami z rozstrzeleniem, ok. 26 px (np. „A JAK POWIEM Z BŁĘDEM?”, „NIKT NIE SPRAWDZA CI GRAMATYKI”). Jedna na każdy wątek albo akapit, widoczna przez cały ten fragment.
- **Karteczka-lista** (gdy Karolina coś wymienia): w lewym górnym rogu nad głową, ciemne szkło, zaokrąglenie ok. 28 px. Ma mały nagłówek WIELKIMI literami z kropką (np. „W SZKOLE ZA BŁĄD”, „NAJWAŻNIEJSZE”). Punkty z różowym numerem w kółku dopisują się po kolei, dokładnie wtedy, gdy o nich mówi.
- **Karta przykładu** (zdanie po angielsku): u góry, biało-różowa (#FFF3F7) z cieniem. Ma czarny tag „PRZYKŁAD”, duże zdanie po angielsku (Inter Black ok. 46 px), pod nim wyjaśnienie ok. 28 px i różowy przycisk „Zrozumiane ✓ →”.
- **Koniec:** przycisk albo plakietka CTA (np. @angielskizkarolina, link albo „Napisz: WYZWANIE”).
- **Przejścia:** przy cięciu krótkie rozmycie z lekkim przyciemnieniem i cichy whoosh. Bez czarnych pasków na brzegach.
- **Dźwięki efektów:** ciche, dużo ciszej niż głos (whoosh na przejściach, delikatny „pop” przy karcie).

Styl 2 był pierwotnie renderowany w Remotion (kompozycja Style2, napisy słowo po słowie na podstawie czasów słów z rozpoznawania mowy). Ten kod nie przetrwał. Przy następnym montażu odtwórz wygląd według opisu powyżej (w Remotion albo przez Playwright/ASS) i zrób 4–6 klatek kontrolnych do porównania z wzorem. Jeśli Karolina ma pod ręką blad_ig.mp4, poproś o niego jako referencję.

## Kroki
1. **Narzędzia (raz na sesję):**
   ```bash
   pip install --break-system-packages -q sherpa-onnx soundfile playwright numpy
   mkdir -p /home/claude/asr && cd /home/claude/asr
   curl -sSL -O https://github.com/k2-fsa/sherpa-onnx/releases/download/asr-models/sherpa-onnx-whisper-small.tar.bz2
   curl -sSL -O https://github.com/k2-fsa/sherpa-onnx/releases/download/asr-models/silero_vad.onnx
   tar xjf sherpa-onnx-whisper-small.tar.bz2
   ```
   Modele z openaipublic i Hugging Face są blokowane, więc używaj GitHuba. Czcionkę Inter pobierz z `https://raw.githubusercontent.com/google/fonts/main/ofl/inter/`, jeśli jej nie ma w systemie.
2. **Sprawdź nagranie:** `ffprobe` (rozdzielczość, czas; przy HDR dodaj tonemapping), kilka klatek, średnia jasność.
3. **Transkrypcja:** `ffmpeg -i nagranie.mov -ac 1 -ar 16000 a.wav` → `python3 transkrypcja.py a.wav seg.json`. Fragmenty z angielskim przepuść też z `language='en'` i porównaj. Popraw tekst ręcznie, zachowaj czasy.
4. **Plan:** cięcia, frazy z czasami słów, słowa kluczowe, pigułki wątków, karteczki-listy (czas każdego punktu), karta przykładu, CTA.
5. **Montaż** według stylu i zasad dźwięku opisanych wyżej.
6. **Kontrola:** 4–6 klatek (napisy nie na twarzy, karty nie zasłaniają twarzy, jasność) i korelacja audio z oryginałem. Potem wyślij przez SendUserFile (display: render) z listą niepewnych słów.

## transkrypcja.py
```python
import sys
import sherpa_onnx, soundfile as sf, numpy as np, json
M='/home/claude/asr/sherpa-onnx-whisper-small/'
rec=sherpa_onnx.OfflineRecognizer.from_whisper(encoder=M+'small-encoder.int8.onnx',decoder=M+'small-decoder.int8.onnx',tokens=M+'small-tokens.txt',language='pl',task='transcribe',num_threads=4)
a,sr=sf.read(sys.argv[1],dtype='float32')
cfg=sherpa_onnx.VadModelConfig(); cfg.silero_vad.model='/home/claude/asr/silero_vad.onnx'; cfg.silero_vad.min_silence_duration=0.15; cfg.silero_vad.min_speech_duration=0.2; cfg.silero_vad.max_speech_duration=4.0; cfg.sample_rate=16000
vad=sherpa_onnx.VoiceActivityDetector(cfg,buffer_size_in_seconds=60)
out=[]
def drain():
    while not vad.empty():
        s=vad.front; seg=np.array(s.samples,dtype='float32'); st=s.start/sr
        r=rec.create_stream(); r.accept_waveform(sr,seg); rec.decode_stream(r)
        out.append({'start':round(st,2),'end':round(st+len(seg)/sr,2),'text':r.result.text.strip()}); vad.pop()
for i in range(0,len(a),512):
    vad.accept_waveform(a[i:i+512]); drain()
vad.flush(); drain()
json.dump(out,open(sys.argv[2],'w'),ensure_ascii=False,indent=0)
for o in out: print(o['start'],o['end'],o['text'])
```

## Cięcie z czystym dźwiękiem (do użycia w każdym montażu)
```python
# segs = [(start, end), ...] z transkrypcji; IN = nagranie
PAD, X, MERGE = 0.25, 0.20, 0.6
groups = []
for s, e in segs:
    if groups and s - groups[-1][1] < MERGE: groups[-1][1] = e
    else: groups.append([s, e])
clips = []
for i, (s, e) in enumerate(groups):
    a = s - PAD if i == 0 else s - min(PAD, (s - groups[i-1][1]) / 2 - 0.02)
    b = e + PAD if i == len(groups)-1 else e + min(PAD, (groups[i+1][0] - e) / 2 - 0.02)
    clips.append((max(0, a), b))
# każdy klip: ffmpeg -ss a -to b -i IN -vf "<grade>,fps=30,format=yuv420p" -c:v libx264 -crf 14 -c:a pcm_s24le cN.mkv
# sklejanie: przejście tylko między klipami (na ciszy), acrossfade=d=X:c1=qsin:c2=qsin, audio dalej pcm_s24le
# miks efektów: amix=inputs=2:normalize=0,alimiter=limit=0.98:level=false
# wyjście: -c:a aac -b:a 256k -ar <oryginalna częstotliwość>
```
## Styl 3: „jak trenernauczycielionline” (AKTUALNY od 29.10.2026)
Wzór: 6 zrzutów rolek @trenernauczycielionline. Rytm szybki: cięcie co 2–4 s, co kilka sekund nowa grafika.
- **Kadr:** pełny, siedzi na kanapie z mikrofonem. Grafiki obok głowy (lewa albo prawa górna ćwiartka), nigdy na twarzy.
- **Napisy:** 1–2 słowa naraz, białe, Poppins SemiBold ok. 80 px, normalna pisownia, miękki cień, na środku na wysokości klatki (y ok. 1240). Bez osobnego różowego słowa kluczowego.
- **Doodle-grafiki (główny wyróżnik):** proste ikony rysowane granatową kreską (#1B2559) z podpisem pod spodem, wchodzą z „rysowaniem” linii (stroke-dashoffset, ok. 400 ms). Np. zegar z kręcącymi się wskazówkami + „ile godzin?”, liczba w odręcznym kółku + „obserwujących”. Akcent kółka/podkreślenia: różowy Karoliny (#E8638A) zamiast zielonego.
- **Dymek-prompt / wiadomość:** jasne szkło (białe 85%, blur, zaokrąglenie ok. 40 px), mały szary nagłówek „WYSŁANE” z kropką, tekst granatowy ok. 40 px, granatowe kółko ze strzałką po prawej. Dobry do: „co mówisz w hotelu”, pytania kursantki, DM.
- **Karta pełnoekranowa (przerywnik/hook):** granatowe tło #0E1838, na środku ogromny biały tekst WIELKIMI literami (Poppins ExtraBold ok. 150 px, 2–3 linie), pod nim odręczne różowe podkreślenie, nad nim mały licznik czasu w stylu timecode (np. „00:00:31:00”). 1–1,5 s.
- **Słowo-echo:** kluczowe słowo powtórzone półprzezroczystą szarą czcionką nad głową (y ok. 300), gdy pada w mowie.
- **Tło-zrzut:** przy mówieniu o produkcie Karolina wycięta z tła (segmentacja), a za nią przyciemniony, rozmyty zrzut strony sprzedażowej z nagłówkiem i przyciskiem „Zapisz się →”.
- **CTA na końcu:** „Napisz START, a wyślę Ci…” (słowo do komentarza/DM).
- Zasady stałe (dźwięk bez zmian, jasny grade, ręczna kontrola napisów, bez muzyki) obowiązują jak zawsze.
