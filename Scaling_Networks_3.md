# Deel 3 Scaling Networks

Als netwerkbeheerder van Cosci.be is je de opdracht gegeven het fabrieksgebouw te voorzien van netwerk. 
De fabriek bestaat uit twee verschillende sites. 
In elke site wil men op laag 3 een redundante verbinding met één enkele Internet ISP (Ricky’s Internet: Never Gonna Give You up; 203.0.113.16/29) en wil men hebben dat de link state databases zo klein mogelijk worden gehouden. Waarom?! 
Op de tweede site wil men in het switched netwerk de bottlenecks verminderen in de uplinks, deze voorwaarde is niet belangrijk voor site een. 
De distribution layer switch op site twee moet als root bridge ingesteld worden. Wat stel jij voor op site één?
Het aantal PC’s/servers:
* in vlan 2 is 120
* in vlan 4 is dit 44
*	in vlan 8 zijn er 17
*	in het admin vlan zijn er 2


De client toestellen krijgen IP’s toegewezen met DHCP.
Site één heeft 192.168.X.0/24 als start subnet en site twee - 172.16.X.0/24. De WAN lijnen worden onderverdeeld in 172.17.X.0/24 subnetten. 
Maak dat alle vlans intern met elkaar kunnen communiceren en dat communicatie met het Internet mogelijk is.

Mochten er kleine en korte vragen zijn, horen wij het graag. Lange of onduidelijke vragen worden niet beantwoord.

Succes!
