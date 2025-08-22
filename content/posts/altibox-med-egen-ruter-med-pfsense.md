---
_edit_last: "1"
author: wpgundersen
categories:
  - freebsd
  - norwegian
date: "2014-04-06T13:54:25+00:00"
guid: http://www.gundersen.net/?p=244
parent_post_id: null
post_id: "244"
tags:
  - altibox
  - freebsd
  - hp-microserver
  - ipv6
  - pfsense
title: Altibox med egen ruter med pfSense
url: /altibox-med-egen-ruter-med-pfsense/
wp_featherlight_disable: ""

---
**Denne posten fra 2014 er oppdatert i 2015 med IPv6-innstillinger - se nederst.**

De fleste som har Altibox fiber-bredbånd og vil bruke egen ruter hjemme setter Altibox-ruteren i bridge-mode. Men det er unødvendig å ha den som et ekstra ledd mellom deg og internett. Du kan like gjerne kople din egen ruter eller server rett på linjen.

{{< figure align="alignnone" width=450 src="//gundersen.net/wp-content/uploads/2014/04/Zyxel%5FAltibox.jpg" alt="Denne trengs ikke" caption="Denne trengs ikke" >}}

Om du ikke har vært Altiboxkunde alt for lenge så har du en mediekonverter foran ruteren som leveres av Altibox. Da kopler du deg direkte i mediekonverteren. Har du ikke en mediekonverter er jobben litt mer innfløkt fordi du må direkte på fiberen med f.eks egen mediekonverter.

![Mediakonverter](//gundersen.net/wp-content/uploads/2014/04/Mediakonverter_Altibox.jpg)

En PC med to nettverkskort, hvorav kortet som skal være WAN støtter VLAN (det gjør de fleste) er alt som trengs. Jeg bruker en HP Microserver med et ekstra nettverkskort og [pfSsense](https://www.pfsense.org/ "pfSense"). HP Microserver sitt innebygde nettverkskort støtter VLAN. Boksen har heller ingen problemer med de høye båndbreddene man kan få hos Altibox. Vil du bare teste så fungerer det helt fint å starte pfSense fra USB-pinne uten å installere.

Internett leveres på VLAN 102 og den offentlige IP-adressen kommer med DHCP. TV leveres på VLAN 101. Takk til første poster i [denne posten](http://freak.no/forum/showthread.php?t=219922) for disse opplysningene. Det er mulig rettskriving ikke er posterens beste side, men han vet hva han snakker om.

Når pfSense starter spør den først etter evt VLAN. Svar ja og oppgi VLAN-tag 102 på WAN-kortet. Om WAN-kortet heter bge0 skal deretter WAN bindes til bge0\_vlan102. LAN bindes til det andre kortet.  Videre innstillinger kan gjøres i pfSense webinterface på http://192.168.1.1/.

![pfSense Altibox](//gundersen.net/wp-content/uploads/2014/04/pfSense_Altibox.png)

pfSense setter som default opp ruting og NAT mellom WAN og LAN og henter adressen på WAN-kortet via DHCP automatisk. Det er overraskende lite jobb å få dette til, jeg hadde satt av godt med tid til frustrasjon, men opplevde ingen. Ingen endringer på Altibox sine selvbetjeningssider er nødvendig. Det er heller ikke nødvendig å sette MAC-adressen på WAN-kortet til samme MAC som Altibox-ruteren. Alt bare virker.

(Oppdatert NOV2016: Ruting av TV-signalet ser ikke ut til å være like enkelt lenger. Jeg har selv ikke TV lenger og kan ikke undersøke.)

Altibox burde dokumentere denne fremgangsmåten på egne sider. Det gir ikke mer supportkostnader enn for bridge-mode-brukerne ("eget ansvar, plugg tilbake ruteren vår før vi hjelper deg"), men ville gitt Altibox som brand mye geek-cred. Og det er slike som velger bredbånd for venner og slektninger.

## IPv6

Når du først er i gang, så gå for [Altibox IPv6](http://www.altibox.no/privat/internett/ipv6 "Altibox IPv6") i tillegg. Altibox bruker en mellomstasjon på veien mot full IPv6 kalt 6rd (Rapid Deployment). Støtten for 6rd er ødelagt i pfSense fra januar 2013 til lanseringen av v2.2-beta i januar 2015. For å bruke 6rd i pfSense trenger man derfor en versjon før eller etter dette, f.eks v2.2.

IPv6 må skrues på to steder, WAN og LAN. i Interfaces \| WAN velger man IPv6 Configuration Type: 6rd tunnel. Under avsnittet 6RD Configuration skal 6RD Prefix være 2a01:79c::/30. Dette er en kombinasjon av de to opplysningene IPv6 Prefix og IPv6 Prefix Length hos Altibox. 6RD Border Relay settes til 213.167.115.92 (Altibox kaller denne IPv4 BR adresse). 6RD IPv4 Prefix Length skal være 0 bits (Denne kalles IPv4 Prefix hos Altibox).

Når WAN er satt opp vil IPv6-adressen vise i Dashboard sammen med IPv4 adressen. Man kan også gå i konsollet i pfSense og teste med ping6 facebook.com (legg merke til adressen facebook bruker, den er litt kreativ).

For å gi IPv6-adresser videre til LAN kan man velge flere løsninger. Les deg opp på IPv6 om du vil gjøre et informert valg her. Det enkleste er å sette IPv6 Configuration Type til Track Interface eller SLAAC. Vips så deles det ut adresser. Har du Android-enheter på nettet (dvs mobiltelefoner) så er SLAAC å foretrekke fordi det løser [dette problemet](https://code.google.com/p/android/issues/detail?id=79576). Mange moderne Android-enheter fungerer ikke med ren DHCPv6, utrolig nok. Man opplever da 1-2 sekund frys før den kopler over på IPv4 når det er en stund siden man har vært inne på en nettside.

Altibox oppgir DNS 2a01:798:0:8012::4, men mange bruker heller Googles DNS som på IPv6 har adressene 2001:4860:4860::8888 og 2001:4860:4860::8844. Disse kan legges inn i System \| General Setup i pfSense.

Fra enhver maskin på LANet kan man nå gå til f.eks ipv6.google.com eller [teste](http://test-ipv6.com/) ipv6-innstillingene. Score 10 av 10 mulige. Suksess!

Obs: Windows XP har ikke IPv6 støtte som standard. Både XP og Vista kan kreve litt ekstra konfigurering for å få opp IPv6. Oppgrader til nåtiden.
