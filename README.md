# Warsztaty: BOLA / IDOR na platformie PortSwigger Web Security Academy

Celem warsztatów jest praktyczne poznanie podatności typu IDOR/BOLA oraz ogólnych błędów kontroli dostępu.  Laboratoria wykonujemy na platformie PortSwigger. Zanim można wykonywać laby trzeba tam założyć konto. Wszystkie zadania da się zrobić bez Burp Suite.

---

## Zadanie 1: User ID controlled by request parameter
Link do laboratorium:
https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter

Cel:
Pobrać dane innego użytkownika, manipulując parametrem `id` w adresie URL.

Wskazówki:
1. Zaloguj się na konto testowe.
2. Otwórz zakładkę „My Account”.
3. W URL zobaczysz parametr `id=...`.
4. Podmień wartość `id` na `carlos`
5. Odczytaj dane innego użytkownika.

---

## Zadanie 2: Insecure Direct Object References (plik transkryptu)
Link do laboratorium:
https://portswigger.net/web-security/access-control/lab-insecure-direct-object-references

Cel:
Pobrać plik czatu innego użytkownika, manipulując nazwą pliku w URL.

Wskazówki:
1. Wciśnij F12 i pobierz transkrypt. Zobacz jakie zapytanie pojawia się w zakładce Network. Z jakiego URL-a pobierany jest plik?
2. Podmień nazwę pliku na inną (np. 1.txt).

---

## Zadanie 3: Unprotected admin functionality
Link do laboratorium:
https://portswigger.net/web-security/access-control/lab-unprotected-admin-functionality

Cel:
Znalezienie ukrytego panelu administratora, który nie jest chroniony hasłem.

Wskazówki:
1. Spróbuj dopisać w URL `/admin` lub `/administrator` – zobaczysz "Not Found".
2. Często ciekawe informacje można znaleźć w pliku robots.txt


---

## Zadanie 4: URL-based access control can be circumvented
Link do laboratorium:
https://portswigger.net/web-security/access-control/lab-url-based-access-control-can-be-circumvented

Cel:
Uzyskać dostęp do panelu administratora, manipulując nagłówkiem `X-Original-URL`.

Wskazówki:
1. Wejdź na /admin — powinna być blokada.
2. Otwórz DevTools → Network.
3. Odśwież stronę, na samej górze panelu Network powinno się pokazać żądanie `GET /`
4. W Firefoxie możesz kliknąć na nie prawym przyciskiem myszy i wybrać opcję 'edit and resend'
5. Dodaj nagłówek `X-Original-URL` o wartości  `/admin`
6. Wyślij to spreparowane żądanie, w odpowiedzi na nie znajdziesz panel admina. Tam powinna być opcja usunięcia Carlosa
7. Na górze panelu 'edit and resend' dopisz `?username=carlos`. Zmień wartość nagłówka `X-Original-URL` z `/admin` na  `/admin/delete`

---

## Zadanie 5: User role can be modified in user profile (Mass Assignment)
Link do laboratorium:
https://portswigger.net/web-security/access-control/lab-user-role-can-be-modified-in-user-profile

Cel:
Zmienić swoją rolę na administratora poprzez manipulację danymi wysyłanymi w formularzu profilu.

Wskazówki:
1. Otwórz formularz edycji profilu.
2. W DevTools → Network przechwyć żądanie `POST change-email`.
3. W tym zadaniu też możesz użyć opcji 'edit and resend'
4. W menu 'edit and resend' dodaj roleid:2 do jsona.
5. Prześlij żądanie, odśwież stronę, panel admina powinien być dostępny.
