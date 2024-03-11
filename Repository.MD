# Repository

Repository je interface, ktorého implementácia slúži na zapisovanie a čítanie objektov z databázy. Neobsahuje doménovú logiku.

# Metódy

Vo všeobecnosti podľa DDD platí, že programátor si môže vybrať jeden zo štandardov, ako nazývať metódy v Repository:
1. Collection-like: add, remove, get
2. CRUD-like: create, read, update, delete, findByXXX
V PosAme fungujeme podľa štandardu číslo 2, čiže žiadna z metód v Repository sa nemôže volať inak než create, read, update, delete alebo findBy.
