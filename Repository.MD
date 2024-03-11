# Repository

Repository je interface, ktorého implementácia slúži na zapisovanie a čítanie objektov z databázy. Neobsahuje doménovú logiku.

## Metódy

Vo všeobecnosti podľa DDD platí, že programátor si môže vybrať jeden zo štandardov, ako nazývať metódy v Repository:
1. Collection-like: add, remove, get
2. CRUD-like: create, read, update, delete, findByXXX

V PosAme fungujeme podľa štandardu číslo 2, čiže žiadna z metód v Repository sa nemôže volať inak než create, read, update, delete alebo findBy. Metódy už nemajú žiadnu inú predponu alebo príponu, pretože je zbytočné písať napríklad OrganizationRepository.createOrganization. Z toho, že ide o OrganizationRepository, je zrejmé, že metóda "create" vytvára Organization. 

Pozor! Metódy v Repository musia pracovať výlučne len s tými objektami, ktorých názov je súčasťou Repository. Takže napríklad UserRepository nemôže mať metódu "findOrganization()", ktorá vráti organizáciu daného používateľa. Takýto prípad treba riešiť tak, že do OrganizationRepository vytvoríme metódu findByUserId(), ktorá vráti Organization.

Metóda "findBy" môže byť viac a vždy majú v názve jeden alebo viac atribútov, podľa ktorých sa objekty vyhľadávajú. Čiže napríklad "OrganizationRepository.findByAddress(address: Address): Collection<Organization>". Na nájdenie objektu podľa ID sa používa read, ktorej vstupným parametrom je jednoznačný identifikátor a výstupom je práve jeden objekt: OrganizationRepository.read(id:long): Organization"

# Factory

Metódu vo Factory nazývame "create". Napríklad OrganizationFactory.create. Opäť už netreba používať dlhý názov "createOrganization", lebo význam je zrejmý z kontextu.

# Dátové typy

Na ukladanie peňazí používame v jave dátový typ BigDecimal. Primitívne typy double a float sú nevhodné, lebo nemusia byť presné (1+1=1.9999).
