# Tytuł projektu
Serwis konferencyjny "NauKon"

# Opis przeznaczenia aplikacji
Serwis ten pozwalałby na ułatwioną organizację ogólnopolskich konferencji naukowych oraz tworzyłby platformę, na której publikowane by były artykuły i prezentacje z różnych dziedzin nauki, co pozwalałoby na kreowanie nauki wśród młodych naukowców, dając im możliwość do przedstawienia wyników swoich badań naukowych.

Użytkownicy z pozycji gościa mieliby dostęp do:
 - przeglądania dostępnych konferencji,
 - sortowania i filtrowania konferencji wg nazwy, dziedziny nauki, miejscowości i miejsca odbycia, daty,
 - zarejestrowania się w systemie.

Zarejestrowany użytkownik ponadto miałby możliwość:
 - edycji szczegółów swojego konta,
 - możliwości rejestracji na konferencję,
 - składania opłat za udział w konferencji i/lub publikację artykułu,
 - umieszczania swoich prac na serwisie w postaci plików html, csv, txt, pdf.

Administratorzy dodatkowo mogliby:
 - organizować konferencje oraz dodawać ogłoszenia ich dotyczące,
 - przeglądać i moderować listę zarejestrowanych użytkowników,
 - przeglądać i moderować listy uczestników danych wydarzeń,
 - akceptować referaty złożone przez użytkowników.

## Wymagania systemowe
* wersja apache'a - Apache/2.4.41 (Ubuntu)
* wersja PHP'a - PHP version: 7.4.3
* wersja MySQL -  libmysql - mysqlnd 7.4.3

## Wymagania sprzętowe 
* dla serwera
    *środowisko korzystające z Linux, Apache, PHP i MySQL*
* dla klienta
    *urządzenie, które jest w stanie obsługiwać przeglądarkę internetową*

## Wymagania bezpieczeństwa
Niniejszy serwer niestety nie jest odporny na ataki SQL Injection - dla stabilności systemu zalecane jest niepróbowanie takich ataków.
Dostęp do zasobów osobom nieuprawnionym blokuje system sesyjności, który pozwala rozróżnić trzy stany użytkownika - gość, użytkownik i administrator. Dzięki temu gość nie ma dostępu do części dla zarejestrowanych użytkowników, a zwykły użytkownik nie może dokonywać zmian administracyjnych. Jednocześnie użytkownik nie wpisuje żadnych komend SQL-owych - dzięki czemu niwelowane jest ryzyko, że dokona krytycznych zmian w działaniu systemu; wpisuje jedynie dane w textboxy czy klika guziki, które mają już przygotowane kwerendy.

## Instalacja

Aby hostować aplikację, należy umieścić dane z folderu `IBD_zaliczenie` na serwerze Apache oraz zaimportować tabele `z_konferencja`, `z_ogloszenia`, `z_pliki`, `z_referat`, `z_uzytwkonicy` i `z_uczestnicy` do serwera baz danych. Aby móc zapisywać pliki, potrzebne jest nadanie adekwatnych uprawnień folderowi `IBD_zaliczenie/uploads` - z możliwością zapisu dla wszystkich grup tj. np. `drwxrwxrwx`.

Niemniej, możliwy jest dostęp bezpośrednio działającej już aplikacji, pod adresem: 
https://www.manticore.uni.lodz.pl/~agorecki/IBD_zaliczenie/index.php

## Uwierzytelnienie w aplikacji

Do momentu logowania ma się uprawnienia gościa. Uwierzytelnianie zapewniane jest przez panel logowania, gdzie udany proces logowania przenosi do panelu użytkownika/administratora - adekwatnie do roli użytkownika przechowywanej w sesji. 

* Dane dla przykładowego usera:
    - login: user
    - hasło: user

* Dane dla przykładowego admina:
    - login: admin
    - hasło: admin

## Opis użytkowników

System, jak zostało wyżej zaznaczone, posiada trzy rodzaje użytkowników: 
    Gość - czyli stan, w którym nie jest się zalogowanym, 
    Użytkownik - stan, w którym jest się zalogowanym na konto z uprawnieniami użytkownika; jednocześnie nadal posiadając możliwości Gościa
    Administrator - stan, w którym jest się zalogowanym na konto z uprawnieniami administratora; jednocześnie nadal posiadając możliwości Użytkownika i Gościa.

## Zakres możliwości

Jak wyżej wynika, ograniczeniem zakresu możliwości jest rola aktualnie zalogowanego użytkownika (lub jej brak w przypadku niezalogowania).

* Gość może:
    - przeglądać konferencje,
        - sortować (wg wszystkich podanych kolumn) i filtrować (wg nazwy) konferencje,
    - przeglądać ogłoszenia dotyczące konferencji,
    - utworzyć konto.

* Użytkownik może ponadto:
    - edytować szczegóły swojego konta (imię, nazwisko, reprezentowana uczelnia czy e-mail kontaktowy),
    - może rejestrować się na konferencje,
        - może opłacać wstęp na konferencje,
    - może publikować artykuły naukowe dotyczące konferencji,
        - może opłacać opłaty za publikację artykułu,
        - może umieszczać załączniki dotyczące artykułów (wyłącznie w formacie html, csv, txt i pdf).

* Administrator może ponadto:
    - dodawać nowe konferencje,
    - dodawać ogłoszenia dotyczące konferencji,
    - przeglądać listę zarejestrowanych użytkowników,
        - moderować zarejestrowanych użytkowników,
    - przeglądać listę uczestników wydarzeń,
        - moderować listy uczestników wydarzeń, zmieniać status opłat dla użytkownika (docelowo w przypadku np. przelewu klasycznego zamiast natychmiastowego przez jakiegoś operatora),
    - akceptować referaty złożone przez użytkowników, decydując o tym, czy mogą się pojawić na konferencji.


## Schemat bazy danych
Tabela `z_konferencja`
Table structure for table z_konferencja
Column	        Type	        Null	Default	Comments
id_konferencja	int(11)	        No		
nazwa	        varchar(255)	No		
dziedzina	    varchar(255)	No		
miasto	        varchar(255)	No		
miejsce	        varchar(255)	No		
data	        datetime	    No		
cena	        int(11)	        No		

Tabela `z_ogloszenia`
Table structure for table z_ogloszenia
Column	        Type	        Null	Default
id_ogloszenie	int(11)	        No	
id_konferencja	int(11)	        No	
tekst	        varchar(1000)	No	
id_uzytkownik	int(11)	        No	
data_dodania	date	        No	    current_timestamp()
czy_aktywne	    enum('0', '1')	No	

Tabela `z_pliki`

Table structure for table z_pliki
Column	        Type	        Null	Default
id_plik	        int(11)	        No	
nazwa	        varchar(255)	No	
id_referatu	    int(11)	        No	

Tabela `z_referat`

Column	                Type	        Null    Default
id_referat	            int(11)         No	
id_uzytkownik	        int(11)         No	
id_konferencja	        int(11)         No	
tytul	                varchar(255)	No	
opis	                varchar(1000)	Yes	    NULL
data_dodania	        date	        No	    current_timestamp()
czy_oplacony	        enum('0', '1')	No	    0
czy_zaakceptowany	    enum('0', '1')	No	    0

Tabela `z_uczestnicy`

Table structure for table z_uczestnicy
Column	        Type	        Null	Default
id_uczestnik	int(11)	        No	
id_konferencja	int(11)	        No	
id_uzytkownik	int(11)	        No	
czy_oplacone	enum('0', '1')	No	    0
czy_aktywne	    enum('0', '1')	No	    1

Tabela `z_uzytkownicy`

Table structure for table z_uzytkownicy
Column	        Type	                Null	Default
id_uzytkownik	int(11)	                No	
login	        varchar(100)	        No	
haslo	        varchar(255)	        No	
imie	        varchar(100)	        No	
nazwisko	    varchar(100)	        No	
email	        varchar(100)	        No	
uczelnia	    varchar(255)	        No	
czy_aktywny	    enum('0', '1')	        No	    1
rola	        enum('user', 'admin')	No	    user

## Schemat stron

Strony dotyczące części dla Gościa są w katalogu głównym - `IBD_zaliczenie`, natomiast część dotycząca zalogowanych użytkowników/administratorów i ich czynności są w podfolderze `IBD_zaliczenie/cms/`.


## Opis interfejsu

Część dla *guesta* to część zawierająca stronę główną (w której wyświetlane są konferencje), stronę z ogłoszeniami oraz stronę o projekcie. Dostępne one są zawsze z paska nawigacyjnego, znajdującego się na górze strony. Ponadto jest tam też przycisk pozwalający się na zalogowanie (dla gości) czy przejście do portalu użytkownika (dla zalogowanych).

Strony *logowania i rejestrowania* to formularz pozwalający wpisać swoje dane.

Panel dla *użytkowników* posiada z lewej strony zwijany panel z najważniejszymi funkcjami, a nagłówek daje dostęp do profilu użytkownika czy możliwości wylogowania.

## Autor

* **Artur Górecki** 
* *nr albumu: 389143*
* *login z manticore'y: agorecki*

## Wykorzystane zewnętrzne biblioteki

* bootstrap w wersji 5.1.3
* fonts.googleapis.com - wykorzystanie fontów Lora, Open Sans, Nunito
* jquery
