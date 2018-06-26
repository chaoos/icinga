---


---

<h1 id="installation-debian">Installation (Debian)</h1>
<p>@Tom: Ich habe die Anleitung noch einmal komplett durchgespielt und mit den neuen Versionen der Packete sind mir einige Fehler nicht mehr passiert. Ich habe diese Passagen in der Anleitung auskommentiert, das sieht dann so aus:</p>
<pre><code>&lt;!-- Kommentar Beginn
	Dieser Teil wäre dann nicht mehr relevant.
--&gt; Kommentar Ende
</code></pre>
<h2 id="neuestes-debian-stable-herunterladen">Neuestes Debian (stable) herunterladen</h2>
<ul>
<li><a href="https://www.debian.org/distrib/">https://www.debian.org/distrib/</a></li>
<li>“64-Bit-PC Netinst-ISO”</li>
<li><a href="https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/debian-9.4.0-amd64-netinst.iso">https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/debian-9.4.0-amd64-netinst.iso</a>, weil es das kleinste ist: 250mb</li>
</ul>
<h2 id="grundinstallation">Grundinstallation</h2>
<p>@Tom: Falls du HyperV verwendest musst du “secure boot” ausschalten, sonst bootet das System nicht von der CD/DVD/ISO.</p>
<ul>
<li>
<p>Starten von der CD/DVD/ISO</p>
</li>
<li>
<p>Im Bootscreen “Install” wählen, das oberste.</p>
</li>
<li>
<p>Language “Deutsch”</p>
</li>
<li>
<p>Standort “Schweiz/Deutschland”</p>
</li>
<li>
<p>Layout “was-auch-immer”</p>
</li>
<li>
<p>Installation startet</p>
</li>
<li>
<p>Rechnername: “monsrv01” oder so</p>
</li>
<li>
<p>Domainname: lassen wir leer; das ist die Searchdomain die an nicht FQDN-DNS Abfragen angehängt wird. Kann später in <code>/etc/resolv.conf</code> gesetzt werden, genaueres siehe <a href="https://manpages.debian.org/stretch/manpages/resolv.conf.5.en.html">https://manpages.debian.org/stretch/manpages/resolv.conf.5.en.html</a>. (PS:ich werde immer auf <a href="http://manpages.debian.org">manpages.debian.org</a> referenzieren). Beispiel:</p>
<pre><code>search example.local
domain example.local
</code></pre>
</li>
<li>
<p>Root-Passwort: 2 mal eingeben</p>
</li>
<li>
<p>Neuen User erstellen; brauchen wir nicht, aber Debian lässt es nicht zu ohne weiter zu machen. Es will einen User, aber wir können ihn im nachhinein wieder löschen. Ich weiss nicht was du hier willst. Man kann auch mit diesem User arbeiten und die relevanten Befehle mit sudo starten, bin ich aber kein Freund davon. Wie du willst, ich arbeite mit root.</p>
</li>
<li>
<p>Partitionierung: ich nehme gerne “Geführt - gesamte Platte verwenden und LVM einrichten”, denn so kann man im nachhinein während dem Betrieb vergrössern, verkleinern, usw…</p>
</li>
<li>
<p>Festplatten partitionieren: die Festplatte auswählen</p>
</li>
<li>
<p>Partitionierungsschema: musst du selber wissen, aber ist hierfür Schnuppe, wenn du dazu etwas sagen willst, kannst du sagen: Wenn man alles in dieselbe Partition schmeisst, können normale User das System füllen (über ihr Home), bis kein Speicherplatz mehr da ist. Wenn man diese separiert, kann das nicht passieren. Auch <code>/var</code>  wo die Logs sind, will man nicht im Root haben; gleicher Grund wie oben. <code>/usr</code> könnte übers Netzwerk readonly  geshared werden. In grösseren Umgebungen macht man das auch separat, der Rechner ist nicht abhängig davon. <code>/tmp</code> ist für alle beschreibbar (+ Stickybit), gleicher Grund wie oben. Du kann noch sagen wer hier mehr wissen will, sei auf den FHS (File Hierarchy Standard) verwiesen:</p>
</li>
<li>
<p>Offiziell: <a href="http://www.pathname.com/fhs/">http://www.pathname.com/fhs/</a></p>
</li>
<li>
<p>P.S: Debian 9 ist FHS 2.3 konform: <a href="https://www.debian.org/doc/debian-policy/#document-ch-opersys">https://www.debian.org/doc/debian-policy/#document-ch-opersys</a></p>
</li>
<li>
<p>Für Infos über Debian Richtlinien generell: das “Debian Policy Manual”, immer eine gute Quelle: <a href="https://www.debian.org/doc/debian-policy/">https://www.debian.org/doc/debian-policy/</a></p>
</li>
<li>
<p>“Änderungen auf die Speichergeäte schreiben und LVM einrichten”: Ja.</p>
</li>
<li>
<p>“Partitionierung beenden und Änderungen übernehmen” -&gt; “Änderungen auf die Festplatte schreiben”: Ja</p>
</li>
<li>
<p>Installation läuft weiter</p>
</li>
<li>
<p>Paketmanager konfigurieren: Das sind einfach die Mirrors, die apt verwendet. Die werden in <code>/etc/apt/sources.list</code> eingetragen. Man wählt normalerweise welche im selben Land, und ich nehm immer die von Switch: “<a href="http://mirror.switch.ch">mirror.switch.ch</a>” die sind schnell.</p>
</li>
<li>
<p>Proxy haben wir nicht: leer lassen</p>
</li>
<li>
<p>Paketerfassung: was du willst, ich wähle immer: nein</p>
</li>
<li>
<p>Softwareauswahl: alles weg, ausser “Standard-Systemwerkzeuge”. Was wir brauchen installieren wir später. Geht auch schneller so.</p>
</li>
<li>
<p>Bootloader: installieren in den MBR</p>
</li>
<li>
<p>CD raus &amp; reboot (bei HyperV weiss ich nicht ob man noch etwas spezielles beachten muss, dass das System richtig bootet), bei KVM musste ich</p>
</li>
</ul>
<h2 id="grundkonfiguration">Grundkonfiguration</h2>
<ul>
<li>
<p>Ich gehe nun davon aus, dass das System gebootet hat und wir sind eingeloggt als root</p>
</li>
<li>
<p>Erst mal ssh drauf tun:</p>
<pre><code>$ apt-get install openssh-server
</code></pre>
</li>
<li>
<p>der Daemon startet selber</p>
</li>
<li>
<p>Login via root muss noch erlaubt werden. Editeren Sie <code>/etc/ssh/sshd_config</code> und setzen Sie <code>PermitRootLogin Yes</code>:</p>
<pre><code>$ vi /etc/ssh/sshd_config
PermitRootLogin yes
</code></pre>
</li>
<li>
<p>SSH Daemon neu starten:</p>
<pre><code>$ /etc/init.d/ssh restart
</code></pre>
</li>
<li>
<p>Von nun an bin ich über SSH und mit root drauf (zum Beispiel mit Putty), also der ganze Rest ist via ssh</p>
</li>
<li>
<p>NTP installieren:</p>
<pre><code>$ apt-get install ntp
</code></pre>
</li>
<li>
<p>Konfigurationdatei: <code>/etc/ntp.conf</code></p>
</li>
<li>
<p>Deinen NTP Server eintragen (kann auch ein Windows Domaincontroller sein, die bestehenden auskommentieren mit #):</p>
<pre><code>server ntp.wieduwillst.com
</code></pre>
</li>
<li>
<p>NTP neustarten:</p>
<pre><code>$ /etc/init.d/ntp restart # Die alte SysV-Variante
</code></pre>
</li>
<li>
<p>oder:</p>
<pre><code>$ service ntp restart # Die neue fancy Art (systemd)
</code></pre>
</li>
<li>
<p>Checken (ntp braucht seine 5 Minuten bis er synchon ist, je länger er läuft desto präziser ist die Zeit &amp; kleiner das Offset):</p>
<pre><code>$ ntpq -p
     remote           refid      st t when poll reach   delay   offset  jitter
============================================================       ==================
 metasntp13.admi .PPS.            1 u   14   64    3   12.013    6.115   1.401
</code></pre>
</li>
<li>
<p>Netzwerk einrichten (bisher DHCP der Installation). Man sollte dem Überwachungsserver eine fixe IP geben, denn für viele Services (zB SNMP auf Switches) verlangen IPs von welchen Geräten die Netzwerkkomponenten abgefragt werden dürfen.</p>
</li>
<li>
<p><code>/etc/network/interfaces</code> (könnte so aussehen):</p>
<pre><code> allow-hotplug eth0
 iface eth0 inet static
         address 192.168.1.10
         netmask 255.255.255.0
         gateway 192.168.1.1
</code></pre>
</li>
<li>
<p>Netzwerk neu starten (Achtung SSH Verbindung bricht zusammen):</p>
<pre><code> $ /etc/init.d/networking restart # SysV
 $ service networking restart # systemd
</code></pre>
</li>
<li>
<p>Ich werde von nun an nur noch die SysV Variante zeigen.</p>
</li>
<li>
<p>Nun ist die IP statisch, auch nach einem Reboot</p>
</li>
<li>
<p>Computername:</p>
<ul>
<li>in <code>/etc/hostname</code> setzen (hat die Installationsroutine schon gemacht)</li>
</ul>
</li>
<li>
<p>Updaten des Systems</p>
<ul>
<li>
<p>Packetquellen aktualisieren (von den eingestellten Mirrors, nur die Liste)</p>
<pre><code>$ apt-get update
</code></pre>
</li>
</ul>
</li>
<li>
<p>Neuere Versionen der installierten Packete herunterladen und installieren (löscht nichts):</p>
<pre><code>$ apt-get upgrade
</code></pre>
</li>
</ul>
<h2 id="icinga">Icinga</h2>
<ul>
<li>
<p>Installation:</p>
<pre><code>$ apt-get install icinga
</code></pre>
</li>
<li>
<p>Wichtige Packete, die auch mitinstalliert werden (ist eine riesen Liste): apache, mysql, nagios-plugins, snmp, sambaclient, perl, php</p>
</li>
<li>
<p>Fragen während der Installation (Reihenfolge kann variieren):</p>
</li>
<li>
<p>Icinga: Passwort für die Web-Administration von Icinga “icingaadmin”: setzen</p>
</li>
<li>
<p>Icinga: Externe Befehle verwenden? Ja (was das ist kommt später)</p>
</li>
<li>
<p>Icinga: Apache-Server für Icinga auswählen (evtl. kommt diese Frage nicht mehr)</p>
</li>
<li>
<p>Fertig</p>
</li>
<li>
<p>Nun kann man aufs Webinterface:</p>
</li>
<li>
<p><code>http://&lt;ip&gt;/</code> für die Debian Apache2 “it works” Seite</p>
</li>
<li>
<p><code>http://&lt;ip&gt;/icinga/</code> fürs Webinterface von Icinga<br>
username: icingaadmin<br>
pw: haben wir während der Installation gesetzt</p>
</li>
<li>
<p>Man landet im “Tactical Monitoring Overview”</p>
</li>
<li>
<p>Beim rumklcken sieht man, dass der “localhost” schon überwacht wird, jetzt gehts ums füllen.</p>
</li>
</ul>
<p>Ein kleine Anpassung müssen wir noch machen, denn der “Disk Space” Check überprüft ALLE Filesysteme, auch die virtuellen/pseudo Filesysteme. Diese müssen wir im “check_all_disks” Kommando in der Konfiguration ausklammern:</p>
<pre><code>$ vi /etc/nagios-plugins/config/disk.cfg
define command{
        command_name    check_all_disks
        command_line    /usr/lib/nagios/plugins/check_disk -X proc -X sysfs -X tmpfs -X devtmpfs -w '$ARG1$' -c '$ARG2$' -e
}
</code></pre>
<h2 id="icinga---generell">Icinga - Generell</h2>
<p>Icinga ist ein “Fork” des wohlbekannten Überwachungssystems Nagios. Eine 100%ige Kompatibilität mit den internen Strukturen des letzteren erlaubt es Ihnen, mit Icinga alle Plugins und Addons zu benutzen, die von verschiedenen Firmen und der großen Community entwickelt wurden bzw. werden.</p>
<p>Bevor man mit der Konfiguration beginnt, sollte man sich daüber im klaren sein, was man eigentlich überwachen will und wie genau. Ein komplettes Netz effektiv zu überwachen und das Konfigurieren wird seine Zeit dauern und einiges an Arbeit erfordern. Nehmen Sie sich diese Zeit!</p>
<p>Es wird zwischen 2 verschiedenen Arten von Checks unterschieden:</p>
<ul>
<li>Aktive Checks<br>
Diese Checks werden vom Rechner, auf dem Icinga läuft, periodisch und aktiv ausgeführt und ausgewertet. Wie das System an seine Informationen gelangt kann unterschiedlich sein. Zum Beispiel kann es mittels SNMP, Werte aus dem zu überwachenden System auslesen oder über einen auf dem System installiereten Daemon kommunizieren. Unter Unix-basierenden Systemen ist es auch üblich Informationen über SSH auszulesen. Auch können Dienste, die von Zielsystem angeboten werden (http, mail, ftp) direkt von aussen überprüft werden, indem eine Verbindung auf diesen Port initiiert wird. Ein Mail-Server, beispielsweise kann überwacht werden, indem:
<ul>
<li>der Port des Dienstes überprüft wird oder</li>
<li>der Check sich über SMTP einloggen kann oder</li>
<li>indem der Prozess auf dem Server überwacht wird oder</li>
<li>indem ein Testmail verschickt wird, dessen Versand überprüft wird.</li>
</ul>
</li>
<li>Passive Checks<br>
Von passives Checks ist die Rede, wenn der Überwachungsrechner den Check nicht selber initiiert, sondern ein anderes System. Die Ergebnisse werden somit in einem Intervall (freshness_threshold) übermittelt und auf dem Überwachungsrechner nur ausgewertet. Das kann sinnvoll sein, wenn Sie zum Beispiel eine komplexere Umgebung mit Firewalls zwischen einzelnen zu überwachenden Netzen haben. Bei riesigen Settings mit tausenden von Systemen und Services, kann es sinnvoll sein, nicht alle Checks von einem einzigen Rechner auszuführen.</li>
</ul>
<p>Streng genommen gibt es noch weitere Möglichkeiten. Zum Beispiel SNMP Traps, die oft auf Switches eingerichtet werden können und einen Zielrechner informieren, wenn etwas nicht in Ordnung ist; zum Beispiel ein Link geht down. Solche Dinge können allerdings auch über aktive Checks ermittelt werden.</p>
<h2 id="konfiguration-icinga">Konfiguration Icinga</h2>
<h3 id="konfigurationsschema-generell">Konfigurationsschema generell</h3>
<p>Die Konfigurationsdateien in <code>/etc/icinga/</code> (nicht erschrecken, dass es so viele sind, das ist nur aus Gründen der Übersicht, wenn man vieles überwacht)</p>
<ul>
<li><code>/etc/icinga/icinga.cfg</code> – Hauptkonfigurationsdatei, von hier aus werden alle anderen included</li>
<li><code>/etc/icinga/apache2.conf</code> – Konfiguration für den Webserver, wird vom apache eingelesen, nicht von icinga</li>
<li><code>/etc/icinga/cgi.cfg</code> - Konfiguration der CGIs; den Skripten, die das Webinterface und die Seiten zu generieren.</li>
<li><code>/etc/icinga/commands.cfg</code> – Definition der Check-Kommandos (diese werden über eine Shell ausgeführt, mehr siehe “Manuelles Testen von Checks” später)</li>
<li><code>/etc/icinga/htpasswd.users</code> – Benutzer mit Zugriff auf das Webinterface (icinaadmin:Passwort), standard htaccess Datei von Apache</li>
<li><code>/etc/icinga/resource.cfg</code> – Variablen zB Usernamen und Passwörter für die Check-Kommandos</li>
<li><code>/etc/icinga/stylesheets/*.css</code> – CSS Stylesheets für das Webinterface</li>
<li><code>/etc/icinga/objects/*</code> - Ordner mit den Objektdefinitionen</li>
<li><code>/etc/icinga/objects/hostgroups_icinga.cfg</code> - Definition der Hostgruppen</li>
<li><code>/etc/icinga/objects/localhost_icinga.cfg</code> - Die Checks des Localhosts, die wir beim Rumklicken schon gesehen haben</li>
<li><code>/etc/icinga/objects/contacts_icinga.cfg</code> - Definition der Kontakte für die Notifications</li>
<li><code>/etc/icinga/objects/services_icinga.cfg</code> - Definition der Services (das werden wir allerdings anders machen, die schreiben wir nicht alle da rein)</li>
<li><code>/etc/icinga/objects/generic-host_icinga.cfg</code> - Definitions-Template für normale Hosts</li>
<li><code>/etc/icinga/objects/timeperiods_icinga.cfg</code> - Definition der Zeiten, währenddessen Checks und Notifications ausgeführt werden sollen</li>
<li><code>/etc/icinga/objects/generic-service_icinga.cfg</code> - Definitions-Template für normale Services</li>
<li><code>/etc/icinga/objects/extinfo_icinga.cfg</code> - TODO</li>
<li><code>/etc/nagios-plugins/config/*</code> - Die Check Command Definitionen der Plugins</li>
</ul>
<p>Was von mir neu dazukommt:</p>
<ul>
<li><code>/etc/icinga/objects/servicegroups.cfg</code> – Definition der Servicegruppen</li>
<li><code>/etc/icinga/objects/generic-switch.cfg</code> – Definitions-Template für Switches</li>
</ul>
<p>Es empfielt sich für jedes Gerät, dass überwacht werden soll eine eigene Konfigurationsdatei zu erstellen, oder zumindest für Gruppen, die zusammengehören, denn sonst verliert man schnell den Überblick.<br>
Konfigurationsdirektiven in Icinga, wie auch in Nagios, sehen immer gleich aus. Es handelt sich um Definitionsblöcke eingeschlossen in geschwungenen Klammern <code>{}</code>. Alles in diesen Klammern gehört zum selben Element. Ein Element kann zum Beispiel ein Host, Service oder ein Check-Commmando sein. Mehr dazu später.</p>
<h3 id="allgemeine-einstellungen">Allgemeine Einstellungen</h3>
<p>Über die Konfigurationsdatei <code>/etc/icinga/icinga.cfg</code> werden alle anderen Dateien und Ordner geladen. Diese Datei enthält üblicherweise Einstellungen über den Daemon selber. Die Ausgangs-Konfigurationsdatei aus den Debian Repository ist schon sehr ausführlich kommentiert. Natürlich kann die komplette Konfiguration in dieser Datei sein, aber es empfielt sich, dass man Objektdefinitionen und Daemon-Einstellungen trennt. Hier nur die wichtigsten Einstellungen:</p>
<ul>
<li><code>cfg_file</code> &amp; <code>cfg_dir</code>: Diese beiden Direktiven laden eine zusätzliche Datei (bzw. Ordner mit allen Dateien darin) als Konfigurationsdateien.</li>
<li><code>icinga_user</code>: Der Systemuser unter dem der Daemon und sämtliche Checks laufen sollen. Die Paketinstallation hat schon einen User namens “nagios” mit Gruppe “nagios” erstellt. Es ist ein Systemuser (uid unter 1000), dessen Login gesperrt ist.</li>
<li><code>icinga_group</code>: Die Gruppe, siehe oben</li>
<li><code>check_external_commands</code>: Legt fest ob Icinga die Kommando-Datei (Direktive: <code>command_file=/pfad/zu/datei</code>) liest, um externe Kommandos auszuführen. Das ist nötig, wenn Sie über das Webinterface Host- oder Service-Kommandos absetzen wollen: wenn Sie zum Beispiel einen Service Check forcieren wollen.</li>
<li><code>service_check_timeout</code>: Zeit in Sekunden, bis ein Service Check abgebrochen wird.</li>
<li><code>host_check_timeout</code>: Zeit in Sekunden, bis ein Host Check abgebrochen wird.</li>
<li><code>enable_notifications</code>: Ein- und Ausschalten der Notifications</li>
<li><code>process_performance_data</code>: Ein- und Ausschalten der Verarbeitung von Performance Daten (siehe “Graphen mit PNP4Nagios &amp; einbinden ins Webinterface”)</li>
<li>evtl. mehr (TODO)</li>
</ul>
<h4 id="simple-host-definition">Simple Host Definition</h4>
<p>Diese definitieren den zu überwachenden Host per IP Adresse. Hier ein Beispiel:</p>
<pre><code>define host{
    use             generic-host
    host_name       websrv01
	hostgroups		http_servers
	alias           Unser Web Server
    address         1.2.3.4
}
</code></pre>
<ul>
<li><code>use</code>: Mit “use” wird eingeleitet welches Template für diese Hostdefinition verwendet werden soll, den es gibt noch viel mehr einzustellen als nur diese 4 Eigenschaften. Da mann nicht immer alle 43 Optionen definieren möchte, können diese in einem Template einmal gesetzt werden. Unser Template hier heisst “generic-host”, seine Definition ist zu finden in <code>/etc/icinga/objects/generic-host_icinga.cfg</code> und auch da sind nicht alle 43 Optionen gesetzt, denn nicht alle sind obligatorisch. Von einigen werden auch Defaultwerte genommen, wenn sie nicht gesetzt sind. Eine Liste mit Dokumentation findet man auf dem Webinterface unter <code>http://&lt;ip&gt;/icinga/docs/de/objectdefinitions.html#host</code>.</li>
<li><code>host_name</code>: Dies definiert den Namen des Hosts, wie er im Webinterface dargestellt werden soll (idealerweise nicht etwas wie “Unser Web Server”, sondern es empfielt sich den Computernamen zu nehmen, zB “websrv01” oder den FQDN “websrv01.domain.local”).</li>
<li><code>hostgroups</code>: Name der Hostgruppe, der der Host zugeordnet ist, mehr dazu später.</li>
<li><code>alias</code>: Für Icinga nicht relevant, nur für den Benutzer; wird im Interface angezeigt</li>
<li><code>address</code>: Das ist wichtig: Hier tragen Sie die IP Adresse des Hosts ein, wie er vom <strong>Icinga-Server</strong> aus erreichbar ist. Theoretisch kann hier alles drinstehen, was zur eindeutigen Identifikation des Hosts genutzt wird. Es empfielt sich nicht, hier einen DNS-Namen einzutragen, es würde zwar auch funktionieren, aber wenn der DNS Dienst ausfällt, kann der Host nicht mehr überprüft werden. Das wäre entgegen dem Sinn eines Überwachungssystems, denn mit dem Server mag vielleicht alles in Ordnung sein, nur das DNS hat Probleme. Darum sollte es auch separat überprüft werden. Diese Option wird häufig (aber nicht immer) in Checks verwendet um den Host zu checken.</li>
</ul>
<h4 id="simple-service-definition">Simple Service Definition</h4>
<p>Genau gleich wie Hosts definiert werden, werden auch Services definiert. Die Services sind die eigentlichen Checks. Sie können alles mögliche sein. Einzig müssen sie einen Wert zurückgeben, mit dem Icinga etwas anfangen kann. Diese Wert sind: <code>OK</code>, <code>WARNING</code>, <code>CRITICAL</code>, <code>UNKNOWN</code>. Mehr dazu unter “Manuelles Testen von Checks”. Hier ein Beispiel:</p>
<pre><code>define service{
        use                      generic-service
        host_name                websrv01
        service_description      Total Processes
        check_command            check_procs!250!400
}
</code></pre>
<ul>
<li><code>use</code>: Wie bei den Hosts, gibt es auch bei den Services unzählige Optionen (ca. 40), die den Service beschreiben. <code>generic-service</code> ist ein Template indem schon einiges Allgemeines definiert wurde. Zu finden: <code>/etc/icinga/objects/generic-service_icinga.cfg</code>.</li>
<li><code>host_name</code>: bestimmt welchem Host der Service zugeordnet werden soll. Das MUSS derselbe Name sein, wie in der Hostdefinition. So wird der Service einem Host zugeordnet und das Check Kommando kann dann die IP Adresse des Hosts für den Service Check verwenden.</li>
<li><code>service_description</code>: Der Name, wie der Service im Webinterface angezeigt wird.</li>
<li><code>check_command</code>: Hier wird der Service mit einem Kommando verbunden, dass schlussendlich in einer Shell ausgeführt wird. Das Kommando ist eingeteilt in mehrere Teile durch ! getrennt. Das heisst in <code>check_procs!250!400</code> wie oben wird das Kommando <code>check_procs</code> ausgeführt (welches gleich im nächsten Kapitel erklärt wird). Als Argument wird dem Kommando die beiden Argumente <code>250</code> und <code>400</code> übergeben. (als weiteres Argument wird oft das Icinga-interne <code>$HOSTADDRESS$</code> Makro übergeben, also das, was in der Host Definition unter <code>address</code> steht. Diverse solcher Makros lassen sich in den eigentlichen Check-Kommandos einsetzen. Für eine Liste aller verfügbaren Makros siehe: <code>http://&lt;ip&gt;/icinga/docs/de/macrolist.html</code>)</li>
</ul>
<h4 id="command-definition">Command Definition</h4>
<p>Als Beispiel, die Definition von “<code>check_procs</code>” (Anzahl laufender Prozesse):</p>
<pre><code>define command{
        command_name    check_procs
        command_line    /usr/lib/nagios/plugins/check_procs -w '$ARG1$' -c '$ARG2$'
}
</code></pre>
<ul>
<li>
<p><code>command_name</code>: Der Name des Kommandos; in der Service Definition wird darauf referenziert.</p>
</li>
<li>
<p><code>command_line</code>: Das eigentliche Shell-Kommando. Beachten Sie, dass es mit den absoluten Pfad angegeben werden sollte, denn in der Shell, in der es ausgeführt wird, haben wir nicht erzwungenermassen eine komplette Umgebung mit allen Variablen, wie in einer interaktiven Shell. Im Beispiel oben werden dem Check 2 Argumente übergeben: -w und -c. Üblicherweise bei Icinga/Nagios Checks bedeuten diese Argumente die Schwelle zum WARNING bzw. die Schwelle zum CRITICAL. Mehr Informationen dazu, und was die Checks sonst noch an Argumenten akzeptieren, kann meistens mit --help herausgefunden werden:</p>
<pre><code>$ /usr/lib/nagios/plugins/check_procs --help
</code></pre>
</li>
</ul>
<p>Die beiden Argumente erwarten jeweils einen spezifischen Wert. Diesen haben wir oben in der Serivce Definition unter check_command gesetzt: <code>check_procs!250!400</code>. Das erste Argument (250) wird ins Makro <code>$ARG1$</code> eingesetzt, das 2te (400) in <code>$ARG2$</code> usw… Unterstützt werden bis zu 32 Argumente; <code>$ARG1$</code> bis <code>$ARG32$</code>. Ausserdem können in der command_line Direktive andere Makros eingesetzt werden. Eines der wichtigsten ist das <code>$HOSTADDRESS$</code> Makro, dass mit dem Wert host_name der Host- oder Service-Definition ersetzt wird. So werden die eigentlichen Checks geschrieben. Ebenfalls wichtig sind die <code>$USERn$</code> Makros wobei n eine Zahl zwischen 1 und 256 ist. Defniniert werden sie üblicherweise in der Konfigurationsdatei <code>/etc/icinga/resources.cfg</code>. Sie sind gedacht um Benutzernamen und Passwörter zu speichern, die der Check allenfalls braucht. Später, wenn wir Windows- und Linux-Server einbinden, brauchen die Checks Zugangsdaten, um sich am Server zu authentifizieren und die Informationen auszulesen. Icinga unterstützt bis zu 256 User-Makros; <code>$USER1$</code> bis <code>$USER256$</code>.<br>
Hier nochmal zusammenfassend, wie aus dem check_command (aus einer Service- oder Host-Definition) ein Shell-Kommando wird:</p>
<pre><code> check_command check_sample!argument1!parameter!5
                            └───┬───┘ └───┬───┘ └─────────────────────────────────────┐
                                │         └─────────────────────────────────┐         │
                                └─────────────────────────────────┐         │         │
                                                                  │         │         │
 Host Makro ─────────────────────────────────────────┐            │         │         │
                                                     │            │         │         │
 User Makro ──────────┐                              │            │         │         │
                   ┌──┴──┐                     ┌─────┴─────┐    ┌─┴──┐    ┌─┴──┐    ┌─┴──┐
 command_line      $USER1$/sample-plugin.pl -H $HOSTADDRESS$ -a $ARG1$ -p $ARG2$ -n $ARG3$
                   └──┬──┘                     └─────┬─────┘    └─┬──┘    └─┬──┘    └─┬──┘
              ┌───────┘                            ┌─┘         ┌──┘         │        ┌┘
  ┌───────────┴───────────┐                     ┌──┴──┐    ┌───┴───┐    ┌───┴───┐    │
$ /usr/local/icinga/libexec/sample-plugin.pl -H 1.2.3.4 -a argument1 -p parameter -n 5
OK: Sample command successfully
</code></pre>
<h4 id="hostgroup-definition">Hostgroup Definition</h4>
<p>Nun will man nicht für jeden Host jeden Service separat definieren. Dafür sind Hostgroups gedacht. Hier ein Beispiel:</p>
<pre><code>define hostgroup{
    hostgroup_name          http-servers
    alias                   HTTP Servers
    members                 websrv01,websrv02,websrv03
    hostgroup_members		apache-servers,iis-servers
}
</code></pre>
<ul>
<li>
<p><code>hostgroup_name</code>: Der Name der Hostgruppe</p>
</li>
<li>
<p><code>alias</code>: Ein Alias als Name im Webinterface</p>
</li>
<li>
<p><code>members</code>: Diese Hosts befinden sich in der Hostgruppe. Ich empfehle allerdings die Hostgruppen Members in der Host Definition zu hinterlegen (ist übersichtlicher). Das sähe dann so aus für unseren Beispiel-Host:</p>
<pre><code>define host{
    use				generic-host
    host_name		websrv01
    hostgroups		http-servers
    alias			Unser Web Server
    address         1.2.3.4
  }
</code></pre>
</li>
<li>
<p><code>hostgroup_members</code>: Hostgruppen können auch verschachtelt werden. Das ist nützlich, wenn man verschiedene Typen desselben Services hat. Wenn wir nun zum Beispiel einen Apache Webserver haben, wollen wir Apache-spezifische Dinge checken, aber wir wollen auch, dass alle Apaches in der Gruppe http-servers sind und damit die Services zugewiesen bekommen, die übergreifend sind. Wenn also ein Host nun in der Hostgruppe “apache-servers” ist, ist er automatisch auch ein Mitglied von “http-servers”, ohne dass wir ihn separat zuweisen müssen. Der Host erbt alle Services, die in verschachtelten Hostgroups definiert wurden.</p>
</li>
</ul>
<p>Dieser Hostgroup weisen wir nun einen Service zu, denn wir wollen, dass alle Webserver so getestet werden:</p>
<pre><code>define service{
        use                     generic-service
        hostgroup_name          http-servers
        service_description     HTTP
        check_command           check_http
}
</code></pre>
<p>Wenn nun ein weiterer Webserver dazukommt, weisen wir ihn einfach der Hostgroup http-servers (oder apache-servers oder iis-servers) zu und schon werden ihm die entsprechenden Services zugewiesen.</p>
<h4 id="servicegroup-definition">Servicegroup Definition</h4>
<p>Auch Services können gruppiert werden, das hat aber nur kosmetischen Wert. Mann kann zum Beispiel alle Services, die die RAM-Auslastung überwachen, zusammennehmen und hat so im Webinterface auf einen Blick alle diese Services zusammen.</p>
<h4 id="kontakt---kontagruppen-definitionen">Kontakt- &amp; Kontagruppen Definitionen</h4>
<p>Hier werden die zu alarmierenden Kontakte definiert. In unserem Beispielszenario haben wir nur einen Kontakt. Dieser könnte so aussehen:</p>
<pre><code>define contact{
        contact_name                    tom
        alias                           Tom Wechsler
        host_notifications_enabled      1
        service_notifications_enabled   1
        service_notification_period     24x7
        host_notification_period        24x7
        service_notification_options    w,u,c,r
        host_notification_options       d,r
        service_notification_commands   notify-service-by-email
        host_notification_commands      notify-host-by-email
        email                           name@exmaple.com
}
</code></pre>
<ul>
<li><code>contact_name</code>: Definiert einen Namen</li>
<li><code>alias</code>: um den Kontakt zu identifizieren</li>
<li><code>host_notifications_enabled</code>: bei 1 wird der Kontakt benachrichtigt über ändernde Status der Hosts</li>
<li><code>service_notifications_enabled</code>:bei 1 wird der Kontakt benachrichtigt über ändernde Status der Services</li>
<li><code>host_notification_period</code>: definiert den Name des Zeitfensters (siehe “Zeitfenster Definition”) bei Hosts</li>
<li><code>service_notification_period</code>: definiert den Name des Zeitfensters (siehe “Zeitfenster Definition”) bei Services</li>
<li><code>service_notification_options</code> &amp; <code>host_notification_options</code>: Bei welchen Zuständen soll der Kontakt benachrichtigt werden (w: WARNING, c: CRITICAL, u: UNKNOWN, r: OK, f: Flapping, s: Downtime start und stop, n: keines)</li>
<li><code>host_notification_commands</code> &amp; <code>service_notification_commands</code>: Das Kommando zum benachrichtigen (findet sich auch in <code>/etc/icinga/commands.cfg</code>). Hier sehen sie gut, wie stark Icinga ausgebaut werden kann. Wenn Sie zum Beispiel SMS als Notifications verschicken wollen, müssen Sie Ihren Server so einrichten, dass über ein Shell Kommando SMS verschickt werden können. Dann müssen Sie nur die notification_commands dementsprechend abändern. Dieses SMS-Kommando kann selbstverständlich auch ein Skript sein, dass SMS über eine Webplattform versendet.</li>
<li><code>email</code>: die Email-Adresse (wird als Makro <code>$CONTACTEMAIL$ i</code>n den Kommandos verwendet)</li>
</ul>
<h4 id="zeitfenster-definition">Zeitfenster Definition</h4>
<p>Definiert einen Zeitperiode in der Benachrichtigungen verschickt werden oder in der Services gecheckt werden. Vordefinierte sind zu finden in <code>/etc/icinga/objects/timeperiods_icinga.cfg</code>: 24x7, workhours, nonworkhours.</p>
<h3 id="manuelles-testen-von-checks">Manuelles Testen von Checks</h3>
<p>Immer wieder muss ein Check manuell getestet oder überprüft werden. Die Nagios-Plug-Ins werden in der Regel unter <code>/usr/lib/nagios/plugins/</code> (<code>/usr/lib/nagios</code> ist das Home-Verzeichnis des Nagios Benutzers) abgelegt. Es handelt sich hierbei um Skripte oder Kompilate, die die Rückgabe der verschiedenen Checks ermitteln. Diese Dateien müssen <em>ausführbar</em> sein für den Benutzer “nagios”.<br>
Um ein Plug-In nun so zu testen, wie der Icinga-Prozess es ausführen würde, muss man sich mit dem Benutzer nagios anmelden und in den Ordner <code>/tmp/</code> wechseln (da der Login deaktiviert ist, müssen wir mit -s eine Shell angeben):</p>
<pre><code>$ su - nagios -s /bin/sh
$ cd /tmp
</code></pre>
<p>Von da aus startet man das Plug-In direkt und bekommt die Ausgabe in die Shell:</p>
<pre><code>$ /usr/lib/nagios/plugins/check_ping -H 127.0.0.1 -p 1 -w 5000,100% -c 5000,100%
PING OK - Packet loss = 0%, RTA = 0.01 ms|rta=0.015000ms;5000.000000;5000.000000;0.000000 pl=0%;100;100;0
</code></pre>
<p>Oder mit einem Host, der nicht erreichbar ist:</p>
<pre><code>$ /usr/lib/nagios/plugins/check_ping -H 1.2.3.4 -p 1 -w 5000,100% -c 5000,100%
PING CRITICAL - Packet loss = 100%|rta=5000.000000ms;5000.000000;5000.000000;0.000000 pl=100%;100;100;0
</code></pre>
<p>Hier sieht man auch schön, wie die Checks funktionieren. Nagios/Icinga überprüft den Rückgabewert des Kommandos (nicht den Output!). Dieser Rückgabewert speichert die Shell in der Variable <code>$?</code> und kann folgendermassen ausgelesen werden:</p>
<pre><code>$ /usr/lib/nagios/plugins/check_ping -H 127.0.0.1 -p 1 -w 5000,100% -c 5000,100%
PING OK - Packet loss = 0%, RTA = 0.01 ms|rta=0.015000ms;5000.000000;5000.000000;0.000000 pl=0%;100;100;0
$ echo $?
0
$ /usr/lib/nagios/plugins/check_ping -H 1.2.3.4 -p 1 -w 5000,100% -c 5000,100%
PING CRITICAL - Packet loss = 100%|rta=5000.000000ms;5000.000000;5000.000000;0.000000 pl=100%;100;100;0
$ echo $?
2
</code></pre>
<p>Die Returnvalues bedeuten:</p>
<ul>
<li><code>0</code>: OK: Der Check verlief erfolgreich</li>
<li><code>1</code>: WARNING: Die Messlatte, die als Warnung definiert wurde, ist überschritten</li>
<li><code>2</code>: CRITICAL: Die Messlatte, die als Kritisch definiert wurde, ist überschritten</li>
<li><code>3</code>: UNKNOWN: Ein sonstiger Fehler; oft fehlende oder falsche Argumente im Kommando</li>
</ul>
<p>Man beachte die Daten in Ouput hinter der Pipe (<code>|</code>). Dies sind die Performancedaten die PNP4Nagios (oder ein anderes Tool) weiter auswerten könnte.<br>
Wenn ein Plug-In auf diese Weise funktioniert und den gewünschten Output liefert, muss es an der Icinga-Konfiguration liegen, wenn ein Check sich nicht so verhält, wie er soll.</p>
<h2 id="graphen-mit-pnp4nagios--einbinden-ins-webinterface">Graphen mit PNP4Nagios &amp; einbinden ins Webinterface</h2>
<p>Manchmal kann es nützlich sein, auf einen Blick zu sehen, wie sich gewisse Werte eines zu überwachenden Systems über einen längeren Zeitraum verändern. Nagios/Icinga bieten eine solche Auswertung nicht von Haus aus an, aber die meisten Checks geben so genannte “Performance Daten” mit (siehe “Manuelles Testen von Checks” weiter oben). Diese sind, im Gegensatz zu den normalen Outputs der Checks, so formattiert, dass sie gut geparst und verarbeitet werden können. Das wurde von Nagios standardisiert siehe: <a href="https://nagios-plugins.org/doc/guidelines.html">https://nagios-plugins.org/doc/guidelines.html</a></p>
<p>PNP4nagios (<a href="https://docs.pnp4nagios.org/">https://docs.pnp4nagios.org/</a>) ist ein Addon für Nagios (da Icinga 1 100% kompatibel zu Nagios ist können alle Plugins/Addons auch verwendet werden). Es speichert die von den Checks zuruckgegebenen Performance Daten in RRD Datenbanken.</p>
<p>Wir installieren nun PNP4Nagios:<br>
In <code>/etc/apt/sources.list</code> die Debian Backports hinzufügen:</p>
<pre><code>deb http://ftp.de.debian.org/debian/ jessie-backports main
</code></pre>
<p>Den Packet-Cache updaten:</p>
<pre><code>$ apt-get update
</code></pre>
<p>PNP4nagios aus den Debian Backports installieren:</p>
<pre><code>$ apt-get install pnp4nagios
</code></pre>
<hr>
<pre><code>&lt;!--
	Anpassungen in /etc/pnp4nagios/apache.conf:
	* Referenz auf das richtige htpasswd file:
		AuthUserFile /etc/icinga/htpasswd.users
	Oder mit sed (@Tom: ist gefixt, war die falsche Datei):
	$ sed -i 's|\(\s\+AuthUserFile\s\+\).*|\1/etc/icinga/htpasswd.users|' /etc/pnp4nagios/apache.conf
	* Pfad zum Icinga Base Konfigurationsordner in /etc/pnp4nagios/config.php setzen:
	$conf['nagios_base'] = "/cgi-bin/icinga";
	* Oder mit sed:
	$ sed -i "s|\(\$conf\['nagios_base'\]\) = .*|\1 = \"/cgi-bin/icinga\";|" /etc/pnp4nagios/config.php

	PNP4nagios in der Apache Konfiguration aktivieren:
	$ cd /etc/apache2/conf-available
	$ ln -s ../../pnp4nagios/apache.conf pnp4nagios.conf
	$ cd ../conf-enabled
	$ ln -s ../conf-available/pnp4nagios.conf pnp4nagios.conf
	$ cd ../mods-enabled
	$ ln -s ../mods-available/rewrite.load rewrite.load
--&gt;
</code></pre>
<p>Nun den npcd Daemon (Nagios Performance C Daemon) aktivieren in</p>
<pre><code>/etc/default/npcd:
RUN="yes"
</code></pre>
<p>Oder mit <code>sed</code>:</p>
<pre><code>$ sed -i 's/^RUN=.*/RUN="yes"/' /etc/default/npcd
</code></pre>
<p>Dann starten:</p>
<pre><code>$ /etc/init.d/npcd restart
</code></pre>
<p>Den Apache Daemon neu starten:</p>
<pre><code>$ /etc/init.d/apache2 reload
</code></pre>
<p>pnp4nagios freigeben:</p>
<pre><code>$ rm /usr/share/pnp4nagios/html/install.ignore
</code></pre>
<p>Mit dem Browser auf: <code>http://&lt;ip&gt;/pnp4nagios</code></p>
<ul>
<li>Da sollte alles grün sein, ansonsten sind PHP Einstellungen falsch oder Abhängigkeiten nicht vorhanden.</li>
</ul>
<p>Falls alles in Ordnung ist, sollte die <code>install.php</code> Datei gelöscht werden:</p>
<pre><code>$ rm /usr/share/pnp4nagios/html/install.php
</code></pre>
<p>Nun ist icinga dran:<br>
Anpassungen in <code>/etc/icinga/icinga.conf</code>:</p>
<ul>
<li>
<p>Aktivieren des Processings für Performance Daten (alles hinter der Pipe “<code>|</code>” sind Performance Daten; siehe “Manuelles Testen von Checks”):</p>
<pre><code>process_performance_data=1
</code></pre>
</li>
<li>
<p>oder automatisch mit <code>sed</code>:</p>
<pre><code>$ sed -i 's/^\(process_performance_data\)=.$/\1=1/' /etc/icinga/icinga.cfg
</code></pre>
</li>
<li>
<p>Setzen des Kommandos für die Verarbeitung:</p>
<pre><code>service_perfdata_file_processing_command=pnp-bulknpcd-service
host_perfdata_file_processing_command=pnp-bulknpcd-host
</code></pre>
</li>
<li>
<p>Oder mit <code>sed</code>:</p>
<pre><code>$ sed -i 's/^#\?\(service_perfdata_file_processing_command\)=.*/\1=pnp-bulknpcd-service/' /etc/icinga/icinga.cfg
$ sed -i 's/^#\?\(host_perfdata_file_processing_command\)=.*/\1=pnp-bulknpcd-host/' /etc/icinga/icinga.cfg
</code></pre>
</li>
<li>
<p>Laden des broker moduls: Neue .cfg Datei erstellen in <code>/etc/icinga/modules/pnp4nagios.cfg</code> mit Inhalt:</p>
<pre><code>define module{
        module_name     pnp4nagios
        path            /usr/lib/pnp4nagios/npcdmod.o
        args            config_file=/etc/pnp4nagios/npcd.cfg
        module_type     neb
}
</code></pre>
</li>
</ul>
<p>Das <code>generic-host</code> (<code>/etc/icinga/objects/generic-host_icinga.cfg</code>) Template erweitern mit einem Link auf die pnp4nagios Graphen:</p>
<pre><code>action_url	/pnp4nagios/index.php/graph?host=$HOSTNAME$&amp;srv=_HOST_' class='tips' rel='/pnp4nagios/index.php/popup?host=$HOSTNAME$&amp;srv=_HOST_
</code></pre>
<p>Das <code>generic-service</code> (<code>/etc/icinga/objects/generic-service_icinga.cfg</code>) Template erweitern mit einem Link auf die pnp4nagios Graphen:</p>
<pre><code>$ vi /etc/icinga/objects/generic-service_icinga.cfg
action_url	/pnp4nagios/index.php/graph?host=$HOSTNAME$&amp;srv=$SERVICEDESC$' class='tips' rel='/pnp4nagios/index.php/popup?host=$HOSTNAME$&amp;srv=$SERVICEDESC$
</code></pre>
<p>Für den Tooltip muss ein Teil des Webinterfaces von Icinga verändert werden. Ermöglicht wird dies durch die CGI Includes, die es erlauben, Javascript-Code an geeigneter Stelle im Seitenkopf der Statusübersicht einzubinden:</p>
<pre><code>$ cp /usr/share/doc/pnp4nagios/examples/ssi/status-header.ssi /usr/share/icinga/htdocs/ssi/status-header.ssi

&lt;!--
	Fix eines Fehlers (PHPs Error Reporting ist zu restriktiv eingestellt), der das darstellen von Graphen verhindert (Tom, du kannst den Fehler im Video ja zeigen, wenn man auf einen Graphen klickt: "Non-static method nagios_Core::SummaryLink() should not be called statically, assuming $this from incompatible context"):
	Line 90 in /usr/share/pnp4nagios/html/index.php auskommentieren, mit sed:
	$ sed -i '90s/\(.*\)/#\1/' /usr/share/pnp4nagios/html/index.php
	TODO: warum?
--&gt;
</code></pre>
<p>Icinga neu starten</p>
<pre><code>$ service icinga restart
</code></pre>
<p>Diese beiden Links (für Hosts und Services) und die Tooltips sind nun im Webinterface ersichtlich, wenn man auf “Status Detail” oder “Host Detail” eines beliebigen Hosts geht. Wenn Sie draufklicken, sehen Sie die Graphen der Performance Daten im verschidenen Zeitspannen. Wenn die Graphen nicht erscheinen müssen Sie noch etwas Geduld haben, denn basierend auf den Check Intervallen müssen warten bis der npcd Daemon beginnnt die Daten auszuwerten. Man beachte: Wir haben die Graphen nun im generischen Template für Hosts und für Services hinterlegt, dass heisst ALLE Hosts und Services werden nun mit Graphen ausgestattet. Falls das nicht erwünscht ist, kann ein separates Template dafür gemacht werden und bei den Host und Services, bei denen Graphen erwünscht sind mit der use-Direktive (siehe “Simple Host Definition” &amp; “Simple Service Definition”) eingebunden werden.</p>
<h2 id="überwachung-von-systemen">Überwachung von Systemen</h2>
<p>Es ist nun an der Zeit die Theorie umzusetzen mit einem eigentlichen Beispiel.</p>
<h3 id="einbinden-eines-windows-systems">Einbinden eines Windows Systems</h3>
<p>Wenn Sie einen Windows Server mit Icinga überwachen wollen, brauchen Sie dazu einen Mechanismus auf dem Server selbst, der die Werte ausliest und die Ergebnisse liefert. Eine Clientsoftware ist also notwendig. Eine der simpelsten und vielseitigsten Lösungen bietet der NSClient++ (<a href="https://www.nsclient.org/">https://www.nsclient.org/</a>).</p>
<h4 id="einrichten-auf-dem-windows-system">Einrichten auf dem Windows System</h4>
<p>Folgendes muss auf dem Windows-Server erledigt werden, den ich winsrv01 nenne mit der IP 192.168.1.11.</p>
<ul>
<li>
<p>Download des neuesten Clients via <a href="https://www.nsclient.org/download/">https://www.nsclient.org/download/</a> (zur Zeit des Videos: 0.4.3.143).</p>
</li>
<li>
<p>Ausführen der Instalaltionsdatei <code>xyz.msi</code></p>
</li>
<li>
<p>Wir wählen die “Complete” Installation</p>
</li>
<li>
<p>Bei “Allowed Hosts” geben Sie die IP Adresse des Icinga-Servers ein</p>
</li>
<li>
<p>Password: eines setzen</p>
</li>
<li>
<p>Enable common check plugins, auswählen</p>
</li>
<li>
<p>Enable nsclient server (check_nt), die alte unsichere Methode, nicht auswählen</p>
</li>
<li>
<p>Enable NRPE server (check_nrpe), was wir benutzen werden, auswählen</p>
<ul>
<li>Safe mode</li>
</ul>
</li>
<li>
<p>Enable NCSA nicht auswählen</p>
</li>
<li>
<p>Enable Webserver nicht auswählen</p>
</li>
<li>
<p>Die Software installiert sich als Dienst und kann dementsprechend in <code>services.msc</code> gestartet und gestoppt werden (evtl. zeigen, der Service heisst “NSClient++” und dass er automatisch gestartet wird: weisst du besser als ich). Wenn zB Dinge in der Konfigurationsdatei (<code>nsclient.ini</code>, nicht die Icingakonfigurationsdatei) geändert wurden, muss er neu gestartet werden.</p>
</li>
<li>
<p>Der Installationsordner ist: <code>C:\Program Files\NSClient++\</code>.</p>
</li>
<li>
<p>Darin befindet sich nun die nsclient.ini Konfigurationsdatei, in der alle Einstellungen, die wir während der Installation getroffen haben, gesetzt wurden.</p>
</li>
<li>
<p>Wie NSClient funktioniert: evtl Bild aus <a href="https://docs.nsclient.org/_images/normal-nrpe.png">https://docs.nsclient.org/_images/normal-nrpe.png</a> zeigen</p>
</li>
<li>
<p>Wir verwenden NSClient++ mit NRPE. Das funktioniert fogendermassen:</p>
</li>
<li>
<p>Icinga führt das check_nrpe Kommando mit Argumenten aus, die NSClient mitteilen, welcher Check ausgeführt weden soll.</p>
</li>
<li>
<p>NSClient erhält das Kommando</p>
</li>
<li>
<p>NSClient führt das Kommando aus und erhält das Resultat in Form von OK,CRITICAL,WARNING,UNKNOWN und einer Nachricht und evtl. Performance Daten</p>
</li>
<li>
<p>NSClient sendet das Ergebnis zurück zum Icinga Server</p>
</li>
<li>
<p>Der Server verarbeitet das Ergebnis, wie alle anderen Ergebnisse</p>
</li>
<li>
<p>Bevor wir loslegen müssen wir erst mittels der ini-Datei NSClient++ konfigurieren. Dazu öffnen wir sie mit einem Text-Editor (egal welcher). Wir setzen oder ergänzen (Achtung: in den richtigen Sektionen setzen):</p>
<pre><code>[/modules]
CheckExternalScripts = 1
CheckEventLog = 1
CheckNSCP = 1
CheckDisk = 1
CheckSystem = 1
NRPEServer = enabled
[/settings/NRPE/server]
insecure = true
</code></pre>
</li>
<li>
<p>Nun kann NSCLient gestartet werden. Erst starten wir den NSClient im Test-Modus; so sehen wir alle Ausgaben/Fehler usw…</p>
</li>
<li>
<p>Eine <code>cmd.exe</code> öffnen:</p>
<pre><code>$ cd C:\Program Files\NSClient++
$ nscp test
</code></pre>
</li>
<li>
<p>Die Plugins sollten nun geladen werden und als letzte Zeile sollte stehen:</p>
</li>
<li>
<pre><code> D        cli Enter command to execute, help for help or exit to exit...
</code></pre>
</li>
<li>
<p>Geben Sie testhalber <code>checkcpu</code> ein und sehen Sie sich die Ausgabe an.</p>
</li>
</ul>
<h4 id="einrichten-auf-dem-überwachungsserver">Einrichten auf dem Überwachungsserver</h4>
<ul>
<li>
<p>Nun probieren wir es vom Überwachungsserver aus (gemäss “Manuelles Testen von Checks”):</p>
</li>
<li>
<p>Erst müssen wir allerdings noch das check_nrpe plugin installieren:</p>
<pre><code>$ apt-get install nagios-nrpe-plugin
</code></pre>
</li>
<li>
<p>Nun zum Testen:</p>
<pre><code>$ su - nagios -s /bin/sh
$ cd /tmp
$ /usr/lib/nagios/plugins/check_nrpe -H &lt;IP der Windows Maschine&gt;
</code></pre>
</li>
<li>
<p>Die Ausgabe sollte folgendermassen aussehen:</p>
<pre><code>I (0.4.3.143 2015-04-29) seem to be doing fine...
</code></pre>
</li>
<li>
<p>Die Ausgabe im cmd auf der Windows Maschine sollte folgendermassen aussehen:</p>
<pre><code>D       nrpe Accepting connection from: &lt;IP des Überwachungsservers&gt;, count=1
</code></pre>
</li>
<li>
<p>Nun probieren wir es noch einmal mit einem Kommando:</p>
<pre><code>$ /usr/lib/nagios/plugins/check_nrpe -H &lt;IP der Windows Maschine&gt; -c checkcpu
</code></pre>
</li>
<li>
<p>Die Ausgabe im Terminal (Bsp):</p>
<pre><code>OK: CPU load is ok.|'total 5m'=0%;80;90 'total 1m'=0%;80;90 'total 5s'=0%;80;90
</code></pre>
</li>
<li>
<p>Die Ausgabe auf dem Windows Server:</p>
<pre><code>D       nrpe Accepting connection from: &lt;IP des Überwachungsservers&gt;, count=1
D  w32system Created command: "detail-syntax=${time}: average load ${load}%"
</code></pre>
</li>
<li>
<p>Wenn alles klappt, kann das Test Kommando im cmd.exe mit ctrl-c abgebrochen werden und wir starten den Service über die “Dienste”.</p>
</li>
<li>
<p>Jetzt müssen wir noch Icinga konfigurieren</p>
<ul>
<li>
<p>Erst definieren wir die Hostgroup für Windows Server, sodass wir später, wenn neue Windows Server dazukommen, diese nur dieser Hostgroup zuordnen müssen. Dieser Hostgroup weisen wir gerade einen Service zu. Dazu die Datei <code>/etc/icinga/static/windows.cfg</code> erstellen (evtl. noch erst den Ordner <code>/etc/icinga/static</code> weil den gibts noch nicht):</p>
<pre><code>$ vi /etc/icinga/static/windows.cfg
define hostgroup{
      hostgroup_name          windows-servers
      alias                   Windows Servers
}

define service{
        use                     generic-service
        hostgroup_name          windows-servers
        service_description     CPU
        check_command           check_nrpe_1arg!checkcpu
}
</code></pre>
</li>
</ul>
</li>
<li>
<p>Beachten Sie: Das Kommando müssen wir nicht definieren, denn diese Definition ist mit dem Packet “nagios-nrpe-plugin” mitinstalliert worden. Die Definition befände sich in der Konfigurationsdatei <code>/etc/nagios-plugins/config/check_nrpe.cfg</code>, die auch von Icinga eingelesen wird. PS: Da sind 2 verschiedene.</p>
</li>
<li>
<p>Nun müssen wir noch den Host definieren und ihn der Hostgroup zuordnen. Neue Datei erstellen <code>/etc/icinga/objects/winsrv01.cfg</code>:</p>
<pre><code>$ vi /etc/icinga/objects/winsrv01.cfg
define host{
        use                     generic-host
        host_name               winsrv01
        hostgroups              windows-servers
        alias                   Unser Web Server
        address                 1.2.3.4
}
</code></pre>
</li>
<li>
<p>In der Hauptkonfigurationsdatei muss noch der Ordner <code>/etc/icinga/static/</code> als Konfigurationsordner definiert werden, sodass Dateien darin auch als Konfiguration eingelesen werden.</p>
<p>$ vi /etc/icinga/icinga.cfg<br>
cfg_dir=/etc/icinga/static/</p>
</li>
<li>
<p>Jetzt die Konfiguration neu laden:</p>
<pre><code> $ /etc/init.d/icinga reload
</code></pre>
</li>
<li>
<p>Und auf den Webinterface ist ein neuer Host sichtbar; der Windows Server, mit einem Service; dem CPU Check, den wir der Hostgroup zugeordnet haben.</p>
</li>
<li>
<p>Der Status wird vermutlich noch auf <code>PENDING</code> sein. Das bedeutet, dass er noch nie ausgeführt wird. Der Service wartet nun bis er an der Reihe ist. Wir wollten aber nich warten, wir wollen den Check forcieren. Dazu klicken wir auf den Check, das wir in der “Service Information” laden. Ganz rechts haben wir “Service Commands”. Da klicken wir auf “Re-schedule the next check of this service”, häckeln “Force Check” an und drücken “Commit”. Nun wurde der Check ins Scheduling mit Priorität “Forced” aufgenommen (nicht “Normal” wie vorher). Wir sollten also bald Resultate sehen. Hier sind noch einige zusätzliche Checks, die man der Hostgroup “windows-servers” zuordnen kann:</p>
<p>define service{<br>
use                     generic-service<br>
hostgroup_name          windows-servers<br>
service_description     Memory<br>
check_command           check_nrpe_1arg!checkmem<br>
}</p>
<p>define service{<br>
use                     generic-service<br>
hostgroup_name          windows-servers<br>
service_description     Drive Space<br>
check_command           check_nrpe_1arg!checkdrivesize<br>
}</p>
<p>define service{<br>
use                     generic-service<br>
hostgroup_name          windows-servers<br>
service_description     Event Log<br>
check_command           check_nrpe_1arg!checkeventlog<br>
}</p>
<p>define service{<br>
use                     generic-service<br>
hostgroup_name          windows-servers<br>
service_description     Uptime<br>
check_command           check_nrpe!checkuptime!MaxWarn=200d MaxCrit=365d<br>
}</p>
</li>
<li>
<p>Beachten Sie: beim “Uptime” Service, geben wir noch ein Argument mit <code>MaxWarn=100d MaxCrit=365d</code>. Dazu muss <code>allow arguments = true</code> in der <code>nsclient.ini</code> auf dem Windows-Server gesetzt sein. Hiermit definieren wir auch, ab wann der Check in den WARNING Zustand gelangen soll: nämlich nach 200 Tagen. Nach 365 Tagen in den CRITICAL Zustand. Diese Werte sind natürlich nur Beispiele und können angepasst werden.</p>
</li>
</ul>
<h4 id="windows---einen-speziellen-dienst-überwachen">Windows - Einen speziellen Dienst überwachen</h4>
<ul>
<li>
<p>Wenn Sie einen gewissen Dienst überprüfen wollen, ob er gestartet ist und läuft, müssen Sie dazu einen neuen Serivce schreiben. Als Beispiel nehmen wir den DHCP-Client Dienst.</p>
</li>
<li>
<p>Erst müssen wir herausfinden wie der Dienstname lautet. Dazu gehen Sie in die Dienste Ansicht auf den Windows Server.</p>
</li>
<li>
<p>Doppelklicken Sie auf den Dienst</p>
</li>
<li>
<p>In der Registrierkarte “Allgemein” unter “Dienstname” sehen wir den genauen Namen.</p>
</li>
<li>
<p>Kopieren Sie ihn.</p>
</li>
<li>
<p>Auf dem Überwachungsserver erstellen wir einen Service, den wir dem Host zuweisen. Weil es etwas spezielles ist, gehen wie davon aus wir wollen das nur auf dieser einen Maschine überwachen, demnach macht eine Hostgroup keinen Sinn:</p>
<pre><code>$ vi /etc/icinga/objects/winsrv01.cfg
define service{
        use                     generic-service
        host_name               winsrv01
        service_description     DHCP Client Service
        check_command           check_nrpe!checkservicestate!Dhcp
}
</code></pre>
</li>
<li>
<p>Das Argument im Kommando stimmt überein mit dem Dienstnamen, den wir kopiert haben.</p>
</li>
<li>
<p>Wie immer: Icinga Konfiguration neu laden:</p>
<pre><code>$ /etc/init.d/icinga reload
</code></pre>
</li>
<li>
<p>Sie können auch mehrere Dienste mit einem Check überwachen, indem Sie einfach mehrere Dienstname als Argument übergeben. Der Check wechselt in den CRITICAL Zustand, sobald einer nicht gestartet ist.</p>
</li>
</ul>
<h4 id="windows---einen-speziellen-prozess-überwachen">Windows - Einen speziellen Prozess überwachen</h4>
<ul>
<li>
<p>Analog zum obigen Beispiel überwachen wir ob der Prozess <code>explorer.exe</code> läuft.</p>
</li>
<li>
<p>So sähe der Service aus:</p>
<pre><code>define service{
        use                     generic-service
        host_name               winsrv01
        service_description     Explorer Prozess
        check_command           check_nrpe!checkprocstate!explorer.exe
}
</code></pre>
</li>
</ul>
<h4 id="windows---externe-scripts">Windows - Externe Scripts</h4>
<p>Mit externen Skripts haben Sie die Möglichkeit Batch-, VBS-, oder Powershell-Skripts auszuführen. Auch eine exe-Datei wäre natürlich möglich. Dies funktioniert nach folgendem Schema:</p>
<pre><code>┌───────────Überwachungsserver─────────────┐              ┌────────────────────────────────Windows-Server───────────────────────────────┐
│┌────────────┐            ┌──────────────┐│              │ ┌──────────────┐            ┌──────────────┐                ┌──────────────┐│
││   Icinga   │─führt-aus─&gt;│  check_nrpe  │┼─verbindet-zu─┼&gt;│ NRPE-Server  │─führt-aus─&gt;│ cscript.exe  │─interpretiert─&gt;│  script.vbs  ││
│└────────────┘            └──────────────┘│              │ └──────────────┘            └──────────────┘                └──────────────┘│
└──────────────────────────────────────────┘              └─────────────────────────────────────────────────────────────────────────────┘
</code></pre>
<ul>
<li>
<p>Als Beispiel wollen wir nun ermitteln, ob für einen Server Windows Updates verfügbar sind und wenn ja, welche. In einem Check soll das angezeigt werden.</p>
</li>
<li>
<p>Da das Skript seine Zeit braucht, um alle verfügbaren Updates zu ermitteln, müssen wir den Timeout Wert hochsetzen… Und zwar an nicht weniger als 5 Orten (siehe Schema oben).</p>
</li>
<li>
<p>Skript downloaden: <a href="https://github.com/chaoos/check_windows_updates/blob/master/check_windows_updates.wsf">https://github.com/chaoos/check_windows_updates/blob/master/check_windows_updates.wsf</a></p>
</li>
<li>
<p>Speichern in NSClient Installationsordner unter <code>\scripts</code>, normalerweise: <code>C:\Program Files\NSClient++\scripts</code></p>
</li>
<li>
<p>Anpassen der <code>nsclient.ini</code> Konfigurationsdatei. Nebst den bisherigen Einstellungen brauchen wir einige mehr. Achten sie auf die Sektionen in denen die Werte gesetzt werden sollen:</p>
<pre><code>[/settings/NRPE/server]
timeout=300
allow arguments = true

[/settings/external scripts]
timeout=300

[/settings/external scripts/scripts]
check_win_updates=cscript.exe //T:300 //NoLogo scripts\check_windows_updates.wsf /w:1 /c:100
check_win_updates_with_args=cscript.exe //T:300 //NoLogo scripts\check_windows_updates.wsf $ARG1$
</code></pre>
</li>
<li>
<p>Beachten Sie: Ich habe den Check hier 2 mal definiert. Einmal mit fixen Argumenten <code>/w:1 /c:100</code> und einmal soll das Argument von Icinga kommen. Welche Sie wählen, hängt davon ab, wo Sie diese Optionen definiert haben wollen.</p>
</li>
<li>
<p>Der Vorteil an der zweiten Definition ist, dass die Werte (für alle Windows-Server) einmal in der Icingakonfiguration geändert werden können. Sie bringt allerdings Sicherheitsprobleme mit sich. Man sagt, ein “dummer” Monitoring-Agent ist weniger gefährlich als einer, der Remote kontrolliert werden kann.</p>
</li>
<li>
<p>Der Vorteil an der ersten Definition ist, dass der Check als Ganzes auf der Clientseite definiert ist und so keine Argumente über Netz gehen. Das kann allerdings mühsam zu konfigurieren sein, vor Allem, wenn Änderungen anfallen.</p>
</li>
<li>
<p>In der obigen Konfiguration haben wir nun schon 3 Timeouts gesetzt: Einmal für den NRPE Server, einmal für externe Skripts und den vom <code>cscript.exe</code> (<code>/T:300</code>).</p>
</li>
<li>
<p>Nachden der Dienst neu gestartet wurde, muss noch Icinga konfiguriert werden.</p>
</li>
<li>
<p>Erst schreiben wir die Kommando Definition für unseren neuen Check. Ich bespreche beide Varianten: mit und ohne Argumente. Das Standard check_nrpe Kommando können wir nicht verwenden, denn es bricht ab nach 30 Sekunden. Hier die 2 neuen Kommandos in <code>/etc/nagios-plugins/config/check_nrpe.cfg</code>:</p>
<pre><code>$ vi /etc/nagios-plugins/config/check_nrpe.cfg
define command {
        command_name    check_nrpe_long
        command_line    /usr/lib/nagios/plugins/check_nrpe -t 300 -H $HOSTADDRESS$ -c $ARG1$
}

define command {
        command_name    check_nrpe_long_with_args
        command_line    /usr/lib/nagios/plugins/check_nrpe -t 300 -H $HOSTADDRESS$ -c $ARG1$ -a $ARG2$
}
</code></pre>
</li>
<li>
<p>Hier wird noch das 4te Timeout auf 5 Minuten gesetzt: das vom <code>check_nrpe</code> Kommando.</p>
</li>
<li>
<p>Nun schreiben wir den Service; auch hier schreibe ich beide, denn hier werden die Argumente gesetzt (oder eben nicht). In <code>/etc/icinga/static/windows.cfg</code>:</p>
<pre><code>$ vi /etc/icinga/static/windows.cfg
define service{
        use                     generic-service
        hostgroup_name          windows-servers
        service_description     Windows Updates
        check_command           check_nrpe_long!check_win_updates
        check_interval          1440
        retry_interval          120
        notifications_enabled   0
}

define service{
        use                     generic-service
        hostgroup_name          windows-servers
        service_description     Windows Updates mit Argumenten
        check_command                 check_nrpe_long_with_args!check_win_updates_with_args!"/w:2 /c:3"
        check_interval          1440
        retry_interval          30
        notifications_enabled   0
}
</code></pre>
</li>
<li>
<p>Sie sehen beim zweiten Service werden die Argumente in der check_command Direktive nach dem zweiten <code>!</code> gesetzt. Sie müssen in double quotes gesetzt werden, denn sie haben einen Leerschlag dazwischen; tun wir das nicht, wird nur der Teil bis zum Leerschlag weitergegeben.</p>
</li>
<li>
<p>Beachten Sie, dass ich hier 3 weitere Werte gesetzt habe, die wir noch nicht besprochen haben:</p>
<ul>
<li><code>check_interval</code>: Dies ist der Intervall in dem der Service überprüft werden soll, normalerweise (im Template <code>/etc/icinga/objects/generic-service_icinga.cfg</code>) ist er gesetzt auf 5 Minuten. Da die Windows Updates nicht alle 5 Minuten überprüft werden sollen, habe ich hier den Wert auf 1440 Minuten (1 Tag) gestellt. So wird also alle 24 Stunden gecheckt, ob auf dem Server Updates anstehen.</li>
<li><code>retry_interval</code>: Wenn der Check einen Zustand rückmeldet, der nicht “OK” ist, soll der Check nochmals überprüft werden nach der Anzahl Minuten in dieser Direktive. Wichtig ist auch der Wert in <code>max_check_attempts</code> (siehe auch im Template <code>/etc/icinga/objects/generic-service_icinga.cfg</code>), normalerweise auf 4 gesetzt. Das bedeutet, dass der Check 4 mal “checkt” wird und erst wenn er 4 mal misslingt wird eine Notification per Email gesendet. Der <code>retry_interval</code> Wert ist hier auf 30 Minuten gestellt. Der Service meldet also nach 3 * 30 Minuten (=90 Minuten) nach dem ersten WARNING/CRITICAL/UNKNOWN Zustand (also insgesamt 4 Checks), dass der Service ein Problem hat. Dieser Wert ist standardmässig auf 1 Minute gesetzt (auch im Template). Diese Konfigurationsoption wurde eingebaut, falls ein Check sich nach kurzer Zeit wieder erholt und man darum nicht direkt ein Mail möchte (bzw. 2 Mail, denn die Nachricht, dass sich der Check erholt hat, generiert auch eine Notification).</li>
<li><code>notifications_enabled</code>: Bestimmt ob für diesen Service Nachrichten (Notifications) gesendet werden sollen. Ich habe das hier ausgeschalten (0), denn die Windows Updates möchte ich einfach im Webinterface (als Zusammenfassung) sehen, aber nicht jedesmal ein Mail bekommen. Ist ganz Ihnen überlassen.</li>
</ul>
</li>
<li>
<p>Nun muss noch der letzte Timeout gesetzt werden. Es handelt sich um den von Icinga selber, muss also in der Konfigurationsdatei /etc/icinga/icinga.cfg gesetzt werden:</p>
<pre><code>$ vi /etc/icinga/icinga.cfg
service_check_timeout=300
</code></pre>
</li>
<li>
<p>Konfiguration neu laden:</p>
<pre><code>$ /etc/init.d/icinga reload
</code></pre>
</li>
<li>
<p>Im Webinterface klicken wir nun bei dem neuen Service wieder auf “Re-schedule the next check of this service” und “Force Check” um den Check manuell auszuführen (dauert etwa 40-300 Sekunden, je nach Anzahl Updates). @Tom: Du kannst ja hier beide Services im Webinterface zeigen, ich hab sie extra unterschiedlich genannt. Auch das Symbol vor dem Service kannst du kurz erwähnen: Es bedeutet, dass Notifications ausgeschalten sind, wie wir in der Servicedefinition angegeben haben.</p>
</li>
</ul>
<p>Das wars mit den Windows Server Checks. Weiter gehts mit Linux Systemen.</p>
<h3 id="einbinden-eines-linux-systems">Einbinden eines Linux Systems</h3>
<p>Um Linux Systeme zu überwachen gibt es mehrere Möglichkeiten. Im Grunde muss man einfach ein Kommando absetzen und das Resultat auswerten.</p>
<h4 id="mit-nrpe">Mit NRPE</h4>
<ul>
<li>Vorteile: gleich wie bei Windows Systemen, gleicher Client, einheitlich, gleiche Checks, wenig Aufwand</li>
<li>Nachteile: Installation, fehlende Sicherheit, Verbindungsaufbau, -abbau bei jedem Check</li>
</ul>
<h5 id="einrichten-auf-dem-linux-system">Einrichten auf dem Linux System</h5>
<ul>
<li>
<p>Download des neuesten Clients via <a href="http://sourceforge.net/projects/nagios/files/nrpe-2.x/">http://sourceforge.net/projects/nagios/files/nrpe-2.x/</a> (zur Zeit des Video Version 2.15). Wir laden den Source Code als Tarball herunter, denn diese Anleitung soll unabhängig von einer Distribution sein. Evtl. bietet der Distributor ein Packet in den Repositories an (Debian/Ubuntu Packet: <code>nagios-nrpe-server</code>). Als Testmaschine verwenden wir auch eine Debian 8 Standardinstallation. Zum kompilieren sind einige Packete und Abhängigkeiten notwendig. Wenn man diese installiert ist in jeder Distribution unterschiedlich. Ich zeige es hier am Beispiel einer Debian 8 Installation.</p>
</li>
<li>
<p>neuen Ordner machen (irgendwo) und runterladen:</p>
<pre><code>$ mkdir nrpe
$ cd nrpe
$ wget http://sourceforge.net/projects/nagios/files/nrpe-2.x/nrpe-2.15/nrpe-2.15.tar.gz/download -O nrpe-2.15.tar.gz
</code></pre>
</li>
<li>
<p>Beachten Sie: Es Kann gut sein, dass der Download-Link nicht mehr aktuell ist. Dann müssen Sie ihn über einen Browser kopieren.</p>
</li>
<li>
<p>Entpacken des Archivs:</p>
<pre><code>$ tar xf nrpe-2.15.tar.gz
$ cd nrpe-2.15
</code></pre>
</li>
<li>
<p>Nun müssen wir noch die Abhängigkeiten installieren: die SSL Bibliothek, openssl und die Tools zum kompilieren. Diese Abhängigkeiten werden auch vom configure-Skript angegeben falls sie noch nicht aufgelöst wurden.</p>
</li>
<li>
<p>Installation unter Debian:</p>
<pre><code>$ apt-get install libssl-dev openssl build-essential
$ ./configure --with-ssl=/usr/bin/openssl --with-ssl-lib=/usr/lib/x86_64-linux-gnu
</code></pre>
</li>
<li>
<p>Das Ende des Outputs sollte etwa folgendermassen aussehen:</p>
<pre><code>*** Configuration summary for nrpe 2.15 09-06-2013 ***:

 General Options:
 -------------------------
 NRPE port:    5666
 NRPE user:    nagios
 NRPE group:   nagios
 Nagios user:  nagios
 Nagios group: nagios


Review the options above for accuracy.  If they look okay,
type 'make all' to compile the NRPE daemon and client.
</code></pre>
</li>
<li>
<p>Nun kompilieren wir das Packet. Allerdings nicht mit <code>make all</code> wie in der Ausgabe erwähnt, denn wir brauchen nur den NRPE-Client, nicht auch noch den Server:</p>
<pre><code>$ make nrpe
</code></pre>
</li>
<li>
<p>Bevor wir den daemon installieren, müssen wir noch den Benutzer und die  Gruppe erstellen:</p>
<pre><code>$ groupadd -r nagios
$ useradd -r -m -d /var/lib/nagios -s /bin/false -g nagios nagios
</code></pre>
<ul>
<li><code>-r</code> erstellt einen Systembenutzer</li>
<li><code>-m</code> erstellt das Homeverzeichnis</li>
<li><code>-d</code> gibt das Homeverzeichnis an</li>
<li><code>-s</code> gibt die Shell an; <code>/bin/false</code> -&gt; der Benutzer soll nicht einloggen können</li>
<li><code>-g</code> gibt die Gruppe an</li>
</ul>
</li>
<li>
<p>Nun den Daemon installieren, inkl. Konfigurationsdatei</p>
<pre><code>$ make install-daemon
$ make install-daemon-config
</code></pre>
</li>
<li>
<p>Auch ein SysV Init-Skript haben wir im Verzeichnis. Dieses kopieren wir nach <code>/etc/init.d/</code> und setzen die Berechtigungen</p>
<pre><code>$ cp init-script.debian /etc/init.d/nagios-nrpe-server
$ chmod 755 /etc/init.d/nagios-nrpe-server
</code></pre>
</li>
<li>
<p>Der Daemon liegt nun in <code>/usr/local/nagios/bin/nrpe</code>, die Konfigurationsdatei in <code>/usr/local/nagios/etc/nrpe.cfg.</code> Wenn Sie das Packet über die Packetverwaltung installiert haben, werden die beiden Dateien woanders im System sein; Der Daemon in <code>/usr/sbin/nrpe</code> und die Konfigurationsdatei in <code>/etc/nagios/nrpe.cfg</code>. Warum das so ist: siehe <a href="http://www.pathname.com/fhs/pub/fhs-2.3.html">http://www.pathname.com/fhs/pub/fhs-2.3.html</a></p>
</li>
<li>
<p>Wir möchten, dass der Daemon beim Start des Systems automatisch gestartet wird (Beachten Sie: Dies wird in jeder Distribution anders gehandlet, hier am Beispiel von Debian 8):</p>
<pre><code>$ update-rc.d nagios-nrpe-server defaults
</code></pre>
</li>
<li>
<p>Nun brauchen wir noch die Plugins, sodass der Daemon auch etwas zu checken hat.</p>
</li>
<li>
<p>Wir besorgen Sie uns ebenfalls von der offiziellen Quelle <a href="http://nagios-plugins.org/downloads/">http://nagios-plugins.org/downloads/</a> (zur Zeit des Videos 2.1.1) und kompilieren sie manuell. Auch hier bieten zahlreiche Distributoren schon vorgefertigte Packete an (Im Falle von Debian: apt-get install nagios-plugins). Um unabhängig zu bleiden zeige ich es von den Sourcen.</p>
<pre><code>$ mkdir nagios-plugins
$ cd nagios-plugins
$ wget http://www.nagios-plugins.org/download/nagios-plugins-2.1.1.tar.gz
$ tar xf nagios-plugins-2.1.1.tar.gz
$ cd nagios-plugins-2.1.1/
$ ./configure
$ make
$ make install
</code></pre>
</li>
<li>
<p>Die Checks liegen nun unter <code>/usr/local/nagios/libexec/</code>. Hätte man die Plugins über APT installiert würden sie unter <code>/usr/lib/nagios/plugins/</code> liegen.</p>
</li>
<li>
<p>Machen wir einen schnellen Test:</p>
<pre><code>$ /usr/local/nagios/libexec/check_users -w 5 -c 10
</code></pre>
</li>
<li>
<p>Oder:</p>
<pre><code>$ /usr/lib/nagios/plugins/check_users -w 5 -c 10
</code></pre>
</li>
<li>
<p>Nun gehts an die Konfiguration des NRPE Daemons.</p>
<pre><code>$ vi /usr/local/nagios/etc/nrpe.cfg
nrpe_user=nagios
nrpe_group=nagios
allowed_hosts=&lt;IP des Überwachungsservers&gt;
command[check_users]=/usr/local/nagios/libexec/check_users -w 5 -c 10
command[check_load]=/usr/local/nagios/libexec/check_load -w 15,10,5 -c 30,25,20
command[check_disk]=/usr/local/nagios/libexec/check_disk -w 20% -c 10%
command[check_total_procs]=/usr/local/nagios/libexec/check_procs -w 150 -c 200
</code></pre>
</li>
<li>
<p>Jetzt kann der Daemon gestartet werden:</p>
<pre><code>$ /etc/init.d/nagios-nrpe-server start
</code></pre>
</li>
</ul>
<h5 id="einrichten-auf-dem-überwachungsserver-1">Einrichten auf dem Überwachungsserver</h5>
<ul>
<li>
<p>Nun probieren wir es vom Überwachungsserver aus wie Sie es schon kennen (gemäss “Manuelles Testen von Checks”):</p>
<pre><code>$ su - nagios -s /bin/sh
$ cd /tmp
$ /usr/lib/nagios/plugins/check_nrpe -H &lt;IP des Linux Servers&gt;
</code></pre>
</li>
<li>
<p>Die Ausgabe sollte folgendermassen aussehen:</p>
<pre><code>NRPE v2.15
</code></pre>
</li>
<li>
<p>Nun probieren wir es noch einmal mit einem Kommando:</p>
<pre><code>$ /usr/lib/nagios/plugins/check_nrpe -H &lt;IP des Linux Servers&gt; -c check_users
</code></pre>
</li>
<li>
<p>Die Ausgabe im Terminal (Bsp):</p>
<pre><code>USERS OK - 2 users currently logged in |users=2;5;10;0
</code></pre>
</li>
<li>
<p>Jetzt müssen wir noch Icinga konfigurieren</p>
</li>
<li>
<p>Wir definieren wieder eine Hostgroup für Linux Server, sodass wir später, wenn neue dazukommen, diese nur dieser Hostgroup zuordnen müssen. Dieser Hostgroup weisen wir gerade einen Service zu. Dazu die Datei <code>/etc/icinga/static/linux_nrpe.cfg</code> erstellen (evtl. noch erst den Ordner <code>/etc/icinga/static</code> weil den gibts noch nicht):</p>
<pre><code>$ vi /etc/icinga/static/linux_nrpe.cfg
define hostgroup{
        hostgroup_name          linux-nrpe-servers
        alias                   Linux NRPE Servers
}

define service{
        use                     generic-service
        hostgroup_name          linux-nrpe-servers
        service_description     Current Load by NRPE
        check_command           check_nrpe_1arg!check_load
}
</code></pre>
</li>
<li>
<p>Nun müssen wir noch den Host definieren und ihn der Hostgroup zuordnen. Neue Datei erstellen <code>/etc/icinga/objects/linuxsrv01.cfg</code>:</p>
<pre><code>$ vi /etc/icinga/objects/linuxsrv01.cfg
define host{
        use                     generic-host
        host_name               linuxsrv01
        hostgroups              linux-nrpe-servers
        alias                   Unser Linux Server
        address                 1.2.3.4
}
</code></pre>
</li>
<li>
<p>Icinga neu starten, bzw. Konfig neu laden:</p>
<pre><code>$ /etc/init.d/icinga reload
</code></pre>
</li>
<li>
<p>Nun sollte der Server im Webinterface erscheinen. Analog zur Windows Konfiguration fügen wir weitere sinnvolle Checks hinzu:</p>
<pre><code>$ vi /etc/icinga/static/linux_nrpe.cfg
define service{
        use                     generic-service
        hostgroup_name          linux-nrpe-servers
        service_description     Current Users by NRPE
        check_command           check_nrpe_1arg!check_users
}

define service{
        use                     generic-service
        hostgroup_name          linux-nrpe-servers
        service_description     Disk Space by NRPE
        check_command           check_nrpe_1arg!check_disk
}

define service{
        use                     generic-service
        hostgroup_name          linux-nrpe-servers
        service_description     Total Processes by NRPE
        check_command           check_nrpe_1arg!check_total_procs
}

define service{
        use                     generic-service
        hostgroup_name          linux-nrpe-servers
        service_description     Uptime by NRPE
        check_command           check_nrpe_1arg!check_uptime
}
</code></pre>
</li>
<li>
<p>Auf den Linux Server müssen die Checks in der <code>nrpe.cfg</code> definiert sein:</p>
<pre><code>$ vi /usr/local/nagios/etc/nrpe.cfg
command[check_users]=/usr/local/nagios/libexec/check_users -w 5 -c 10
command[check_load]=/usr/local/nagios/libexec/check_load -w 15,10,5 -c 30,25,20
command[check_disk]=/usr/local/nagios/libexec/check_disk -X proc -X sysfs -X tmpfs -X devtmpfs -w 20% -c 10%
command[check_total_procs]=/usr/local/nagios/libexec/check_procs -w 150 -c 200
command[check_apt_updates]=/usr/local/nagios/libexec/check_apt -t 300 -u
command[check_uptime]=/usr/local/nagios/libexec/check_uptime -u days -w 200 -c 365
</code></pre>
</li>
<li>
<p>Da wir hier eine Debian Maschine haben, machen wir noch einen spezifischen Check, der überprüft, ob über APT Updates verfügbar sind. Wir erstellen also eine Hostgroup (die Hostgroup <code>debian-servers</code> existiert schon) und weisen einen Check zu. Beachten Sie: die Änderung an der bestehenden Hostgroup <code>linux-nrpe-servers</code>.</p>
<pre><code>$ vi /etc/icinga/static/linux_nrpe.cfg
define hostgroup{
        hostgroup_name          linux-nrpe-servers
        alias                   Linux NRPE Servers
      	hostgroup_members	debian-nrpe-servers
}

define hostgroup{
        hostgroup_name          debian-nrpe-servers
        alias                   Debian NRPE Servers
}

define service{
        use                     generic-service
        hostgroup_name          debian-nrpe-servers
        service_description     APT Updates by NRPE
        check_command           check_nrpe_long!check_apt_updates
        check_interval          1440
        retry_interval          120
        notifications_enabled   0
}
</code></pre>
</li>
<li>
<p>Auch zeige ich hier wie Hostgroups verschachtelt werden können. Wenn also der Server der Hostgroup debian-nrpe-servers zugewiesen ist, soll er auch automatisch der Hostgroup linux-nrpe-servers zugewiesen sein.</p>
</li>
<li>
<p>Wir bedienen uns hier wieder dem <code>check_nrpe_long</code> Kommando, welches wir erstellt haben, mit einem Timeout von 300 Sekunden. Auch stellen wir den Intervall höher ein und die Notifications stellen wir ab, analog zum Windows Update Check weiter oben.</p>
</li>
<li>
<p>Der Host selbst muss nun in diese Hostgroup <code>debian-nrpe-servers</code> und NUR in diese. Wir passen seine Konfiguration an:</p>
<pre><code>$ vi /etc/icinga/objects/linuxsrv01.cfg
define host{
        use                     generic-host
        host_name               linuxsrv01
        hostgroups              debian-nrpe-servers
        alias                   Unser Linux Server
        address                 1.2.3.4
}
</code></pre>
</li>
<li>
<p>Der Host bekommt nun den Check “APT Updates by NRPE” und vererbt ausserden die Checks die wir schon definiert haben, weil die Hostgroup <code>debian-nrpe-servers</code> Member der Hostgroup <code>linux-nrpe-servers</code> ist.</p>
</li>
<li>
<p>Der NRPE Daemons (des zu checkenden Linux-Servers) muss auch noch um ein weiteres Kommando angepasst werden:</p>
<pre><code>$ vi /usr/local/nagios/etc/nrpe.cfg
command[check_apt_updates]=/usr/local/nagios/libexec/check_apt -t 300 -u
</code></pre>
</li>
<li>
<p>Neu starten; Überwachungsserver:</p>
<pre><code>$ /etc/init.d/icinga reload
</code></pre>
</li>
<li>
<p>Und Linux Server:</p>
<pre><code>$ /etc/init.d/nagios-nrpe-server restart
</code></pre>
</li>
</ul>
<h4 id="mit-ssh">Mit SSH</h4>
<ul>
<li>Nun sehen wir uns das Ganze statt mit NRPE mit SSH an.</li>
<li>Vorteile: fast keine Installation, Sicherheit durch SSHs asynchroner Authentifizierung (PKI)</li>
<li>Nachteile: Verbindungsaufbau, -abbau bei jedem Check, ein bisschen mehr Aufwand</li>
<li>Damit das funktioniert müssen wir eine Möglichkeit schaffen, sodass der nagios Benutzer des Überwachungsserver eine SSH-Verbindung auf dem zu überwachenden Linux Server aufbauen kann. Dies sollte ohne Passwortabfrage geschehen. Dem Check kann allerdings auch ein Benutzer und Passwort als Argument übergeben werden, aber SSH hat einen viel eleganteren und sicheren Mechanismus.</li>
</ul>
<h5 id="ssh---passwortloses-login">SSH - Passwortloses Login</h5>
<ul>
<li>
<p>Wir befinden uns auf dem Überwachungsserver und wechseln in einer Shell zum nagios Benutzer:</p>
<pre><code>$ su - nagios -s /bin/bash
</code></pre>
</li>
<li>
<p>Nun generieren wir einen SSH Schlüssel. Wir geben keine Passphrase ein (einfach Enter drücken):</p>
<pre><code>$ ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/var/lib/nagios/.ssh/id_rsa):
Created directory '/var/lib/nagios/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /var/lib/nagios/.ssh/id_rsa.
Your public key has been saved in /var/lib/nagios/.ssh/id_rsa.pub.
The key fingerprint is:
b2:ae:17:44:e8:4e:e1:5a:a0:d0:79:71:9c:b1:7e:71 nagios@monsrv01
The key's randomart image is:
+---[RSA 2048]----+
| . ..+oo         |
|. + +.+.         |
|.. = o. . E      |
|.   =..  o       |
|   = .o S        |
|  . . .+         |
|      ..         |
|     ..          |
|    .o.          |
+-----------------+
</code></pre>
</li>
<li>
<p>Drücken Sie einfach <code>&lt;Enter&gt;</code> bei den Fragen, denn wir wollen einfach die Default-Werte (die in den Klammern stehen).</p>
</li>
<li>
<p>Es wurden nun Keys generiert und im Homeverzeichnis des Benutzers unter <code>~/.ssh/</code> (Die Tilde <code>~</code> referenziert auf auf Home-Verzeichnis des Users) abgelegt.</p>
</li>
<li>
<p>Nun loggen wir auf die zu überwachende Linux Maschine ein.</p>
</li>
<li>
<p>Da erstellen wir einen Benutzer (mit Gruppe), der die Checks letztendlich ausführen soll (gleich wie oben, beachten sie die Shell):</p>
<pre><code>$ groupadd -r nagios
$ useradd -r -m -d /var/lib/nagios -s /bin/sh -g nagios nagios
</code></pre>
<ul>
<li><code>-r</code> erstellt einen Systembenutzer</li>
<li><code>-m</code> erstellt das Homeverzeichnis</li>
<li><code>-d</code> gibt das Homeverzeichnis an</li>
<li><code>-s</code> gibt die Shell an; <code>/bin/sh</code> -&gt; der Benutzer soll einloggen können</li>
<li><code>-g</code> gibt die Gruppe an</li>
</ul>
</li>
<li>
<p>Wir müssen dem Benutzer noch ein Passwort zuweisen:</p>
<pre><code>$ passwd nagios
</code></pre>
</li>
<li>
<p>Dann wechseln wir zu diesem Benutzer:</p>
<pre><code>$ su - nagios -s /bin/bash
</code></pre>
</li>
<li>
<p>Hier müssen wir den .ssh Ordner manuell erstellen (mit restriktiven Berechtigungen):</p>
<pre><code>$ mkdir -m 700 .ssh
</code></pre>
</li>
<li>
<p>Jetzt muss der Public Key vom Überwachungsserver-Benutzer (Ich nenne den nun nagios@monsrv01) in die Datei <code>.ssh/authorized_keys</code> des Benutzers des zu überwachenden Servers (Ich nenne den nun nagios@linuxsrv01) eingetragen werden. Dies tun wir manuell, als nagios@monsrv01:</p>
<pre><code>$ cat ~/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCpxUnKdQcy+XKYYSUpe4Z1nyBtUgHmcv1jxTvvZ4HgnPQZbqwV9EXVaPfd6/ACzWKTceVHDglYLaFAajjmEtIodAe0XHXCWPzXnMidZoEelZQo3j1ok9Lv+VgPqNSXhLbcPcwmOWjizzlABjjVlg5lu/doc7EQd46nPKe1CcL6bLapJd/Qdv3FqT3SCW3Bv/LmmX3l71Ff+xfrJFb8bCxvDyPiOgwhqHILNfYqeug+RcErcBcLu1vk3wsr3FrcJj8XfkoQqGgmCqR1PH2idgSLaMvCqGLdbvHkQiK7yPAvfCGpWPYhTslqGt7aEqdVAgAJqELUOVa7silx1qIvMo3/ nagios@monsrv01
</code></pre>
</li>
<li>
<p>Diesen Output kopiern wir (die ganze Zeile von “ssh-rsa” bis “nagios@monsrv01”). Nun als nagios@linuxsrv01 schreiben wir die Zeile in die Datei <code>.ssh/authorized_keys</code> (kann man natürlich auch mit einem Editor machen):</p>
<pre><code>$ echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCpxUnKdQcy+XKYYSUpe4Z1nyBtUgHmcv1jxTvvZ4HgnPQZbqwV9EXVaPfd6/ACzWKTceVHDglYLaFAajjmEtIodAe0XHXCWPzXnMidZoEelZQo3j1ok9Lv+VgPqNSXhLbcPcwmOWjizzlABjjVlg5lu/doc7EQd46nPKe1CcL6bLapJd/Qdv3FqT3SCW3Bv/LmmX3l71Ff+xfrJFb8bCxvDyPiOgwhqHILNfYqeug+RcErcBcLu1vk3wsr3FrcJj8XfkoQqGgmCqR1PH2idgSLaMvCqGLdbvHkQiK7yPAvfCGpWPYhTslqGt7aEqdVAgAJqELUOVa7silx1qIvMo3/ nagios@monsrv01" &gt;&gt; ~/.ssh/authorized_keys
$ chmod 640 ~/.ssh/authorized_keys
</code></pre>
</li>
<li>
<p>Nun sollte der SSH Login von nagios@monsrv01 zu nagios@linuxsrv01 passwortlos erfolgen; wir testen es als nagios@monsrv01 (die Frage mit dem Fingerprint bestätigen Sie mit “yes”, dies muss einmal gemacht werden, alle zukünftigen Logins werden ohne diese Frage erfolgen):</p>
<pre><code>$ ssh nagios@linuxsrv01
The authenticity of host 'linuxsrv01 (1.2.3.4)' can't be established.
ECDSA key fingerprint is 11:22:33:44:55:66:77:88:99:00:aa:bb:cc:dd:ee:ff.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'linuxsrv01,1.2.3.4' (ECDSA) to the list of known hosts.

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Fri Dec  4 13:21:16 2015 from monsrv01
$ 
</code></pre>
</li>
<li>
<p>Wir sind auf dem anderen Server! Test erfolgreich.</p>
</li>
</ul>
<h5 id="ssh---einen-check-testen">SSH - Einen Check testen</h5>
<ul>
<li>
<p>Nun ist es an der Zeit einen richten Check auf den Linux Server abzusetzen (gemäss “Manuelles Testen von Checks”):</p>
<pre><code>$ su - nagios
</code></pre>
</li>
<li>
<p>Die Shell mitanzugeben ist nun nicht mehr nötig, denn der User hat ja nun als Default Shell <code>/bin/sh</code> eingetragen.</p>
<pre><code>$ cd /tmp
$ /usr/lib/nagios/plugins/check_by_ssh -H &lt;IP des Linux Servers&gt; -C "/usr/local/nagios/libexec/check_load -w 15,10,5 -c 30,25,20"
OK - load average: 0.00, 0.01, 0.05|load1=0.000;15.000;30.000;0; load5=0.010;10.000;25.000;0; load15=0.050;5.000;20.000;0;
</code></pre>
</li>
<li>
<p>Wir können nun die Kommandos und Services definieren.</p>
</li>
</ul>
<h5 id="ssh---einrichten-auf-dem-überwachungsserver">SSH - Einrichten auf dem Überwachungsserver</h5>
<ul>
<li>
<p>Jetzt müssen wir noch Icinga konfigurieren</p>
</li>
<li>
<p>Wir definieren wieder eine Hostgroup für Linux Server, sodass wir später, wenn neue dazukommen, diese nur dieser Hostgroup zuordnen müssen. Dieser Hostgroup weisen wir gerade einen Service zu. Dazu die Datei <code>/etc/icinga/static/linux_ssh.cfg</code> erstellen (evtl. noch erst den Ordner <code>/etc/icinga/static</code> weil den gibts noch nicht):</p>
<pre><code>$ vi /etc/icinga/static/linux_ssh.cfg
define hostgroup{
        hostgroup_name          linux-ssh-servers
        alias                   Linux SSH Servers
        hostgroup_members       debian-ssh-servers
}

define hostgroup{
        hostgroup_name          debian-ssh-servers
        alias                   Debian SSH Servers
}

define service{
        use                     generic-service
        hostgroup_name          linux-ssh-servers
        service_description     Current Load by SSH
        check_command           check_load_by_ssh!15,10,5!30,25,20
}

define service{
        use                     generic-service
        hostgroup_name          linux-ssh-servers
        service_description     Current Users by SSH
        check_command           check_users_by_ssh!5!10
}

define service{
        use                     generic-service
        hostgroup_name          linux-ssh-servers
        service_description     Disk Space by SSH
        check_command           check_disk_by_ssh!20%!10%
}

define service{
        use                     generic-service
        hostgroup_name          linux-ssh-servers
        service_description     Total Processes by SSH
        check_command           check_total_procs_by_ssh!150!200
}

define service{
        use                     generic-service
        hostgroup_name          linux-ssh-servers
        service_description     Uptime by SSH
        check_command           check_uptime_by_ssh!200!365
}

define service{
        use                     generic-service
        hostgroup_name          debian-ssh-servers
        service_description     APT Updates by SSH
        check_command           check_apt_by_ssh
        check_interval          1440
        retry_interval          120
     	notifications_enabled   0
}
</code></pre>
</li>
<li>
<p>Nun die Kommandos:</p>
<pre><code>$ vi /etc/nagios-plugins/config/ssh.cfg

define command{
        command_name    check_load_by_ssh
        command_line    /usr/lib/nagios/plugins/check_by_ssh -H '$HOSTADDRESS$' -E -C "/usr/local/nagios/libexec/check_load -w '$ARG1$' -c '$ARG2$'"
}

define command{
        command_name    check_users_by_ssh
        command_line    /usr/lib/nagios/plugins/check_by_ssh -H '$HOSTADDRESS$' -E -C "/usr/local/nagios/libexec/check_users -w '$ARG1$' -c '$ARG2$'"
}

define command{
        command_name    check_disk_by_ssh
        command_line    /usr/lib/nagios/plugins/check_by_ssh -H '$HOSTADDRESS$' -E -C "/usr/local/nagios/libexec/check_disk -X proc -X sysfs -X tmpfs -X devtmpfs -w '$ARG1$' -c '$ARG2$'"
}

define command{
        command_name    check_total_procs_by_ssh
        command_line    /usr/lib/nagios/plugins/check_by_ssh -H '$HOSTADDRESS$' -E -C "/usr/local/nagios/libexec/check_procs -w '$ARG1$' -c '$ARG2$'"
}

define command{
        command_name    check_uptime_by_ssh
        command_line    /usr/lib/nagios/plugins/check_by_ssh -H '$HOSTADDRESS$' -E -C "/usr/local/nagios/libexec/check_uptime -u days -w '$ARG1$' -c '$ARG2$'"
}

define command{
        command_name    check_apt_by_ssh
        command_line    /usr/lib/nagios/plugins/check_by_ssh -H '$HOSTADDRESS$' -E -C "/usr/local/nagios/libexec/check_apt -t 300 -u"
}
</code></pre>
</li>
<li>
<p>Und noch den Host der Hostgroup zuweisen:</p>
<pre><code>$ vi /etc/icinga/objects/linuxsrv01.cfg
define host{
        use                     generic-host
        host_name               linuxsrv01
        hostgroups              debian-nrpe-servers,debian-ssh-servers
        alias                   Unser Linux Server
        address                 192.168.140.79
}
</code></pre>
</li>
<li>
<p>Schlussendlich ist der Host natürlich nur einer dieser beiden Hostgroups zugewiesen, nicht beiden. Dies ist nur aus Demonstrationszwecken. Sie sehen beide Arten haben das gleiche Ergebnis, kommen aber anders ran.</p>
</li>
<li>
<p>Icinga neu starten</p>
<pre><code>$ /etc/init.d/icinga reload
</code></pre>
</li>
</ul>
<h3 id="servicesports-von-aussen-überwachen">Services/Ports von aussen überwachen</h3>
<p>Wir wolle hier nun eine Art von Checks besprechen, die nicht abhängig von einem Betriebssystem sind. Wir wollen dementsprechend von aussen überprüfen, ob ein Service funktioniert und auf Anfragen antwortet. Als Beispiel könnte zu Anfang ein Webserver dienen. Nehmen wir an, der Server und darauf der Dienst oder Daemon läuft zu unserer Zufriedenheit (dies überprüfen wir ja bisher), aber dennoch können wir noch nicht garantieren, ob der Server auch Webseiten zur Verfügung stellt. Um dies quasi auf der Enduser-Seite zu prüfen machen wir einen Check:</p>
<h4 id="http">HTTP</h4>
<ul>
<li>
<p>Erst definieren wir eine Hostgruppe in der neuen Datei <code>/etc/icinga/static/web_servers.cfg</code> mit dem Inhalt:</p>
<pre><code>$ vi /etc/icinga/static/web_servers.cfg
define hostgroup{
        hostgroup_name          web-servers
        alias                   Web Servers
}
</code></pre>
</li>
<li>
<p>Einen unserer Hosts müssen wir nun natürlich dieser Hostgruppe zuweisen.</p>
</li>
<li>
<p>Für das Kommando sind in der Datei <code>/etc/nagios-plugins/config/http.cfg</code> schon etliche vordefiniert. Wir müssen uns also nur noch den richtigen aussuchen und der Hostgruppe zuweisen; wir nehmen vorerst den ganz simplen:</p>
<pre><code>$ vi /etc/icinga/static/web_servers.cfg
define service{
        use                     generic-service
        hostgroup_name          web-servers
        service_description     HTTP Service
        check_command           check_http
}
</code></pre>
</li>
</ul>
<p>Ein weiterer Ansatz wäre den Port des Services zu überwachen. Also, ob der Server überhaupt Verbindungen auf diesem Port zulässt. Somit können schon einige Fehler ausgeschlossen werden. In unseren Fall, wäre das der Port 80:</p>
<ul>
<li>
<p>Wir erweitern die Hostgruppe <code>web-servers</code> um einen Service:</p>
<pre><code>$ vi /etc/icinga/static/web_servers.cfg
define service{
        use                     generic-service
        hostgroup_name          web-servers
        service_description     HTTP Port
        check_command           check_tcp!80
}
</code></pre>
</li>
<li>
<p>Icinga neu laden:</p>
<pre><code>$ /etc/init.d/icinga reload
</code></pre>
</li>
</ul>
<p>Je genauer Sie einen Service überwachen, desto präziser lässt sich das Problem eingrenzen, im Falle eines Ausfalls. Sie müsen sich allerdings auch überlegen, ob Ihnen nicht besser gedient ist, wenn Sie lediglich den Service von aussen überwachen und nicht alles mögliche (Prozesse/Dienste/Ports/Dateien,usw…).</p>
<h4 id="dns">DNS</h4>
<p>DNS ist auch ein Dienst von dem viel abhängen kann. So könnte man Ihn auf seine Funktionalität hin überprüfen:</p>
<ul>
<li>
<p>Hostgruppe mit Service erstellen in der neuen Datei: <code>/etc/icinga/static/dns_servers.cfg</code></p>
<pre><code>$ vi /etc/icinga/static/dns_servers.cfg
define hostgroup{
        hostgroup_name          dns-servers
        alias                   DNS Servers
}

define service{
        use                     generic-service
        hostgroup_name          dns-servers
        service_description     DNS Service
        check_command           check_dns
}
</code></pre>
</li>
</ul>
<h4 id="dhcp">DHCP</h4>
<ul>
<li>
<p>Hostgruppe mit Service erstellen in der neuen Datei: <code>/etc/icinga/static/dhcp_servers.cfg</code></p>
<pre><code>$ vi /etc/icinga/static/dhcp_servers.cfg
define hostgroup{
        hostgroup_name          dhcp-servers
        alias                   DHCP Servers
}

define service{
        use                     generic-service
        hostgroup_name          dhcp-servers
        service_description     DHCP Service
        check_command           check_dhcp
}
</code></pre>
</li>
</ul>
<h3 id="einbinden-eines-switches">Einbinden eines Switches</h3>
<p>Beachten Sie, um einen Switch korrekt zu überwachen muss er managebar sein und er sollte SNMP unterstützen. Wir überwachen den Switch mit SNMP v3, dass wesentlich bessere Sicherheitsmechanismen als die Versionen 1 und 2 bietet. Der Switch muss selbstverständlich dementsprechend konfiguriert sein. Dies ist bei jedem Swirch und Hersteller anders. Folgende Einstellungen sollten gesetzt sein:</p>
<ul>
<li>SNMP v3 aktivieren</li>
<li>Access Mode: Read Only</li>
<li>Username: zB: monitor</li>
<li>Authentication Protocol: MD5</li>
<li>Authentication Password: Passwort</li>
<li>Privacy/Encryption Protocol: DES</li>
<li>Privacy/Encryption Password: Passwort</li>
</ul>
<p>Nun sollten wir auf dem Überwachungsserver einen Test machen; wir lesen die Uptime des Switches per SNMP aus:</p>
<ul>
<li>
<p>SNMP v1/2:</p>
<pre><code>$ /usr/lib/nagios/plugins/check_snmp -H &lt;IP des Switches&gt; -m RFC1213-MIB -o iso.3.6.1.2.1.1.3.0
SNMP OK - Timeticks: (1228980700) 142 days, 5:50:07.00 |
</code></pre>
</li>
<li>
<p>SNMP v3:</p>
<pre><code>$ /usr/lib/nagios/plugins/check_snmp -H &lt;IP des Switches&gt; -P 3 -L authPriv -a MD5 -x DES -X "Passwort" -U monitor -A "Passwort" -m RFC1213-MIB -o iso.3.6.1.2.1.1.3.0
SNMP OK - Timeticks: (1228980700) 142 days, 5:50:07.00 |
</code></pre>
</li>
</ul>
<p>Diese Passwörter und Benutzernamen sollten wir nun in die Icingakonfiguration einbauen. Dazu ist die Datei <code>/etc/icinga/resource.cfg</code> da:</p>
<pre><code>$ vi /etc/icinga/resource.cfg

# SNMP v3 Username und Authentication Password und Privacy Password:
$USER3$=monitor
$USER4$=encryption_passwort
$USER5$=priv_passwort
</code></pre>
<ul>
<li>
<p>Dies sind sogenannte Markos, siehe <a href="http://docs.icinga.org/latest/de/macrolist.html#macrolist-user">http://docs.icinga.org/latest/de/macrolist.html#macrolist-user</a>. Es können bis zu 256 solche Variablen definiert werden.</p>
</li>
<li>
<p>Anpassungen Rechte:</p>
<pre><code>$ chown nagios:www-data /etc/icinga/resource.cfg
$ chmod 440 /etc/icinga/resource.cfg
</code></pre>
</li>
</ul>
<p>Nun müssen wir ein Kommando schreiben, dass diese Makros einsetzt:</p>
<pre><code>$ vi /etc/nagios-plugins/config/snmp.cfg
define command{
        command_name    check_snmp_v3_uptime
        command_line    /usr/lib/nagios/plugins/check_snmp -H '$HOSTADDRESS$' -P 3 -L authPriv -a MD5 -x DES -X '$USER5$' -U '$USER3$' -A '$USER4$' -m RFC1213-MIB -o iso.3.6.1.2.1.1.3.0
}
</code></pre>
<p>Jetzt eine Hostgroup, die einen Service zugewiesen hat, der das Kommando einsetzt:</p>
<pre><code>$ vi /etc/icinga/static/switches.cfg
define hostgroup{
        hostgroup_name          switches
        alias                   Switches
}

define service{
        use                     generic-service
        hostgroup_name          switches
        service_description     Uptime by SNMP v3
        check_command           check_snmp_v3_uptime
}
</code></pre>
<p>Und zu guter Letzt, einen Host, der in der Hostgroup switches ist:</p>
<pre><code>$ vi /etc/icinga/objects/switch01.cfg
define host{
        use                     generic-host
        host_name               switch01
        hostgroups              switches
        alias                   Unser Switch
        address                 1.2.3.5
}
</code></pre>
<p>Icinga neu laden:</p>
<pre><code>$ /etc/init.d/icinga reload
</code></pre>
<p>Wir wollen nun die einzelnen Ports überwachen. Auch wollen wir Graphen mit dem Traffic haben. Dazu brauchen wir ein Perl Skript, dass uns diese Werte ausliest und aufbereitet.</p>
<ul>
<li>
<p>Installeren von Perl Nagios, DNS und DES plugins:</p>
<pre><code>$ apt-get install libnagios-plugin-perl libnet-dns-perl libcrypt-des-perl
</code></pre>
</li>
<li>
<p>Installation von git:</p>
<pre><code>$ apt-get install git
</code></pre>
</li>
<li>
<p>Klonen des git-Repos:</p>
<pre><code>$ git clone https://github.com/chaoos/nagios-check-iftraffic.git
</code></pre>
</li>
<li>
<p>Kopieren vom Skript:</p>
<pre><code>$ cp nagios-check-iftraffic/check_iftraffic.pl /usr/lib/nagios/plugins/
$ chmod 755 /usr/lib/nagios/plugins/check_iftraffic.pl
</code></pre>
</li>
</ul>
<p>Nun haben wir das neue Skript installiert. Testen wir es:</p>
<pre><code>$ su - nagios -s /bin/bash
$ cd /tmp
$ /usr/lib/nagios/plugins/check_iftraffic.pl -H &lt;IP des Switches&gt; -P 3 -U "monitor" -y MD5 -Y "Passwort" -x DES -X "Passwort" --total -i &lt;Switchport&gt;
IFTRAFFIC OK - Average IN: 675.93Bs (0.00%), Average OUT: 1.73KBs (0.00%) Total RX: 33.08GBytes, Total TX: 163.75GBytes | inUsage=0.00%;85;98 outUsage=0.00%;85;98 inBandwidth=675.93Bs;; outBandwidth=1726.63Bs;; inAbsolut=35516241444;; outAbsolut=175821214498;;
</code></pre>
<ul>
<li>
<p>Falls ein Port down ist, würde es so aussehen:</p>
<pre><code>$ /usr/lib/nagios/plugins/check_iftraffic.pl -H &lt;IP des Switches&gt; -P 3 -U "monitor" -y MD5 -Y "Passwort" -x DES -X "Passwort" --total -i 14
IFTRAFFIC CRITICAL - SNMP error: Interface 14 is down!
</code></pre>
</li>
</ul>
<p>Definition des Kommandos:</p>
<pre><code>$ vi /etc/nagios-plugins/config/snmp.cfg
define command{
        command_name    check_iftraffic_v3
        command_line    /usr/lib/nagios/plugins/check_iftraffic.pl -H '$HOSTADDRESS$' -P 3 -U '$USER3$' -y MD5 -Y '$USER4$' -x DES -X '$USER5$' -i '$ARG1$'
}
</code></pre>
<p>Wir weisen nun unserem Switch zu, den Port 24 zu überprüfen. Wir weisen es direkt dem Switch zu, nicht der Hostgroup, denn welche Ports überwacht werden sollen, ist bei jedem Switch anders:</p>
<pre><code>$ vi /etc/icinga/objects/switch01.cfg
define service{
        use                     generic-service
        host_name               switch01
        service_description     Port 24
        check_command           check_iftraffic_v3!24
}
</code></pre>
<ul>
<li>
<p>Ein Wort zu Nagios/Icinga &amp; Perl Skripts: bei Plugins, die in Perl geschrieben sind, wie beispielsweise <code>check_iftraffic.pl</code>, müssen Sie darauf achten, dass Icinga einen integrierten Perl Interpreter mitbringt. Das Kompliat des Debian Packets von Icinga wurde mit dieser Option kompiliert. Das heisst, dass standardmässig Perl Skripts via Icinga mit diesem Interpreter (ePN, embedded Perl Nagios) ausgeführt werden. Allerdings läuft nicht jedes Skript mit diesem Interpreter. Unseres wollen wir mit dem herkömmlichen Perl Interpreter (<code>/usr/bin/perl</code>) ausführen lassen. Sehen Sie sich die Zeile 3 des Skripts an:</p>
<pre><code>$ vi /usr/lib/nagios/plugins/check_iftraffic.pl
# nagios: -epn
</code></pre>
</li>
<li>
<p><code>-epn</code> bedeutet, dass Icinga nicht den ePN verwenden soll. Analog dazu würde <code># nagios: +epn</code> das Verwenden vom integrierten Interpreter forcieren. Dieser Marker muss in den ersten 10 Zeilen des Perl Skripts erscheinen.</p>
</li>
</ul>
<p>Icinga neu laden:</p>
<pre><code>$ /etc/init.d/icinga reload
</code></pre>
<h2 id="troubleshooting">Troubleshooting</h2>
<p>@Tom: Hier sollte man alles direkt zeigen, nicht nur erzählen:</p>
<p>Nicht immer klappt alles auf Anhieb. Wenn Checks sich nicht verhalten wie man erwartet, sollte man diese manuell ausführen -&gt; siehe “Manuelles Testen von Checks”.</p>
<ol>
<li>
<p>Check forcieren:<br>
Da man nicht immer 5 Minuten warten will, wenn man etwas verändert hat, kann ein Check auch über das Webinterface forciert werden: in der “Service State Information” -&gt; “Re-schedule the next check of this service”, Häkchen auf “Force Check”, dann “Commit”. Dies plant eine erzwungene aktive Prüfung zu einer gegebenen Uhrzeit. Diese Zeit können Sie selbst definieren; normalerweise stellt das Webinterface diese Uhrzeit auf die aktuelle Uhrzeit. “Force Check” bedeutet, dass der Check ausgeführt wird, unabhängig davon, ob Checks global oder für den Host ein- oder ausgeschalten sind.<br>
@Tom: Hier wäre es gut, wenn du das zeigst und die Uhrzeit auf +5 Minuten stellst. Dann in der Rubrik “Scheduling Queue” zeigst wie der Check in die Queue eingereiht wird, mit dem Typ “Forced”.</p>
</li>
<li>
<p>Genaues Kommando herausfinden:<br>
Falls Sie wissen wollen welches Kommando Icinga bei einem Service <em>genau</em> ausführt, weil Sie sich nicht sicher sind, gehen Sie in der “Service State Information” des Services auf “Command Expander”. Da sehen Sie, wie aus der Kommando Definition mit dem Markos Schritt für Schritt das eigentliche Shell Kommando wird. Diese kann man kopieren und so in einer Shell via “Manuelles Testen von Checks” ausführen. Achtung: bei Perl Skripts stimmt das nicht immer, siehe oben.</p>
</li>
<li>
<p>Icinga startet nicht/ladet Konfiguration nicht neu (<code>/etc/init.d/icinga restart</code> oder <code>/etc/init.d/icinga reload</code> schlagen fehl):<br>
Das kann an Vielem liegen. Aber in erster Linie, wird vermutlich die Konfiguration Fehler enthalten: falsche Syntax, fehlende Definitionen/Hosts, usw… Starten Sie Icinga manuell aus einem Terminal mit <code>-v</code>. Dies macht eine Prüfung der gesamten Konfiguration:</p>
<p>$ icinga -v /etc/icinga/icinga.cfg</p>
</li>
</ol>
<p>In unserem Setting sehen wir nun 3 Warnungen, die aber nicht relevant sind. Wir beheben sie trotzdem. Die Warnungen:</p>
<pre><code>[...]
Warning: Variable 'normal_check_interval' with value '5' is DEPRECATED. Replace it with 'check_interval'.
Warning: Variable 'retry_check_interval' with value '1' is DEPRECATED. Replace it with 'retry_interval'.
[...]
Warning: Object definition type 'hostextinfo' is DEPRECATED in file '/etc/icinga/objects/extinfo_icinga.cfg' on line 5.
[...]
</code></pre>
<p>Icinga warnt uns hier, dass 3 Werte, die wir in der Konfiguration nutzen, veraltet sind. Wir ersetzen sie mit den Neuen (in <code>/etc/icinga/objects/generic-service_icinga.cfg</code>: Zeilen 19 und 20):</p>
<pre><code>$ vi /etc/icinga/objects/generic-service_icinga.cfg
                check_interval                  5
                retry_interval                  1
</code></pre>
<p>Was die 3. Warnung angeht, diese belassen wir. Sie stört uns nicht. Es handelt sich um die hostext-Definition des localhost Hosts. Alle diese Werte können auch direkt in der Host-Definition gesetzt werden.</p>
<ol start="4">
<li>evtl. mehr</li>
</ol>
<h2 id="status-map--was-sie-bedeutet">Status Map &amp; was sie bedeutet</h2>
<p>@Tom: Hier solltest du die Status Map (im Webinterface unter Status) zeigen, wie sie aussieht, wenn wir noch nichts konfiguriert haben (also jetzt).</p>
<ul>
<li>
<p>Nun konfigurieren wir die 4 Hosts (localhost, winsrv01, linuxsrv01 &amp; switch01), sodass alle Childhosts des Switches switch01 sind (Wir gehen davon aus, dass alle über diesen Switch verkabelt sind).</p>
<ul>
<li>
<p>In die Hostdefinition von localhost, winsrv01 &amp; linuxsrv01 fügen wir folgende Zeile hinzu:</p>
<pre><code>parents                 switch01
</code></pre>
</li>
</ul>
</li>
<li>
<p>Konfiguration neu laden:</p>
<pre><code>$ /etc/init.d/icinga reload
</code></pre>
</li>
</ul>
<p>@Tom: Hier die veränderte Status Map wieder zeigen. Nun sieht man, dass die 3 Hosts am switch01 hängen. Auch erwähnenswert ist, dass der “Icinga Process” immer in der Mitte ist, weil von ihm aus überprüft wird. Wie das Weltall immer nur von der Erde aus beobachtet werden kann, können Systeme immer nur von Icinga Process aus beobachtet werden. Allerdings ist der Host, auf dem Icinga läuft nicht zwingend in der Mitte, denn er muss ja auch an einem Switch hängen, und somit einen Parent Host haben.</p>
<ul>
<li>Man sollte sich überlegen, welcher Host “ganz oben” im Stammbaum ist (der sogenannte “root host”), der selbst keinen Parent mehr hat. Idealerweise nimmt man dafür einen Router oder eine Firewall, die direkt am Internet hängt. In unserem Beispiel ist dies der Switch switch01.</li>
</ul>
<h2 id="downtimes--comments">Downtimes &amp; Comments</h2>
<p>Nehmen wir an, an einem Host müssen Wartungsarbeiten getätigt werden und dieser Host wird für einige Stunden nicht verfügbar sein. Seine Services werden CRITICAL werden und eine Flut an Notifications wird produziert. Um das zu verhindern definieren wir eine Downtime für diesen Host und seine Services. In der “Host Information” des Hosts unter “Schedule downtime for this host and all services” können wir eine solche definieren.</p>
<ul>
<li>Als Kommentar empfiehlt sich der Grund, warum der Host nicht erreichbar sein wird, dies ist vor Allem interessant in grösseren Umgebungen mit mehreren Administratoren.</li>
<li>Start &amp; End time dürfte selbsterklärend sein</li>
<li>Type: Art des Ausfalls
<ul>
<li>Fixed: Ausfallzeit zwischen definierter Start und End time.</li>
<li>Flexible: Sie wissen nicht genau wann zwischen definierter Start und End time der Host/Service ausfallen wird, aber sobald er ausfällt, wird er für zB 2 Stunden weg sein. Darum braucht Flexible noch eine “Flexible Duration”.</li>
</ul>
</li>
</ul>
<h3 id="comments">Comments</h3>
<p>Der Kommentar der Downtime wird direkt an den Host und die Services “angeheftet”. Ersichtlich ist er durch ein Sprechblasensymbol in der Host oder Service Detail Liste oder direkt unter Comments.</p>
<p>Kommentare können auch ohne Downtime an einen Host oder Service angeheftet werden. Beispielsweise wenn ein Service ein Problem hat und ein Administrator sich dem Problem annimmt, kann er im “Service Information” Bildschirm unter “Service Commands” -&gt; “Acknowledge this service problem” auswählen. Solche Acknowledgements können auch per Mail versendet werden.</p>
<p>@Tom: Hier ist die Anleitung fertig. Das weiter unten wären Ideen für ein “forgeschrittenes” Video.</p>
<hr>
<h3 id="nrpe-sicher-machen-mit-ssl-und-zertifikaten">NRPE sicher machen mit SSL und Zertifikaten</h3>
<p>Auf dem Icinga-Server:</p>
<pre><code>$ apt-get install libssl-dev gcc make
$ git clone -b nrpe-2-16-RC2 https://github.com/NagiosEnterprises/nrpe.git
$ ./configure --enable-ssl --with-ssl=/usr/bin/openssl --with-ssl-lib=/usr/lib/x86_64-linux-gnu
$ make check_nrpe
$ apt-get install git build-essential cmake python python-dev libssl-dev libboost-all-dev protobuf-compiler python-protobuf libprotobuf-dev python-sphinx libcrypto++-dev libcrypto++ liblua5.1-0-dev libgtest-dev
</code></pre>
<hr>
<h2 id="überwachung-von-systemen-1">Überwachung von Systemen</h2>
<ul>
<li>Einbinden eines Windows Systems</li>
<li>Netzwerkauslastung</li>
<li>Einbinden eines Linux Systems</li>
<li>Netzwerkauslastung</li>
<li>Einbinden eines Routers</li>
<li>Services/Ports von aussen überwachen -&gt; ssh, ftp, HTTPS-Zertifikat Gültigkeit, usw… (Bei allen OSes gleich)</li>
<li>SNMP Geschichten</li>
<li>Systeme mit mehreren IPs</li>
<li>Microsoft Exchange Server</li>
</ul>
<h2 id="spezielle-sachen">Spezielle Sachen</h2>
<ul>
<li>HP Server Hardware Raid überwachen</li>
<li>Eigenen Check schreiben (<a href="https://nagios-plugins.org/doc/guidelines.html">https://nagios-plugins.org/doc/guidelines.html</a>)</li>
<li>Synology NAS ?</li>
<li>APC UPS ?</li>
<li>MS Exchange ?</li>
<li>ESXi ?</li>
</ul>

