# Warsztaty: BOLA / IDOR na platformie PortSwigger Web Security Academy

Celem warsztatów jest praktyczne poznanie podatności typu IDOR/BOLA oraz ogólnych błędów kontroli dostępu.  Laboratoria wykonujemy na platformie PortSwigger. Zanim można wykonywać laby trzeba tam założyć konto. Wszystkie zadania da się zrobić bez Burp Suite.

---

## Zadanie 1: User ID controlled by request parameter
Link do laboratorium:
https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter

Cel:
Pobrać dane innego użytkownika, manipulując parametrem `id` w adresie URL.

<details>
<summary> <b>Kliknij, aby rozwinąć wskazówki</b></summary>

1. Zaloguj się na konto testowe.
2. Otwórz zakładkę „My Account”.
3. W URL zobaczysz parametr `id=...`.
4. Podmień wartość `id` na `carlos`
5. Odczytaj dane innego użytkownika.

</details>
---

## Zadanie 2: Insecure Direct Object References (plik transkryptu)
Link do laboratorium:
https://portswigger.net/web-security/access-control/lab-insecure-direct-object-references

Cel:
Pobrać plik czatu innego użytkownika, manipulując nazwą pliku w URL.

<details>
<summary> <b>Kliknij, aby rozwinąć wskazówki</b></summary>
  
1. Wciśnij F12 i pobierz transkrypt. Zobacz jakie zapytanie pojawia się w zakładce Network. Z jakiego URL-a pobierany jest plik?
2. Podmień nazwę pliku na inną (np. 1.txt).

</details>
---

## Zadanie 3: Unprotected admin functionality
Link do laboratorium:
https://portswigger.net/web-security/access-control/lab-unprotected-admin-functionality

Cel:
Znalezienie ukrytego panelu administratora, który nie jest chroniony hasłem.

<details>
<summary> <b>Kliknij, aby rozwinąć wskazówki</b></summary>
  
1. Spróbuj dopisać w URL `/admin` lub `/administrator` – zobaczysz "Not Found".
2. Często ciekawe informacje można znaleźć w pliku robots.txt

</details>
---

## Zadanie 4: URL-based access control can be circumvented
Link do laboratorium:
https://portswigger.net/web-security/access-control/lab-url-based-access-control-can-be-circumvented

Cel:
Uzyskać dostęp do panelu administratora, manipulując nagłówkiem `X-Original-URL`.

<details>
<summary> <b>Kliknij, aby rozwinąć wskazówki</b></summary>
  
1. Wejdź na /admin — powinna być blokada.
2. Otwórz DevTools → Network.
3. Odśwież stronę, na samej górze panelu Network powinno się pokazać żądanie `GET /`
4. W Firefoxie możesz kliknąć na nie prawym przyciskiem myszy i wybrać opcję 'edit and resend'
5. Dodaj nagłówek `X-Original-URL` o wartości  `/admin`
6. Wyślij to spreparowane żądanie, w odpowiedzi na nie znajdziesz panel admina. Tam powinna być opcja usunięcia Carlosa
7. Na górze panelu 'edit and resend' dopisz `?username=carlos`. Zmień wartość nagłówka `X-Original-URL` z `/admin` na  `/admin/delete`

</details>
---

## Zadanie 5: User role can be modified in user profile (Mass Assignment)
Link do laboratorium:
https://portswigger.net/web-security/access-control/lab-user-role-can-be-modified-in-user-profile

Cel:
Zmienić swoją rolę na administratora poprzez manipulację danymi wysyłanymi w formularzu profilu.

<details>
<summary> <b>Kliknij, aby rozwinąć wskazówki</b></summary>

1. Otwórz formularz edycji profilu.
2. W DevTools → Network przechwyć żądanie `POST change-email`.
3. W tym zadaniu też możesz użyć opcji 'edit and resend'
4. W menu 'edit and resend' dodaj roleid:2 do jsona.
5. Prześlij żądanie, odśwież stronę, panel admina powinien być dostępny.
</details>
---

## Zadanie 6: User ID controlled by request parameter with password disclosure
Link do laboratorium:
https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter-with-password-disclosure

Cel:
Zdobyć hasło administratora, logując się na swoje konto i manipulując parametrem URL, a następnie usunąć użytkownika `carlos`.

<details>
<summary> <b>Kliknij, aby rozwinąć wskazówki</b></summary>

1. Zaloguj się danymi `wiener:peter` i wejdź w "My Account".
2. Zauważ, że w URL znajduje się parametr `id=wiener`.
3. Zmień wartość parametru `id` na `administrator`.
4. Strona załaduje się dla administratora. Hasło jest ukryte (zagwiazdkowane) w formularzu.
5. Aby je odczytać bez Burp Suite:
    * Kliknij prawym przyciskiem myszy na pole hasła i wybierz **"Zbadaj element" (Inspect)**.
    * W kodzie HTML poszukaj atrybutu `value` w tagu `<input>`, tam znajdziesz hasło jawnym tekstem.
6. Wyloguj się, zaloguj jako `administrator` (używając znalezionego hasła) i usuń Carlosa.

</details>

---

<details>
<summary> <b>Dla chętnych - Baza wiedzy IDOR / BOLA (Kliknij, aby rozwinąć)</b></summary>

Jeśli chcesz zgłębić temat Insecure Direct Object Reference (w nowoczesnych API nazywany często BOLA - Broken Object Level Authorization), polecamy poniższe zasoby:

1.  **OWASP** - Biblia bezpieczeństwa BOLA
    * [Czytaj więcej na OWASP](https://owasp.org/API-Security/editions/2023/en/0xa1-broken-object-level-authorization/)
2.  **HackTricks - IDOR** - Świetne baza wiedzy
    * [Link do HackTricks](https://book.hacktricks.xyz/pentesting-web/idor)
3.  **PayloadAllTheThings - IDOR** - Lista gotowych payloadów i technik do testowania
    * [GitHub Repository](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Insecure%20Direct%20Object%20References)
4.  **IDOR Writeups** - Często można znaleźć ciekawe opisy problemów z którymi ktoś się już zmagal
    * Wpisz w Google: `site:medium.com "IDOR" bug bounty writeup`

</details>
