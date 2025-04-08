import random
import time
import sys

def print_pozycja(text, delay=0.01):
    for char in text:
        sys.stdout.write(char)
        sys.stdout.flush()
        time.sleep(delay)
    print()

class SystemStudentow:
    def __init__(self):
        self.studenci = {}

    def dodaj_studenta(self, id, imie, wiek, kierunek, ocena):
        self.studenci[id] = {
            "imie": imie,
            "wiek": wiek,
            "kierunek": kierunek,
            "ocena": ocena,
            "status": "aktywny"
        }

    def usun_studenta(self, id):
        if id in self.studenci:
            del self.studenci[id]
            print_pozycja(f"Usunięto studenta o ID {id}")
        else:
            print_pozycja(f"Student o ID {id} nie istnieje")

    def aktualizuj_ocene(self, id, nowa_ocena):
        if id in self.studenci:
            self.studenci[id]["ocena"] = nowa_ocena
            print_pozycja(f"Zaktualizowano ocenę studenta {self.studenci[id]['imie']} do {nowa_ocena}")
        else:
            print_pozycja("Nie znaleziono studenta")

    def wyswietl_studentow(self):
        print_pozycja("Lista studentów:")
        for id, dane in self.studenci.items():
            print_pozycja(f"ID: {id}, Imię: {dane['imie']}, Wiek: {dane['wiek']}, Kierunek: {dane['kierunek']}, Ocena: {dane['ocena']}")

    def statystyka_ocen(self):
        if not self.studenci:
            print_pozycja("Brak danych o studentach")
            return
        oceny = [dane["ocena"] for dane in self.studenci.values()]
        srednia = sum(oceny) / len(oceny)
        print_pozycja(f"Średnia ocen: {round(srednia, 2)}")

    def najlepszy_student(self):
        if not self.studenci:
            print_pozycja("Brak studentów")
            return
        najlepszy = max(self.studenci.items(), key=lambda x: x[1]["ocena"])
        print_pozycja(f"Najlepszy student: {najlepszy[1]['imie']} z oceną {najlepszy[1]['ocena']}")

    def lista_kierunkow(self):
        kierunki = set(dane["kierunek"] for dane in self.studenci.values())
        print_pozycja("Dostępne kierunki:")
        for k in kierunki:
            print_pozycja(k)

    def znajdz_studenta(self, imie):
        znalezieni = [s for s in self.studenci.values() if s["imie"] == imie]
        if znalezieni:
            print_pozycja(f"Znaleziono {len(znalezieni)} studentów o imieniu {imie}:")
            for s in znalezieni:
                print_pozycja(f"Imię: {s['imie']}, Wiek: {s['wiek']}, Kierunek: {s['kierunek']}, Ocena: {s['ocena']}")
        else:
            print_pozycja(f"Nie znaleziono studenta o imieniu {imie}")

    def posortuj_studentow(self):
        posortowani = sorted(self.studenci.items(), key=lambda x: x[1]["ocena"], reverse=True)
        print_pozycja("Studenci posortowani według ocen:")
        for id, dane in posortowani:
            print_pozycja(f"ID: {id}, Imię: {dane['imie']}, Ocena: {dane['ocena']}")

def generuj_studentow(system, ilosc):
    imiona = ["Ania", "Bartek", "Celina", "Dawid", "Ela", "Filip", "Gosia", "Hubert", "Iga", "Jakub"]
    kierunki = ["Informatyka", "Biologia", "Ekonomia", "Matematyka", "Filozofia"]
    
    for i in range(1, ilosc + 1,):
        system.dodaj_studenta(
            id=i,
            imie=random.choice(imiona),
            wiek=random.randint(18, 26),
            kierunek=random.choice(kierunki),
            ocena=round(random.uniform(2.0, 5.0), 1)
        )

def menu(system):
    while True:
        print_pozycja("\nWybierz działanie:")
        print_pozycja("1 - Wyświetl listę studentów")
        print_pozycja("2 - Usuń studenta")
        print_pozycja("3 - Zaktualizuj ocenę studenta")
        print_pozycja("4 - Wyświetl statystyki ocen")
        print_pozycja("5 - Najlepszy student")
        print_pozycja("6 - Lista kierunków")
        print_pozycja("7 - Znajdź studenta po imieniu")
        print_pozycja("8 - Posortuj studentów według ocen")
        print_pozycja("9 - Zakończ program")

        wybor = input("Wybór: ")

        if wybor == "1":
            system.wyswietl_studentow()
        elif wybor == "2":
            id_studenta = int(input("Podaj ID studenta do usunięcia: "))
            system.usun_studenta(id_studenta)
        elif wybor == "3":
            id_studenta = int(input("Podaj ID studenta: "))
            nowa_ocena = float(input("Podaj nową ocenę: "))
            system.aktualizuj_ocene(id_studenta, nowa_ocena)
        elif wybor == "4":
            system.statystyka_ocen()
        elif wybor == "5":
            system.najlepszy_student()
        elif wybor == "6":
            system.lista_kierunkow()
        elif wybor == "7":
            imie = input("Podaj imię studenta: ")
            system.znajdz_studenta(imie)
        elif wybor == "8":
            system.posortuj_studentow()
        elif wybor == "9":
            print_pozycja("Zakończenie programu...")
            break
        else:
            print_pozycja("Niepoprawny wybór, spróbuj ponownie.")

system = SystemStudentow()
generuj_studentow(system, 10)
menu(system)
