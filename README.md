# Think of a number between 1 and 60

Wie die Zahl auf [dkrasniy.com](https://www.dkrasniy.com/) „erraten“ wird.

Es wird nichts gelesen: kein Mauszeiger, keine Verweildauer, keine Kamera. Sechs Ja/Nein-Fragen kodieren die Binärstellen der Zahl. Die angezeigte Zahl ist die Summe der gesetzten Zweierpotenzen.

## Ablauf

1. **Go** startet sechs feste Listen.
2. Jede Liste fragt: *Does your number appear in this list?*
3. **Yes** setzt das Bit dieser Liste auf `1`. **No** lässt es auf `0`.
4. Danach gilt:

```javascript
answers.forEach((yes, bit) => {
  if (yes === 1) total += 2 ** bit;
});
```

Die schwebenden Zahlen im Hintergrund sind Dekoration. Sie kommen aus `Math.floor(60 * Math.random()) + 1` und verschwinden beim Start. Sie gehen nicht in die Rechnung ein.

## Die sechs Listen

Eine Zahl steht auf Karte `i` genau dann, wenn Bit `i` in ihrer Binärdarstellung gesetzt ist.

| Frage | Wert | Zahlen auf der Karte |
| --- | ---: | --- |
| 1 | 1 | 1, 3, 5, 7, 9, … (alle ungeraden, bis 63) |
| 2 | 2 | 2, 3, 6, 7, 10, 11, 14, 15, … |
| 3 | 4 | 4–7, 12–15, 20–23, 28–31, … |
| 4 | 8 | 8–15, 24–31, 40–47, 56–63 |
| 5 | 16 | 16–31, 48–63 |
| 6 | 32 | 32–63 |

Jede ganze Zahl von 1 bis 63 hat genau eine solche Zerlegung. Die sechs Antworten bestimmen sie deshalb eindeutig.

Die Listen im Seitenbundle, in derselben Reihenfolge:

```javascript
const cards = [
  [1, 3, 5, 7, 9, 11, 13, 15, 17, 19, 21, 23, 25, 27, 29, 31, 33, 35, 37, 39, 41, 43, 45, 47, 49, 51, 53, 55, 57, 59, 61, 63],
  [2, 3, 6, 7, 10, 11, 14, 15, 18, 19, 22, 23, 26, 27, 30, 31, 34, 35, 38, 39, 42, 43, 46, 47, 50, 51, 54, 55, 58, 59, 62, 63],
  [4, 5, 6, 7, 12, 13, 14, 15, 20, 21, 22, 23, 28, 29, 30, 31, 36, 37, 38, 39, 44, 45, 46, 47, 52, 53, 54, 55, 60, 61, 62, 63],
  [8, 9, 10, 11, 12, 13, 14, 15, 24, 25, 26, 27, 28, 29, 30, 31, 40, 41, 42, 43, 44, 45, 46, 47, 56, 57, 58, 59, 60, 61, 62, 63],
  [16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, 62, 63],
  [32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, 62, 63],
];
```

## Beispiel: 21

`21` ist `16 + 4 + 1`, binär `010101`.

| Karte | Antwort | Beitrag |
| ---: | --- | ---: |
| 1 | Ja | +1 |
| 2 | Nein | 0 |
| 4 | Ja | +4 |
| 8 | Nein | 0 |
| 16 | Ja | +16 |
| 32 | Nein | 0 |

Summe: **21**.

## Randfälle

- Der Text sagt „1 bis 60“. Sechs Bits decken **1 bis 63** ab, deshalb stehen 61, 62 und 63 trotzdem auf den Karten. Für 1–60 stimmt das Ergebnis.
- Sechsmal **No** ergibt **0**. 0 war nie im angegebenen Bereich.
