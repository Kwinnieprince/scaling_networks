# Deadlinegebonden praktijkopdrachten 1/2

## Inleiding

Als netwerkbeheerders van het bedrijf Cosci.be is jullie onderstaande opdracht toevertrouwd. Vanbij het begin en tijdens de uitvoer van deze opdracht dienen jullie over volgende aspecten duidelijkna te denken:

* technische analyse
* topologie + netwerkschema
* plan van aanpak (Kanban bord via Github)
* rapporteren
* maken en opvolgen van afspraken

## Goede afspraken maken goede vrienden

Stel een samenwerkingscontract op tegen de volgende les. Daarvoor dienen jullie na te denken overvolgende punten:

* Wat houdt een goede samenwerking in?
* Wie neemt welke rollen op zich?
* Wanneer en hoe vaak komen we (minstens) samen?
* ... 

Het samenwerkingscontract is een door alle leden van het team ondertekend document waarin jullieafspraken maken om deze praktijkopdracht tot een goed einde te brengen.

Deadline: Eerste les na de paasvakantie.

## Cosci.be

Cosci is een internationaal bedrijf met 5 vestigingen in Europa en Amerika.  Op de Aziatischeafzetmarkt hadden ze tot op vandaag nog geen vestiging, maar daar komt nu verandering in. De CEO van Cosci heeft vanmorgen een studiebureau aangesteld om een extra vestiging in Okayamaop te starten.Jullie hebben de opdracht gekregen om een bedrijfskritische netwerktopologie voor te stellen.

## Okayama

De vestiging in Okayama zal bestaan uit drie gebouwen: een fabrieksgebouw en twee administ&ratieve gebouwen. In het fabrieksgebouw dient momenteel geen netwerk voorzien te worden.

![](https://static.timesofisrael.com/atlantajewishtimes/uploads/2019/03/RE_Plaza-Midtown_Photo-2-640x400.jpg)

De Aziatische Cosci administratieve gebouwen

## Infrastructuur

Cosci krijgt in Okayama Internettoegang via twee verschillende ISP’s:

* AU
* Fusion GOL

Zorg dat ieder gebouw voorzien wordt van een enkele L3 toegang tot de verschillende ISP’s. Voorzie de verschillende verdiepingen van de gebouwen met minstens een access switch en aggregeer die op verdieping 8 en 21. Zorg dat deze 4 aggregatie switches redundant met elkaar verbonden worden. Enkel de switches op verdieping 8 hebben een verbinding tot hun respectievelijke router.

Hoewel de routers een verbinding hebben tot de verschillende ISP’s dienen jullie ervoor te zorgen dat de toegang in de verschillende gebouwen tot het Internet verdeeld wordt tussen de twee ISP’s. Voorzie uiteraard ook een redundante verbinding en eventuele failover met de andere ISP. Elke ISP stelt vijf IP’s ter beschikking: een voor de router en de overige vier voor NAT. Communicatie tussen de verschillende gebouwen dient uiteraard niet via de ISP’s te verlopen.

Voor de Aziatische vestiging zijn volgende VLANs voorzien:

* VLAN 66: Boekhouding
* VLAN 67: Management CxO
* VLAN 68: IT Management
* VLAN 69: Sales
* VLAN 70: Research and Development
* VLAN 71: Onderhoud
* VLAN 150: Voice

## Technologieën 

Zorg dat volgende technologieën gebruikt worden:

1. VTP
2. DTP
3. Inter-VLAN routering
4. Dynamische routing (multi area OSPF en EIGRP)
5. STP
6. Etherchannel en LACP
7. HSRP
8. Dynamische NAT
9. QoS (optioneel)


Succes!