# Repository

Repository je interface, ktorého implementácia slúži na zapisovanie a čítanie objektov z databázy. Neobsahuje doménovú logiku.

## Metódy

Vo všeobecnosti podľa DDD platí, že programátor si môže vybrať jeden zo štandardov, ako nazývať metódy v Repository:
1. Collection-like: add, remove, get
2. CRUD-like: create, read, update, delete, findByXXX

V PosAme fungujeme podľa štandardu číslo 2, čiže žiadna z metód v Repository sa nemôže volať inak než create, read, update, delete alebo findBy. Metódy už nemajú žiadnu inú predponu alebo príponu, pretože je zbytočné písať napríklad OrganizationRepository.createOrganization. Z toho, že ide o OrganizationRepository, je zrejmé, že metóda "create" vytvára Organization. 

Pozor! Metódy v Repository musia pracovať výlučne len s tými objektami, ktorých názov je súčasťou Repository. Takže napríklad UserRepository nemôže mať metódu "findOrganization()", ktorá vráti organizáciu daného používateľa. Takýto prípad treba riešiť tak, že do OrganizationRepository vytvoríme metódu findByUserId(), ktorá vráti Organization.

Metóda "findBy" môže byť viackrát použitá a vždy má v názve jeden alebo viac atribútov, podľa ktorých sa objekty vyhľadávajú. Čiže napríklad "OrganizationRepository.findByAddress(address: Address): Collection<Organization>". Na nájdenie objektu podľa ID sa používa read, ktorej vstupným parametrom je jednoznačný identifikátor a výstupom je práve jeden objekt: OrganizationRepository.read(id:long): Organization"

# Factory

Metódu vo Factory nazývame "create". Napríklad OrganizationFactory.create. Opäť už netreba používať dlhý názov "createOrganization", lebo význam je zrejmý z kontextu. Špeciálne mutácie pre "create" vytvárame vtedy, ak majú doménový význam - v našom príklade sme mali DiscussionFactory.createQuestion/createAnswer. Druhou možnosťou je preťažiť create, napríklad AddressFactory.create(ulica,psc,obec) a AdressFactory(psc,obec) - lebo sú aj adresy bez ulice.

# Doménová logika

Doménovú logiku umiestňujeme do jednej z týchto tried (napríklad mám triedu Organization):

1. Samotná doménová trieda: ideálne, ak sa týka len atribútov danej triedy - napríklad Organization.invalidate() - zneplatní organizáciu k danému dňu. Následne budeme ešte z Controllera volať OrganizationRepository.update(organization), aby sme zmeny uložili do databázy.
2. Factory - v prípade vytvárania triedy: OrganizationFactory.create - vytvorí organizáciu a pošle notifikáciu ... Následne budeme volať ešte z Controllera OrganizationRepository.create(organization) na zápis zmien do DB.
3. DomainService - napríklad chceme notifikovať používateľov, ktorí sú zaradení do dvoch organizácií: v repository definujeme OrganizationRepository.findByUserId(id): Collection<Organization>, ktorú následne voláme z OrganizationDomainService.notifyDuplicities(user: User). Táto logika nepatrí do Repository (lebo je doménová), ani do jednej organizácie (lebo sa týka viacerých organizácií) a ani do Factory (lebo nesúvisí s vytváraním organizácie).

# Dátové typy

Na ukladanie peňazí používame v jave dátový typ BigDecimal. Primitívne typy double a float sú nevhodné, lebo nemusia byť presné (1+1=1.9999).
