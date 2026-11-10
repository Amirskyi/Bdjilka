# Labirynt Świetlików

Statyczna gra po polsku, bez zależności i bez kroku kompilacji.

## Uruchomienie lokalne

Z katalogu projektu:

```sh
python3 -m http.server 8080 --directory dist
```

Otwórz http://localhost:8080/. Nie otwieraj pliku przez file://: bezwzględne ścieżki zasobów zakładają główny katalog serwera.

## Gotowe adresy

- `/` — Strona główna
- `/gra/` — Gra
- `/jak-grac/` — Jak grać
- `/faq/` — Najczęściej zadawane pytania
- `/polityka-prywatnosci/` — Polityka prywatności
- `/regulamin/` — Regulamin korzystania

Każdy adres ma własny index.html. Nie wymaga przekierowania SPA. Hostuj zawartość dist w katalogu głównym domeny, nie w podkatalogu.

## Mechanika

Losowy połączony labirynt DFS, trzy trudności, 5/10/18 iskier. Iskra daje 100 punktów. Bonus: max(0, 300/600/900 minus pełne sekundy). Rekord dla każdego poziomu zapisuje localStorage pod kluczem `labirynt-swietlikow.best.v1`. Stan próby nie jest zapisywany. Pauza nie liczy czasu. Ukrycie strony zatrzymuje grę. Brak reklam, analityki, zewnętrznych bibliotek i żądań przesyłających wynik.

## Publikacja na hostingu statycznym z HTTPS

1. Uzupełnij dane operatora i zweryfikuj dokumenty prawne według listy na stronie regulaminu. Ustal faktyczne logi dostawcy.
2. W Netlify wybierz „Add new project” i ręczne wdrożenie; prześlij katalog dist. Nie ustawiaj polecenia budowania. Wdrożenie otrzyma adres HTTPS.
3. W zarządzaniu domeną dodaj posiadaną domenę. Ustaw DNS dokładnie według rekordów pokazanych przez hosting; nie kopiuj adresu IP z innego projektu.
4. Zaczekaj na zatwierdzenie DNS i wystawienie certyfikatu. Włącz wymuszanie HTTPS, jeżeli dostawca udostępnia taką opcję. Wskaż jedną domenę główną i przekierowanie alternatywnej.
5. Sprawdź każdą ścieżkę bezpośrednio oraz po odświeżeniu. Dla każdego żądania HTTP oczekuj 301/308 do odpowiadającego HTTPS. Nie wystarczy przekierowanie JavaScript.
6. W przeglądarce sprawdź ważność certyfikatu dla domeny, kompletność łańcucha, brak ostrzeżeń i brak mixed content. Lokalne zasoby używają ścieżek względnych do domeny, bez adresów HTTP.
7. Sprawdź planszę, menu mobilne, pauzę, ukończenie, restart, odświeżenie i rekord na rzeczywistym telefonie oraz komputerze. Wyczyść localStorage i sprawdź usunięcie rekordu.

Alternatywnie użyj Nginx i dołączonego nginx.conf.example. Zastąp wszystkie NAZWA_DOMENY własną domeną. Skopiuj dist do /var/www/labirynt. Najpierw uruchom tylko blok HTTP z katalogiem ACME i zweryfikuj DNS. Wystaw certyfikat: `sudo certbot certonly --webroot -w /var/www/acme -d NAZWA_DOMENY`. Następnie aktywuj cały przykład, wykonaj `sudo nginx -t` oraz `sudo systemctl reload nginx`. Ustaw automatyczne odnawianie i sprawdź `sudo certbot renew --dry-run`.

Przykład weryfikacji po podłączeniu domeny:

```sh
curl -I http://NAZWA_DOMENY/gra/
curl -I https://NAZWA_DOMENY/gra/
```

Powtórz dla wszystkich sześciu tras. Pierwsze żądanie ma zwrócić przekierowanie do HTTPS; drugie 200. Nie używaj -k: ukrywa błędy certyfikatu.

## Regulamin i druk

Regulamin jest projektem zawierającym 50 rozdziałów. Arkusz druku A4 rozpoczyna każdy rozdział na nowej stronie i ukrywa nawigację. Przy standardowym wydruku daje to około 50 stron; ostateczna paginacja zależy od przeglądarki i skalowania. Wybierz A4, skalę 100%, wyłącz nagłówki i stopki drukarki. Objętość nie zastępuje weryfikacji prawnej. Dane właściciela, kontakt, licencja, logi i data obowiązywania nie zostały wymyślone.

## Ograniczenia

Canvas nie ma pełnego tekstowego odpowiednika dla osób niewidomych. Nie ma synchronizacji, zapisu przerwanej gry ani instalacji offline. Rzeczywiste testy przeglądarkowe, fizyczny telefon i paginacja PDF wymagają wykonania — narzędzie przeglądarki nie było dostępne w środowisku przygotowania.
