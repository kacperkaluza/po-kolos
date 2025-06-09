# Kolokwium z Programowania Obiektowego w C++ – Kompendium

Opracowanie: najważniejsze zagadnienia, odpowiedzi i przykłady.

---

## 1. Różnica między programowaniem strukturalnym a obiektowym

- **Strukturalne:** Program dzieli się na funkcje/procedury, skupia się na operacjach na danych. Dane i funkcje są rozdzielone.
- **Obiektowe:** Program dzieli się na obiekty (instancje klas), które łączą dane i metody. Kluczowe są: dziedziczenie, polimorfizm, enkapsulacja, abstrakcja.

---

## 2. Cechy programowania obiektowego

- Abstrakcja
- Enkapsulacja (hermetyzacja)
- Dziedziczenie
- Polimorfizm
- Modularność
- Reużywalność kodu
- Obiekty i klasy

---

## 3. Całkowite typy liczbowe w C/C++. Przykłady zastosowania

- `char`, `unsigned char` – znak, bajt danych (np. bufory).
- `short`, `unsigned short` – licznik, małe liczby, np. wiek.
- `int`, `unsigned int` – licznik pętli, indeksy, suma.
- `long`, `unsigned long` – większe liczby, np. duża populacja, adresy pamięci.
- `long long`, `unsigned long long` – bardzo duże liczby, np. identyfikatory, operacje na dużych plikach.

**Przykład:**
```cpp
int liczba_studentow = 100;
unsigned char bajt_danych = 255;
long long duza_liczba = 12345678901234LL;
```

---

## 4. Zmiennoprzecinkowe typy liczbowe w C/C++. Przykłady zastosowania

- `float` – grafika, gry, współrzędne, proste pomiary.
- `double` – obliczenia naukowe, większa precyzja, symulacje.
- `long double` – bardzo precyzyjne obliczenia naukowe.

**Przykład:**
```cpp
float temperatura = 36.6f;
double pi = 3.141592653589793;
long double bardzo_precyzyjny = 1.0L / 3.0L;
```

---

## 5. Priorytety operatorów w C/C++. Wpływ na wykonanie programu. Przykład

Priorytet operatorów określa kolejność wykonywania działań w wyrażeniu.

**Przykład:**
```cpp
int a = 2, b = 3, c = 4;
int wynik = a + b * c; // wynik = 2 + (3 * 4) = 14, bo * ma wyższy priorytet niż +
```
Użycie nawiasów zmienia kolejność:
```cpp
int wynik = (a + b) * c; // wynik = (2 + 3) * 4 = 20
```

---

## 6. Zmienne wskaźnikowe w C/C++. Przykład

Służą do przechowywania adresów innych zmiennych, dynamicznej alokacji pamięci, pracy z tablicami i strukturami.

**Przykład:**
```cpp
int a = 10;
int *wsk = &a;
*wsk = 20; // zmienia wartość a na 20
```

---

## 7. Różnice: referencje vs wskaźniki. Przykład referencji

- **Wskaźnik:** przechowuje adres, może być NULL, można zmieniać na co wskazuje.
- **Referencja:** alias do zmiennej, musi być zainicjowana, nie może być NULL, nie można zmienić powiązania.

**Przykład:**
```cpp
void zwieksz(int &x) { x++; }

int a = 5;
zwieksz(a); // a == 6
```

---

## 8. Kolejność argumentów domyślnych w C++

- Argumenty domyślne muszą być na końcu listy parametrów.
- W wywołaniu można pomijać tylko te od końca.

**Przykład:**
```cpp
void test(int a, int b = 2, int c = 3);
test(1);       // b=2, c=3
test(1, 4);    // b=4, c=3
```

---

## 9. Przeciążanie funkcji/metod w C++. Przykład

Służy do tworzenia kilku wersji funkcji/metody o tej samej nazwie, ale innych parametrach.

**Przykład:**
```cpp
int suma(int a, int b) { return a + b; }
double suma(double a, double b) { return a + b; }
suma(1, 2);     // int
suma(2.5, 3.1); // double
```

---

## 10. Czym jest konstruktor klasy w języku C++? Kiedy jest wywoływany? Przykład

**Konstruktor** to specjalna metoda klasy, która jest wywoływana automatycznie podczas tworzenia nowego obiektu tej klasy. Służy do inicjalizacji pól i wykonania kodu startowego.

**Przykład:**
```cpp
class Osoba {
public:
    std::string imie;
    Osoba(const std::string& imie_) : imie(imie_) {}
};

Osoba o1("Adam"); // konstruktor wywołany automatycznie
```

---

## 11. Czy konstruktor może wywoływać inne metody klasy i funkcje globalne? Przykład

**Tak**, konstruktor może wywoływać metody klasy oraz funkcje globalne.

**Przykład:**
```cpp
void przywitaj() { std::cout << "Witaj!" << std::endl; }

class Przyklad {
public:
    Przyklad() {
        metoda();
        przywitaj();
    }
    void metoda() { std::cout << "W konstruktorze." << std::endl; }
};
```

---

## 12. Do czego służy konstruktor kopiujący? Kiedy domyślne działanie może powodować błędy?

**Konstruktor kopiujący** służy do tworzenia nowego obiektu jako kopii innego obiektu tej samej klasy.

**Domyślne działanie** (płytka kopia) może być błędne, gdy klasa zarządza zasobami dynamicznymi (np. wskaźniki, dynamiczna pamięć) – oba obiekty mogą wskazywać na ten sam obszar pamięci, co prowadzi do podwójnego zwolnienia pamięci lub innych błędów ("double free").

**Przykład:**
```cpp
class Bufor {
public:
    int* dane;
    Bufor() { dane = new int[100]; }
    ~Bufor() { delete[] dane; }
    // Bez własnego konstruktora kopiującego kopiowane są tylko wskaźniki!
};
```

---

## 13. Czy konstruktor może być przeciążony? Uzasadnij

**Tak, konstruktor może być przeciążony** – można tworzyć wiele konstruktorów z różnymi listami parametrów.

**Przykład:**
```cpp
class Punkt {
public:
    Punkt() { x = y = 0; }
    Punkt(int xx, int yy) { x = xx; y = yy; }
private:
    int x, y;
};
```

---

## 14. Czym jest destruktor klasy w C++ i czy może być przeciążony?

**Destruktor** to specjalna metoda wywoływana automatycznie przy niszczeniu obiektu, służy do zwalniania zasobów.

- Deklaracja: `~NazwaKlasy();`
- **Nie może być przeciążony** – w każdej klasie może być tylko jeden destruktor.

---

## 15. Ogólny schemat tworzenia i korzystania z przestrzeni nazw (namespaces)

```cpp
namespace MojaPrzestrzen {
    void funkcja() { /* ... */ }
    class Klasa { /* ... */ };
}

// użycie
MojaPrzestrzen::funkcja();
MojaPrzestrzen::Klasa obiekt;
```

Można też użyć `using namespace MojaPrzestrzen;` (niezalecane w dużych projektach).

---

## 16. Notacja węgierska – wyjaśnienie i przykłady prefiksów

Notacja węgierska polega na dodawaniu prefiksów do nazw zmiennych, wskazujących na ich typ lub przeznaczenie.

**Przykłady:**
- `iCount` – int (licznik)
- `pszName` – pointer to zero-terminated string (wskaźnik na napis)
- `bReady` – bool (flaga)

Obecnie odchodzi się od tej notacji na rzecz czytelnych nazw i typów.

---

## 17. Czym różni się klasa od obiektu w języku C++? Przykład

- **Klasa** to szablon/opis (definicja) – określa jakie dane i metody będą miały obiekty.
- **Obiekt** to konkretny egzemplarz klasy w pamięci programu.

**Przykład:**
```cpp
class Samochod {
public:
    std::string marka;
};

Samochod auto1; // obiekt klasy Samochod
```

---

## 18. Hermetyzacja (enkapsulacja) w C++. Przykład

To ukrywanie szczegółów działania klasy (pól, metod prywatnych) i udostępnianie tylko wybranych elementów poprzez interfejs publiczny.

**Przykład:**
```cpp
class KontoBankowe {
private:
    double saldo;
public:
    void wplata(double kwota) { saldo += kwota; }
    double pobierzSaldo() const { return saldo; }
};
```

---

## 19. Różnica: `new` vs `malloc` (C++ vs C)

- `new` alokuje pamięć i wywołuje konstruktor, zwraca wskaźnik o odpowiednim typie.
- `malloc` tylko alokuje pamięć (nie wywołuje konstruktora), zwraca `void*`.

**Przykład:**
```cpp
int* a = new int(5);      // C++
int* b = (int*)malloc(sizeof(int)); // C
```

---

## 20. Dziedziczenie – co dostaje klasa potomna?

Klasa potomna dziedziczy:
- wszystkie pola i metody publiczne i chronione (protected),
- nie dziedziczy pól/metod prywatnych.

---

## 21. Dziedziczenie wielorakie/wielobazowe. Przykład

To dziedziczenie po więcej niż jednej klasie bazowej.

**Przykład:**
```cpp
class A {};
class B {};
class C : public A, public B {};
```

---

## 22. Zastępowanie metod klasy bazowej w potomnej (override). Jak nazywa się ten mechanizm?

To **nadpisywanie** (override). Służy do polimorfizmu.

**Przykład:**
```cpp
class Baza {
public:
    virtual void wypisz() { std::cout << "Baza"; }
};

class Potomna : public Baza {
public:
    void wypisz() override { std::cout << "Potomna"; }
};
```

---

## 23. Polimorfizm – co to jest, do czego służy? Przykład

Pozwala wywoływać metody klasy potomnej przez wskaźnik/referencję do klasy bazowej.

**Przykład:**  
Można przechowywać różne typy obiektów w jednej tablicy wskaźników do klasy bazowej i wywoływać odpowiednie wersje metod.

---

## 24. Metody wirtualne vs zwykłe. Deklaracja

- **Wirtualne:** deklaruje się je przez `virtual` w klasie bazowej, pozwalają na polimorfizm.
- **Zwykłe:** nie można ich nadpisywać dynamicznie przez wskaźnik do bazy.

**Przykład:**
```cpp
class B {
public:
    virtual void foo();
};
```

---

## 25. Klasa abstrakcyjna – czy można utworzyć obiekt? Do czego służy?

Nie można utworzyć obiektu klasy abstrakcyjnej (zawiera co najmniej jedną metodę czysto wirtualną). Służy do definiowania wspólnego interfejsu.

---

## 26. Czy klasa w C++ może zawierać inną klasę? Uzasadnij

Tak, można deklarować klasy zagnieżdżone (nested class) – często dla organizacji kodu, kapsułkowania pomocniczych struktur.

---

## 27. Pole statyczne vs stała (`const`). Przykład

- **Statyczne pole:** jedno dla wszystkich obiektów klasy, deklarowane przez `static`.
- **Stała:** niezmienna wartość (w obrębie obiektu lub klasy).

**Przykład:**
```cpp
class Przyklad {
public:
    static int licznik;
    const int stala = 5;
};
```

---

## 28. Metody statyczne – definicja i użycie bez obiektu. Przykład

Metoda statyczna (`static`) nie ma dostępu do pól instancji, można wywołać ją bez tworzenia obiektu.

**Przykład:**
```cpp
class Kalkulator {
public:
    static int dodaj(int a, int b) { return a + b; }
};

int suma = Kalkulator::dodaj(2, 3);
```

---

## 29. Poznane kontenery STL – wymień i opisz krótko zastosowanie

- **std::vector** – dynamiczna tablica, szybki dostęp przez indeks, rośnie automatycznie; często używany do przechowywania sekwencji o zmiennym rozmiarze.
- **std::list** – lista dwukierunkowa, szybkie wstawianie/usuwanie na początku/końcu/środku, wolniejszy dostęp losowy.
- **std::deque** – dwustronna kolejka, szybkie dodawanie/usuwanie na obu końcach.
- **std::set** – zbiór unikatowych, uporządkowanych elementów, szybkie wyszukiwanie.
- **std::map** – mapa asocjacyjna (klucz-wartość), szybkie wyszukiwanie po kluczu, klucze unikalne i uporządkowane.
- **std::unordered_map** – mapa asocjacyjna bez porządku, oparta na hash, bardzo szybkie wyszukiwanie (średnio).
- **std::queue** – kolejka FIFO.
- **std::stack** – stos LIFO.
- **std::priority_queue** – kolejka priorytetowa, elementy wyciągane wg priorytetu.

---

## 30. Iterator w C++ – co to jest, gdzie się używa?

**Iterator** to obiekt pośredniczący w dostępie do elementów kolekcji (np. vector, list, set) w sposób podobny do wskaźnika. Używany do przechodzenia przez kontenery STL.

**Przykład użycia:**
```cpp
std::vector<int> v = {1, 2, 3};
for (auto it = v.begin(); it != v.end(); ++it) {
    std::cout << *it << " ";
}
```

---

## 31. std::vector – jaką strukturę danych reprezentuje? Przykład zastosowania

**std::vector** to dynamicznie alokowana, jednowymiarowa tablica.

**Przykład zastosowania:** przechowywanie wyników pomiarów, listy studentów, dynamiczna tablica liczb.

**Przykładowy program:**
```cpp
std::vector<int> liczby;
liczby.push_back(10);
liczby.push_back(20);
for (int x : liczby) std::cout << x << " "; // wypisze: 10 20
```

---

## 32. std::list – jaka struktura? Przykład

**std::list** to lista dwukierunkowa (doubly linked list).

**Przykład zastosowania:** kolejka zadań do realizacji, bufor historii operacji.

**Przykładowy program:**
```cpp
std::list<std::string> kolejka;
kolejka.push_back("zadanie1");
kolejka.push_front("zadanie0");
for (auto& z : kolejka) std::cout << z << " ";
```

---

## 33. std::set – jaka struktura? Przykład

**std::set** to uporządkowany zbiór unikalnych elementów (zwykle drzewo BST).

**Przykład zastosowania:** zbiór ID, unikalne słowa w tekście.

**Przykładowy program:**
```cpp
std::set<int> liczby = {3,1,2,3};
for (int x : liczby) std::cout << x << " "; // wypisze: 1 2 3
```

---

## 34. Kontener mapy asocjacyjnej + przykład

**std::map** – przechowuje pary klucz-wartość, klucze unikalne, szybkie wyszukiwanie.

**Przykład:**
```cpp
std::map<std::string, int> wiek;
wiek["Jan"] = 20;
wiek["Anna"] = 22;
for (auto& para : wiek) {
    std::cout << para.first << ": " << para.second << std::endl;
}
```

---

## 35. Przykład programu z kontenerem i iteratorem (odwróconym też)

```cpp
#include <vector>
#include <iostream>
int main() {
    std::vector<int> liczby = {1, 2, 3, 4};
    // Zwykły iterator
    for (auto it = liczby.begin(); it != liczby.end(); ++it)
        std::cout << *it << " "; // 1 2 3 4
    // Odwrócony iterator
    for (auto rit = liczby.rbegin(); rit != liczby.rend(); ++rit)
        std::cout << *rit << " "; // 4 3 2 1
    return 0;
}
```

---

## 36. Po co stosuje się szablony klas (templates)? Przykład i definicja

Pozwalają pisać klasy (i funkcje) działające na różnych typach danych, bez powielania kodu.

**Przykład słownie:** szablon klasy stosu, który działa na `int`, `double`, `std::string` itp.

**Definicja:**
```cpp
template <typename T>
class Stos {
    std::vector<T> dane;
public:
    void push(const T& elem) { dane.push_back(elem); }
    void pop() { dane.pop_back(); }
    T top() const { return dane.back(); }
};
```

---

## 37. Po co stosuje się szablony funkcji? Przykład i definicja

Pozwalają pisać jedną funkcję działającą dla różnych typów.

**Przykład słownie:** funkcja zamieniająca miejscami dwie zmienne dowolnego typu.

**Definicja:**
```cpp
template <typename T>
void swap(T& a, T& b) {
    T temp = a;
    a = b;
    b = temp;
}
```

---

## 38. Szablony funkcji i szablony klas – przykłady programów

**Szablon funkcji:**
```cpp
template <typename T>
T maksimum(T a, T b) {
    return (a > b) ? a : b;
}

int main() {
    std::cout << maksimum(5, 10) << std::endl;      // int
    std::cout << maksimum(3.5, 2.1) << std::endl;   // double
}
```

**Szablon klasy:**
```cpp
template <typename T>
class Para {
public:
    T pierwszy, drugi;
    Para(T a, T b) : pierwszy(a), drugi(b) {}
    T suma() { return pierwszy + drugi; }
};

int main() {
    Para<int> p1(2, 3);
    std::cout << p1.suma() << std::endl; // 5

    Para<double> p2(2.5, 3.5);
    std::cout << p2.suma() << std::endl; // 6.0
}
```

---

## 39. Przeciążanie operatorów klas w C++. Zastosowanie

**Przeciążanie operatorów** umożliwia definiowanie własnego zachowania operatorów (np. +, -, ==) dla obiektów własnych klas.

**Zastosowania:**
- Intuicyjna obsługa operacji na własnych typach (np. dodawanie wektorów, porównanie dat).
- Umożliwia użycie operatorów na obiektach jak na typach wbudowanych.

**Przykład:**
```cpp
class Punkt {
public:
    int x, y;
    Punkt(int a, int b) : x(a), y(b) {}
    Punkt operator+(const Punkt& p) const {
        return Punkt(x + p.x, y + p.y);
    }
};

Punkt a(1,2), b(3,4);
Punkt c = a + b; // operator+ przeciążony
```

---

## 40. Funkcje zaprzyjaźnione (friend) w C++. Różnice od zwykłych funkcji

- **friend** – funkcja zaprzyjaźniona ma dostęp do prywatnych i chronionych elementów klasy, ale nie jest jej członkiem.
- **Zwykła funkcja** – nie ma dostępu do prywatnych/składowych klasy.

**Przykład:**
```cpp
class Sekret {
    int wartosc;
public:
    Sekret(int x) : wartosc(x) {}
    friend void pokaz(const Sekret& s);
};

void pokaz(const Sekret& s) {
    std::cout << s.wartosc << std::endl; // dostęp do prywatnego pola
}
```

---

## 41. Czy można przeciążać operatory w klasach abstrakcyjnych? Uzasadnij

**Tak, można.** Klasa abstrakcyjna może mieć przeciążone operatory, ale nie można utworzyć jej obiektu – korzysta się z tego przy polimorfizmie i dziedziczeniu.

---

## 42. Czy hermetyzacja dotyczy szablonów klas? Uzasadnij

**Tak.** Szablony klas mogą zawierać prywatne/protected/public elementy, więc hermetyzacja działa tak samo jak w zwykłych klasach.

---

## 43. Znajomość biblioteki wxWidgets (diagram, zastosowanie)

**wxWidgets** to biblioteka C++ do tworzenia aplikacji GUI wieloplatformowych.  
**Podstawowe klasy:**
- `wxApp` – główna aplikacja
- `wxFrame` – główne okno
- `wxPanel`, `wxDialog` – panele i okna dialogowe
- `wxButton`, `wxTextCtrl`, `wxStaticText` – kontrolki

---

## 44. Analiza wybranych klas, np. wxApp – publiczne pola i metody

- `wxApp` – klasa bazowa dla aplikacji, najważniejsze metody to:  
  - `OnInit()` – inicjalizacja aplikacji  
  - `OnExit()` – czyszczenie po zamknięciu  
  - pole: wskaźnik do głównego okna aplikacji

---

## 45. Struktura programu, szablon (szkielet) z wxWidgets

```cpp
#include <wx/wx.h>

class MyApp : public wxApp {
public:
    virtual bool OnInit();
};

class MyFrame : public wxFrame {
public:
    MyFrame(const wxString& title);
};

wxIMPLEMENT_APP(MyApp);

bool MyApp::OnInit() {
    MyFrame *frame = new MyFrame("Tytuł okna");
    frame->Show(true);
    return true;
}

MyFrame::MyFrame(const wxString& title) : wxFrame(NULL, wxID_ANY, title) {
    // tu kontrolki
}
```

---

## 46. Okno główne aplikacji – przykłady. Budowa okna z ramką, okna dialogowego itp.

**Okno główne z ramką (`wxFrame`):**
```cpp
class MyFrame : public wxFrame {
public:
    MyFrame() : wxFrame(NULL, wxID_ANY, "Moje okno główne") {
        // tu dodajemy kontrolki, menu itp.
    }
};
```

**Okno dialogowe (`wxDialog`):**
```cpp
class MyDialog : public wxDialog {
public:
    MyDialog(wxWindow* parent) : wxDialog(parent, wxID_ANY, "Dialog przykładowy") {
        // tu kontrolki dialogu
    }
};
```
**Tworzenie i wyświetlanie dialogu:**
```cpp
MyDialog dlg(this);
dlg.ShowModal(); // modalny dialog
```

---

## 47. Rodzaje okien dialogowych w wxWidgets

- **Modalne** – blokują interakcję z innymi oknami do czasu zamknięcia (`ShowModal()`).
- **Niemodalne** – można pracować z innymi oknami równolegle (`Show()`).

---

## 48. Klasa wxString – przykłady zastosowań

`wxString` to podstawowy typ tekstowy w wxWidgets, obsługuje Unicode i łatwo konwertuje się z/do std::string.

**Przykład:**
```cpp
wxString tekst = "Cześć, świecie!";
tekst += " Dodaję tekst.";
if (tekst.Contains("Dodaję")) {
    wxMessageBox(tekst);
}
```

---

## 49. Mapa komunikatów (event map) – wyjaśnienie i przykład

**Mapa komunikatów** to mechanizm przypisywania obsługi zdarzeń (np. kliknięcia przycisku) do odpowiednich metod.

**Przykład:**
```cpp
class MyFrame : public wxFrame {
public:
    MyFrame() {
        wxButton* btn = new wxButton(this, wxID_ANY, "Kliknij mnie");
        Bind(wxEVT_BUTTON, &MyFrame::OnButtonClick, this);
    }
    void OnButtonClick(wxCommandEvent& event) {
        wxMessageBox("Kliknięto przycisk!");
    }
};
```
Stara wersja (makra):  
`EVT_BUTTON(wxID_ANY, MyFrame::OnButtonClick)`

---

## 50. Pasek menu i pop-up menu – przykłady zastosowań

**Pasek menu:**
```cpp
wxMenuBar* menubar = new wxMenuBar();
wxMenu* menuPlik = new wxMenu();
menuPlik->Append(wxID_EXIT, "Wyjście");
menubar->Append(menuPlik, "Plik");
SetMenuBar(menubar);
```

**Pop-up menu:**
```cpp
wxMenu menu;
menu.Append(wxID_ANY, "Opcja 1");
PopupMenu(&menu, wxPoint(50,50));
```

---

## 51. Elementy kontrolne wxWidgets – wymień i scharakteryzuj

- **wxButton** – przycisk.
- **wxTextCtrl** – pole tekstowe (edytowalne/nieedytowalne).
- **wxStaticText** – statyczny tekst (label).
- **wxCheckBox** – pole wyboru (checkbox).
- **wxRadioButton** – przycisk radiowy (grupa wyboru).
- **wxComboBox** – lista rozwijana z możliwością edycji.
- **wxListBox** – lista elementów.
- **wxSlider** – suwak.
- **wxGauge** – pasek postępu.
- **wxPanel** – kontener do grupowania kontrolek.

---

## 52. Sizery („sortowniki”) w wxWidgets – wymień i krótko scharakteryzuj

- **wxBoxSizer** – układa elementy w poziomie (`wxHORIZONTAL`) lub pionie (`wxVERTICAL`).
- **wxGridSizer** – siatka prostokątna, wszystkie komórki równej wielkości.
- **wxFlexGridSizer** – siatka z możliwością elastycznego (dynamicznego) rozmiaru wierszy/kolumn.
- **wxStaticBoxSizer** – jak BoxSizer, ale z widoczną ramką i etykietą (grupuje wizualnie elementy).

**Przykład:**
```cpp
wxBoxSizer* sizer = new wxBoxSizer(wxVERTICAL);
sizer->Add(new wxButton(this, wxID_ANY, "Przycisk 1"), 0, wxALL, 5);
SetSizer(sizer);
```

---

> **Wskazówka:**  
> Jeśli chcesz rozwinąć któryś punkt, dodać kolejne przykłady programów lub mieć wersję angielską, daj znać!
