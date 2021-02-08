# Optimierung mit KI Techniken
## Prolog
- Logische Programmiersprache
- Unifikation (Variablenbindungs-Algorithmus)
- Parameter können sowohl Eingabe-, als auch Ausgabeparameter sein = Unifikation?
- Single-Assignment-Variable: Variablenwert ist nicht überschreibbar
	- Zum ändern also Einführung von neuem Variablenname
		- Notwendigkeit Einführung neuer Variablen fürht i.Allg. nicht zu Erhöhung des Speicherverbrauchs (alte VariablenNamen werden im rekursiven Auffruf i.allg. nicht mehr benötit)
	- Variablen sind im mathematischen Sinne zu verstehen, nicht im programmiertechnischen Sinn:
		- Platzhalter für noch incht bekannte Werte
	- Großer anfangsbuchstabe oder mit _ beginnend 
- Suchverfahren mit Entscheidungspunkt-Setzen und Backtracking
	- Backtracking: 
		- Zurückgehen zum zuletzt angelegten Entscheidungsunkt und mit dort aufgeführter nächster Klausel alternative Abarbeitung vornehmen. 
			- Zwischenzeitlich erfolgte Belegungen von Variablen werden zurükgenommen
		- Bei Aufruf einer Prozedur wird Entscheidungspunkt angelegt, wenn nachfolgend weitere Klauseln der Prozedur existieren
		- Prozeduraufruf ist logisch gesehen eine Bedingung. Wenn erfolgreich kann nächste Bedingung folgen. Scheitern -> Backtracking
- ERROR führt zu Abbruch der gesamten Verarbeitung (im Gegensatz zu Scheitern)
- Definitionsbereiche
	- Variable mit Definitionsbereich definieren:
		- X hat Wert in Intervall 3,6: `X::3..6` oder `X::[3..6]`
			- benötigt `lib(gfd).`
		- Y hat definitionsbereich 3,3: `Y::3`

- Datentypen / Datenstrukturen: 
	- Numbers (Integers, Floats, rationals, bounded reals) 
	- Strings: Character sequences in double quotes
		- append Str1 with Str2 and save in Str3:
			```
				join_str(Str1, Str2, Str3) :-
					name(Str1, StrAsciList1), % Save Str1 in StrAsciList1 als List of ASCI characters
					name(Str2, StrAsciList2),
					append(StrList, StrList2, StrList3),
					name(Str3, StrList). % Save Asci list as string in Str3
			```
	- Lists: 
		- empty list: `[]`
		- head and tail: `[Head|Tail]`
			- Head a single element
			- Tail a (possibly empty) list  
			- Equivalent lists: `[1,2,3]`, `[1|[2,3]]`, `[1|[2|[3]]]`, `[1|[2|[3|[]]]]`
		- Listen-operatoren
			- `append(L1, L2, ResL)`: Elemente aus L1 und L2 kombinieren und in ResL speichern
			- `reverse(L, ResL)`: L invertieren und in ResL speichern
			- `member(a, L)`: Überprüfen ob a in L enthalten ist
				- `member(A, L)`: einen member von L in A speichern.
			- length/2(list, N)
			- member/2(X, List) --> checks if X is member of List
			- `[Head|Tail] = L`: In Head das 1. Element von L speichern, in Tail die Restliste.
				- `[Head1, Head2, Head3 | Tail]` : Mehrere Werte extrahieren
				- `[_,[Head|Tail], B|T] = [a, [2,3,c,d], 4,5]`: Auf liste innerhalb von liste zugreifen. (_=a, Head=2, Tail=[3,c,d], B=4, T=[5])
			- Cycle through list: 
				```prolog
					write_list([]).
					write_list([Head|Tail]) :-
						write(Head), nl % nl: new line
						write_list(Tail). % Rekursiver aufruf rest schreiben, abbruch bei []
				```
			- last/2 TODO
			- `maxlist(List, Max)`
				- Maximaler Wert von List wird Max zugeordnet
				- Oder Constraint für liste, dass max Wert Max ist
			- minlist(List, Min)
			- subscript(+Term, ++Subscript, -Elem)
				- https://www.eclipseclp.org/docs/7.0/bips/kernel/termmanip/subscript-3.html
				- Subscript ist für zweidimensionale Matrix eine Liste in Form [Zeilenindex, Spaltenindex], wobei Indizes bei 1 beginnt (nicht wie bei Gecode bei (0)
			- `delete/3(Element, L, Lres)`: Ein Element Löschen 
				- `?-delete(a,[b, a, c, a], L).` => L = [b,c,a]  
			- `select/3(Element, L, Lres)`: Mehrere Elemente Löschen.	
				- `?-select(a,[b, a, c, a], L).` =>  L = [b,c] 
			- `union/3(L1, L2, Lres)`: Liste L1 und L2 Vereinigen (Mengenvereinigung)
				- `?-union([1,2,3],[1,3],L)` =>  L = [2,1,3]     
			- `subtract/3(L1,L2, Lres)`: Mengendifferenz
				- `?-subtract([a,b,c], [b], L).` => L = [a,c]  
			- `intersection/3(L1,L2,Lres)`: Mengendurchschnitt
				- `?-intersection([a,b,c], [b,d], L).` => L = [b]  
			- `sort/2(L, Lres)`: Aufsteigend sortieren

		- Arrays:
			- Initialisierung: `[](2,3,1,5)`
				- 3x2 Matrix:  `[]([](4,2), [](1,6), [](3,1)))`
				- Erzeugung aus/Konvertierung Liste: 
					- `array_list([](6, 1,3), List).` => `List = [6, 3,1]`
					- `array_list([]([](4,2), [](1,6), [](3,1)), Liste).` => `Liste = [[](4, 2), [](1, 6), [](3, 1)]`
						- Nur oberstes Array-Niveau wird in Liste transformiert
					- `array_list(Array, [[1,2],[3,4],[5,6],[7,8]]).` => `Array = []([1, 2], [3, 4], [5, 6], [7, 8])`
						- Nur oberstes Listenniveau wird in Array transformiert
		(- Atoms: Symbolic constatns usually lower case or in singe quotes)
		(- Structures: )
		- List processing:
			- sort/2(List, SortedList)
- Domänen zu Constraint-Vaiablen Zuordnen
	- `?Vars :: ++Domäne` 
	- X :: 1
	- X :: 1..10
	- X :: [1, 2, 3, 4]
	- X :: [1..10, 12, 15..20]
	- [X,Y,Z]:: 10..20
	- [X,[Y,Z]]::1..10

- Bedeutung der Modus-Deklarationen (mode declarations)
	- `?`:  Der Modus ist unbekannt  oder er  ist keiner der folgenden Modi. Im Allg. bedeutet das, das Argument kann variabel sein
	- `+`:  Das Argument ist keine oder eine belegte Variable
	- `++`: Das Argument ist eine Konstante
	- `-`: Das Argument ist eine unbelegte Variable, die in keinem anderen Argument des Aufrufs vorkommt
- Predicates:
	- Has a truth value
	- predicate definition definaes what is true
	- predicate invocation or call checks its truth value
	- Defines a relationship between its arguments
- Goal: Logical formula whose truth value we want to know. Can be a conjunction or disjunction of other (sub-)goals.
- QueryL Initial goal given to a computation
- Unification: 
- Conjunction (AND): Built using commas. Only ture if all conjuncts are true
	`?- integer(5), integer(7), integer(hello).` --> No
- Disjunctions (OR): Built using semicolons. True if at least one disjunct is true
	`?- (integer(5); integer(7); integer(hello)).` --> Yes

- If-Else
	- mit implikation
		```prolog
		fakultaet(N, F):-
			N > 0
				-> N1 is N-1, % Implikation, daher wird die nachfolgende Konjunktion (Die 3 mit Komma verknüpften Goals) ausgeführt, wenn N > 0. 
				fakultaet(N1, Z),
				F is N * Z
				; F = 1. % Else Fall. 
		```
	- Ein Body pro Fall: (Klauseln zur Darstellung verschiedener Fälle )
		```prolog
		fakultaet(N, F):- % Schlägt Fehl, wenn N nicht größer 0. Daher wird dann mit der nächste Body ausprobiert (Backtracking, dann Ausführung nächster passender Klausel)
			N > 0,
			N1 is N - 1,
			fakutaet(N1, Z),
			F is N * Z.
		fakultaet(0,1). % Else Fall
		```
- Prozedduren TODO
	- Aufrufe von Prozeduren 

- Coments:
	- Line comments: `%`
	- Block comments; `/* ... */`
- Prolog program: collection of predicates. Predicate: collection of clauses
	- Clause:
		- defines that something is true
		- simplest form: fact: structure or atom terminated by `.`. e.g.: `capital(london, england).
		- Form of a clause:  `Head :- Body`
- Loops: 
	- foreach(X, List) do writelln(X)

- Libraries: 
	- Finite Domain (fd):
		- `#` vor arithmetischen Verlgeichsoperationen markiert eine fd-constraint-relation. z.b. `#\=`
		- für Constraints über endlichen Domänen
		- Häufig benötigte Zusatzbibliotheken: fd_global (globale C.) , fd_search (Suchmeth.). 
		- Solver ist nicht mit ic-Solver verträgl. -> libs nicht gemeinsam laden.

	- Real Interveral Arithmetic (ic)L
		- erweiterte Menge an BIPs für Constraints über endlichen Domänen und reellen Zahlen (real interval arithmetic)
		- Häufig benötigte Zusatzbibliotheken: ic_global, ic_cumulative, ic_edge_finder(3)(
		
	- gfd aus fd-und ic-lib zusammengeführter, erweiterter und neu programmierter Solver.
		- Benötigt (im Rahmen der LV) i. Allg. keine Zusatzbibliotheken. 
		- Erforderlichenfalls scheint fd_global kompatibel mit gfd zu sein. 
		- Aber notwendig:   :-lib(branch_and_bound)

- BIPs zur Programmsteuerung
	- `true/0` : Wahr füë leere Anweisung
	- `fail/0`
	- `!/0`: (cut) Beeinflyussung des Backtrackings durch Löschen von Entscheidungspunkten
	- `call(A)`: z.B. `read(X),call(X).` Abarbeitung eines Aufrufs A, der zum Progr.zeitpunkt noch unbekannt sein kann. Aufrufe können in in Prolog Funktor-oder Operator-Ausdrücke,  aber  niemals  eine Unbekannte  (Variable), auch wenn diese als Wert einen Namen hat,  sein. 
	- `(Bedingung -> Then ; Else)`: If-Else
	- `(count(Index,  Start, Ende) do Anw1, ...)`: Laufanweisung. 
		- `?-(count(N,1,3) do write(N)).` => 123   
	- `(foreach(Index,  [Start,Ende] do Anw21,...)`: Maplist
		- `?-(foreach(N,[1,3])do write(N)).` => 13
- Bip’s zur Fehleranalyse
	- `trace/0`: Trace ein
		-  z.B. zum Trace der Fakultätsprozedur:  `?-trace.<ENTER>   ?-fakultaet(5,X).`
	- `notrace/0`: Trace aus
- Print:
	- `write/1`
	- `writeln/1`

### Unterschiede zu anderen Programmiersprachen
- Klauseln zur Darstellung verschiedenr Fälle
- Prozedur-Notation
- Backtracking
- Unifikation
- Variablen-Notation
- Delay-Mechanismus für Constraints

## CSP (Constraint-Satisfaction-Probleme)
- CSP besteht aus:
	- (Ges:) Variablen V_i, denen Menge möglicher Werte D_i (Domänen) zugeordnet sind
	- (Geg:): Constraints C, über den Variablen V_1,...V_nm die die Menge möglicher Werte der Variablen begrenzen
	- Ziel: Variablenwerte finden, so dass alle Constraints erfüllt sind
- Binäres CSP: hat nur Unäre und Binäre Constraints (haben jew max 1 oder 2 Variablen)
- Endliches CSP: Domänen D_i bestehen aus ganzen Zahlen. Fokus in LV
- Constraint Propagation statt Fehlerbehandlung: Im gegensatz zu Klassischer Programmierung, werden die Constraints zu begin definiert (abgesetzt) anstatt am Ende überprüft zu werden. Außerdem geschieht das Suchen der Werte für die Variablen (Labeling) am ende, statt als Assignment am anfang. 
	- Konsistenz-Mehoden (consistency methods) (mehr siehe Konsistenzverfahren)
		- node-consistency (Knotenkonsistenz)
			- Constraintnetz ist knotenkonsisten, wenn jeder Wert in Domäne der entsprechenden variable alle einstelligen Constraints mit dieser Variable erfüllt.
			- Ein CSP ist knotenkonsitent, wenn alle Knoten knotenkonsistent sind
				- Algorithmus NC (Herstellung von Knotenkonsistenz): Checke für jeden Knoten in V für jeden Wert A in D , ob ein unäres Constraint auf V inkonsitent mit A ist. Falls Ja entferne A von D.
					- Die auf Kannte genannte Relation wird beim Invertieren so invertiert, dass = enthalten bleibt (bzw. nicht hinzukommt). z.b. #<= wird zu #>=. Entsprechend #< wird zu #> 
		- arc-c. (Kantenkonsistenz): Konsistenz von 2 Knoten
			- Eine Kante(i,j) eines Constraintnetzes ist kantenkonsistent, wenn für jeden Werte in der Domäne D_i ein kompatibler Wert in D_j und für jeden Wert in D_j ein kompatibler Wert in D_i existiert.
			- Ein CSP ist kantenkonsitent, wenn jede Kante (in beide Richtungen) kantenkonsistent ist
			- Algorithmus REVISE (stellt einseitige Kantenkonsitenz bzgl V_i -> V_j her): checke für jedes x in Di, ob ein y in Dj vorhanden ist, sodass (x,y) konsistent. Falls nein, entferne x von D_i
				- Wenn min 1 Wert von irgend einer Domain entfernt wurde, muss der Konsistenzcheck für alle Kanten erneut durchgegangen werden
				- Pseudocode siehe Vl03 S. 24(56)
			- Algorithmus AC-1: Überprüfung, ob eine Änderung (CHANGE) notwendig ist
				```
				procedure AC-1 
					Q <-{(Vi,Vj) in arcs(G),i#j}; 
					repeatCHANGE <-false; 
						for jedes (Vi,Vj) in Q do
							CHANGE <-(CHANGE or REVISE(Vi,Vj));
							endfor
						until not(CHANGE) 
					endAC-1
				```
			- Nachweis der Arc consistency durch fd-CP-Programm
				```prolog
				arc_consistency_test([V1,V2,V3]) :-
					% Anfangs-Domänen der Knoten
					V1::1..3, 
					V2::2..3, 
					V3::2,         
					% Constraints
					V1 #\= V2,
					V2 #\= V3,
					V3 #\= V1.
				
				?-arc_consistency_test(L).  %Aufruf der Proz.
					L = [1, 3, 2]   % Lösung (Ergebnis-Domänen)
					Yes  --> Ergebnis-Domäne \== Anfangs-D. ([1..3, 2..3, 2]), daher ursprüngl. CSP nicht arc-konsistent. Aber s ist ein arc-konsistentest CSP daraus erzeugbar.
				```
				- Da möglicherweise kantenkonsistent, aber keine Lösung existiert hinterher noch `labeling([V1,V2,V3])`
			- Manuelles Herstellen der Knatenkonsistenz: (siehe VL03 38(70))
				- 1. Für jede richtung jeder Kannte überprüfen ob constriant eingehalten ist und ggf, Doäne reduzieren. Notation: 
						```
						Domains: [dom(V1, [1,2,3]), dom(V2, [2,3]), dom(V3,[2])]
						V1 -V2: V1[1,2,3] \== V2[2,3] -> V1[1,2,3] \== V2[2,3] % keine Reduzierung
						V2 - V1: ...
						V2 - V3: V2[2, 3] > V3[2] -> V2[3] > V3[2] Reduced Domain: V2[3]
						...

						Next Loop:
						...
				- 2. Wenn Reduzueriung einer Domäne vorgenommen weiterer Schleifendurchlauf. 
			- Manueller Nachweis der Kantenkonsistenz (Am bsp drei Knoten jew Domäne [1,2], ungleich zu nachbarn)
				```prolog
				% Constraints auflisten
				Constraints: X \== Y, Z \== X, Z \== Y 
				% Domänen Auflisten
				[dom(X,[1,2]),dom(Y,[1,2]),dom(Z,[1,2])]
				% Für jeden Wert aus jeder Domäne zeigen, dass alle  Constraints auf dieser Variable erfüllt sind
				X -Y: 	X{1}\== Y{2} -> Constraint ist für X=1 erfüllt
						X{2} \== Y{1} -> Constraint ist für X=2 erfüllt    
				Y -X:	Y{1}\== X{2}-> Constraint ist für Y{1} erfüllt
				...
				Es wurden keine Werte gestrichen -> kein erneuter Durchlauf erforderlich. 
				Das CSP ist kantenkonsistent aber es exisitert (wie intuitiv bzw. durch erfolglose Suche nach geeigneten Werten ersichtlich) keine Lösung.
				```


		- path-c. (Pfadkonsistenz): Konsistenz von drei Knoten
		- k-c.: Konsistenz von k Knoten
	- Zweck der Constraint-Propagation
		- Finden äquivalenden CSP mit kleineren Domänen (Reduzierung des Suchraums)
		- Auswirkungen von Wertezuweisungen feststellen
		- Erkennen ob Constraints inkonsistent (Wiedersprüchlich sind)
			- Constraintlösseer ist unvollständig wenn er nicht immer erkennen kann ob Constraintsystem inkonsistent ist
			- Je mehr Knoten gemeinsam untersucht werden umso besser kann eine Inkonsistenz festgestellt werden (und dadurch weitere Abarbeitung frühzeitig beendet werden)
	- Welchen Zweck haben Konsistenzverfahren? TODO

- Generate-and-Test-Verfahren (einfaches Backtracking-Verfahren)
	- 1. Belegung für (weitere) Teilmenge der Variablen generieren 
	- 2. Konsistenz-Check der Constraints, die diese Variablen enthalten. Erfolgeich: dann weiter mit 1, sonst fail
	- Also entweder erst alle Variablen definieren, dann Constraints testen. Oder z.b. erst 3 Variablen definieren, Constraints dieser testen, dann weiter mit den nächsten Variablen
	- Nachteil: Sehr viele Möglichkeiten werden mitctels Thrashing (zufälliges Belegen der Variablen und dann Backtracking, ineffizient) ausprobiert anstatt vorher den Lösungsraum einzuschränken
	- <i> Implementierung des Nord-West-Ecken-Regel-Verfahrens mittels klassischem Verfahren (Trial-and-Error- bzw. Generate-and-Test Verfahren, welches Backtracking nutzt). Zum lösen des Transportprblems, siehe unten
		- Nord-West-Ecken-Regel-Verfahren: Oben links (A1N1) angefangen so viel wie möglich/nachgefragt liefern, dann weiter bei A1N2,... bis A1 alle seine waren versendet hat, dann mit A2 weiter die noch benötigten Bedarfe befriedigen (A2N1, A2N2,...) .
		- Maximaler Wert für eine Beziehung A1N1: `min([Nachfrage(A1), Angebotsmenge(Anbieter)], Min)`
		```prolog
		transport(L,GesKosten) :-
			% 1. Definition der Variablen
			L=[A1N1,A1N2,A1N3,A1N4,A2N1,A2N2,A2N3,A2N4,A3N1,A3N2,A3N3,A3N4],
			% 2. Wertebereiche eingrenzen
			% Suche nach geeigneten Werten: Eingrenzung der möglichen Werte für AiNj 
			% Wenn statdessen [0,1,2,3,4,5,6,7] wird statt NW-Ecken-Regel Exthausive Suche umgesetzt.
			wertzuweisung([7,6,5,4,3,2,1,0], L), 
			% 3. Constraints absetzen
			6 is A1N1+A1N2+A1N3+A1N4, % A1 hat Angebotsmenge von 6ME
			7 is A2N1+A2N2+A2+A2N4,                               
			6 is A3N1+A3N2+A3N3+A3N4,     
			% write-Anweisungen zur visuellen Kontrolle des Ablaufs               
			2 is A1N1+A2N1+A3N1,  write(4), % N1 will 2ME beziehen
			5 is A1N2+A2N2+A3N2, write(5),    
			7 is A1N3+A2N3+A3N3, write(6),
			5 is A1N4+A2N4+A3N4, write(7),   
			
			GesKosten is A1N1*5+A1N2*15+A1N3*14+A1N4*21+A2N1*8+A2N2*18+A2N3*11+A2N4*18+A3N1*4+A3N2*14+A3N3*9+A3N4*16.
			
		wertzuweisung(_,[]). % EndRek, da Liste vollständig abgearbeitet
		wertzuweisung(Werte, [V|Vars]) :- % Rek für jeden Member aus 2. Arg ausführen
			member(V,Werte),  % constraint, dass V memer von Werte ist
			wertzuweisung(Werte,Vars). % Weiter mit rest

		% Um statt erstbester Lsg die beste zu bekommen:
		besteLoesung(L,GesKosten):- % L ist Liste der AiNj, 
			% Alle möglichen Lösungen (Gesamptkosten + AiNj-Liste) in Liste speichern
			findall([GesKosten1,L1], transport(L1,Z1), Liste),
			length(Liste, Len), writeln('Loesungsanzahl':Len), % Anz Lösungen ausgeben
			sort(Liste, [Result|_]), % Lösungen sortieren, erstes Element ist das beste, der rest interessiert nicht
			Result=[GesKosten,L]. % berechnete Gesamtkosten und Liste mit Werten für die AiNj. Alternativ: [GesKosten, L] = Result.
		```
	</i>
- Constraint-Verfahren:
	- 1. Alle Constraints absetzen
	- 2. Variablen-Belegung suchen
	- Vorteile: 
		- Modellierung erlaubt direkte Verwendung der mathematischen Relationen, die ein Anwendungsproblem beschreiben
		- Effizienz/Problemgröße: erlaubt i.Allg. Interaktivität. Bsp: Job-Shop-Scheduling was verschiedene gleichwertige Lsg haben kann
	- <i>Implementierung Transportproblem als Finite-domain (fd-)Constraint-Program. (Nicht NW-Ecken-Rgel, sondern fd-CP)
		```prolog
		:-lib(fd). %Laden der fd-lib
		:-lib(branch_and_bound). %Laden bb-Suche 
		transport(L,GesKosten) :-
			% 1. Variablen Deklarieren
			L=[A1N1,A1N2,A1N3,A1N4,A2N1,A2N2,A2N3,A2N4,A3N1,A3N2,A3N3,A3N4],
			% 2. Wertebereiche eingrenzen.
			L::0..100, % AiNj dürfen jew zw 0 und 100 sein
			GesKosten::0..10000,
			% 3. Constraints absetzen
			A1N1*5+A1N2*15+A1N3*14+A1N4*21+A2N1*8+A2N2*18+A2N3*11+A2N4*18+A3N1*4+A3N2*14+A3N3*9+A3N4*16 #= GesKosten,
			A1N1+A1N2+A1N3+A1N4 #= 6, %A1 liefert 6 ME an Nachfrager 
			A2N1+A2N2+A2N3+A2N4#=7,
			A3N1+A3N2+A3N3+A3N4#=6,
			A1N1+A2N1+A3N1#=2, % N1 will 2 ME beziehen
			A1N2+A2N2+A3N2#=5,
			A1N3+A2N3+A3N3#=7,
			A1N4+A2N4+A3N4#=5,  
			% 4. Suchen einer optialen Lösung
			bb_min(labeling(L),GesKosten,_). 
		```
	</i>
- <i>Bsp: Transportproblem: Anbieter A1,A2,A3, Nachfrager N1,N2,N3,N4 Beziehungen dier haben Transportkosten pro Mengeneinheit AiNj. Angebotsmenge und Nachfragemenge die insegesamt pro Anbieter bzw. Nachfrager vorhanden sind begrenzt durch Constraint (z.b. A1 kann insgesamt 6 ME liefern). Gesucht: Welche Mengen AiNj von welchem Anbieter zu welchem Nachfrager transportieren, damit Transportkosten minimal.</i>
- <i>Bsp: Gleichungssystem lösen bzw. Gozintograph für Stücklistenproblem lößen. Es gibt Rohstoffe R1, R2, Zwischenprodukte Z1,Z2,Z3 und Produkte P1,P3 und entsprechende Regeln welche Teile wofür benötigt werden, sowie wieviele Produkte am ende benötigt werden
	```prolog
	:-lib(fd).% CLP-Programm
	stueckliste(R1,R2,Z1,Z2,Z3,P1,P2):-
		R1 #= 2*Z1 + 3*Z2 + 4*Z3 + 2*P1,
		R2 #= 3*Z1 + 7*Z2 + 2*Z3,
		Z1 #= 5*P1 + 5*P2,
		Z2 #= 4*P1 + 3*P2,
		Z3 #= 10*P2,
		P1 #= 100,
		P2 #= 150.
	```
	</i>
### Lösung von CSP-Problemen durch Suche + Suche allgemein
- Warum ist Suche im allgemeinen erforderlich? Ziel der Beschäftigung mit Suchverfahren ist es, das Verfahren zu beschleunigen, indem
	- das Verfahren, den Suchbaum zu durchlaufen, variiert wird
	- auf die Vollständigkeit des Verfahrens durch Abschneiden von Teilen des Suchraums verzichtet wird
	- Möglichkeiten genutzt werden, Teste der Randbedingungen vorzuziehen
	- Werte für Variablenbelegungen ausgeschlossen werden  
	- problemspezifische Heuristiken genutzt werden, um Werte für Variablen zu präferieren
- Vergleichstabelle Suchverfahren siehe vl04 S.4-5(6-7): Suchmethode|Verfahren|Durchmustern des Suchbaums|Vollständigkeit des Verfahrens|Lösungserzeugung|Verwendung von Zufallszahlen
- Heuristische und smarte Suchverfahren spielen wesentliche Rolle, da i.Allg. die expoinentielle Komplexität von CSP-Problemen nicht durch Constriaints aufgehoben werden kann
- <i>Bsp: Produktionsplanung mit N (=2) Aufträgen a1, a2 und M (=3) Maschinen m1,m2,m3 (N*M-Probleme). Die Aufträge müssen sequentiell (vorgegebene Reihenfolge) unter verwendungen der Verschiedenen Maschinen bearbeitet werden. Idr Zeit-Horizont gegeben, also wann spätestens fertig sein muss. Gesucht: Optimaler Belegungsplan der Maschinen zu den verschiedenen Zeiteinheiten ZE1, ZE2, ZE3, ZE4, ZE5 (soviele wie Horizont zulässt) </i>
- Exhaustive Suche
	- Generate-and-Test (1.Belegungen für alle Variablen generieren, 2. Constriants testen)
	- Hier also: Kombinationen für [A1m1,A1m2,A1m3], [A2m1,A2m2,A2m3],... generieren. Dann die Constraints absetzen. 
	- Nachteil: ist zb. daserste Constraint nicht eingehalten werden unnötigerweise trotzem alle Kombinationen der späteren Constraints zuerst durchgegangen werden.
	- Standard-Backtracking (kombination/3 siehe unten)
		```prolog
		%Backtracking-Verfahrenfür 3*3-Job-Shop-Scheduling
		schedule(Horizont,Startzeiten) :-
			kombination(Horizont,[A1m1,A1m2,A1m3]), % 1.def
			A1m1 < A1m3, A1m3 < A1m2, % 2. check
			kombination(Horizont,[A2m1,A2m2,A2m3]), % 1. def
			% 2. check
			A2m3 < A2m1, A2m1 < A2m2,
			not A1m1=A2m1, not A1m2=A2m2, not A1m3=A2m3, 
			kombination(Horizont,[A3m1,A3m2,A3m3]), % 1. def
			%2. check
			A3m1 < A3m2, A3m2 < A3m3,
			not A1m1=A3m1, not A2m1=A3m1,not A1m2=A3m2, 
			not A2m2=A3m2,not A1m3=A3m3, not A2m3=A3m3,
			% Ergebnis in Einer Liste Speichern
			Startzeiten = [[A1m1,A1m2,A1m3],[A2m1,A2m2,A2m3], [A3m1,A3m2,A3m3]]).
		```
		- Wenn stattdessen umsortiert, dass alle Constraints zuerst, dann Definitionen, dann ist es ein Constraint-Verfahren. Die Behandlung der Constraints mit evt. ungebundenen Variablen erfolgt durch einen Constraint-Solver.

- Heuristische (informierte, intelligente Suche)
- Unterscheidung zwischen Permutation, Varianz und Kombination
	- Permutation: Vertauschung von Elementen einer Menge, wobei Reihenfolge der Elemente wichtig ist. (Z.b. Anordnung von 4 Büchern im Regal)
		- N Elemente --> N! verschiedene Anordnungen möglich
	- Variantion: Variation zur KLasse r von k Elementen ohne Wiederhlung(`r=<k`)
		- Bzw. Geordnete Auswahl einer Menge von r Elementen aus einer Menge mit k Elementen ohne Wiederhohlung. Z.b. also [1,2,3] (r=3) aus [1,2,3,4] (k=4)auswählen
		- k!/(k-r)! Möglichkeiten
	- Kombination: Auswahl einer Menge von r Elementen aus einer Menge mit k Elementen ohne Berücksichtigung der Reihenfolge und ohne Wiederholung
		- Bsp: Jemand kann sich aus 4 Büchern 3 auswählen unt mitnehmen (kein Unterschied zw. [1,2,3] und [3,1,2])
		- k über r = k!/(r!*(k-r)!) Möglichkeiten
		- <i>Bsp: Kombinations-Prozedur für Produktionsplanung	</i>
			```prolog
			%kombination(Elemente, Kombination)
			kombination(_,[]). % EndRek
			kombination(HorizontElemente,[Variable|Rest]) :- % Variable: Teil-Aufgabe eines Jobs
				'Member'(Variable,HorizontElemente,HorizontElementeRest),
				kombination(HorizontElementeRest,Rest).

			'Member'(El, [El | T], T). %nicht-rücknehmbare Herausnahme eines Elementes
			'Member'(El, [_| T], TRest) :-
				'Member'(El, T, TRest).
			```
- Suchverfahren im Rahmen der Constraint-Programmierung
	- `indomain(V)`: wie `indomain(V,min)`
	- `indomain(V,min)`: erte Auswahl aus Domäne der Variablen V von links nach rechts
	- `indomain(V,max)`: Werteauswahl aus Domäne der Variablen V von rechts nach links
	- `indomain(V, Strategie)`: siehe Eclipse-Dokumentation
	- `labeling(L)`: Realisierung der Tiefe-zuerst-Suche für eine Liste L von Variablen

	- `search/6(L, Arg, Select, Choice, Method, Option)`: Weitere Suchverfahren mit Var-Werte und Suchstrategie-Auswahl
		- in ic, fd_search, gfd_search, bei uns aus gf_search
		- L: List eine Liste von Domänenvariablen (dann ist Arg=0) oder eine Liste von Termen (dann ist Arg > 0) 
		- Arg: eine ganze Zahl, die 0 ist, wenn L eine Liste von Domänenvariablen. Ist L ein Term bezeichnet Arg die Position des ausgewählten Arguments im Term. 
		Select: eine built-in-Methode zur Variablenauswahl [oder der Name einer zweistelligen Prozedur]. 
			- Daraus resultieren unterschiedlich aufgebaute Suchbäme und i.Allg. unterschiedliche Reihenfolgen möglicher Lösungen
			- Built-in-Methoden zur Variablenauswahl sind:  
				- input_order: Den Variablen werden entsprechend der Reihenfolge in der Variablenliste L Werte zugeordnet
				- first_fail: Der Variablen mit der kleinsten Domäne wird zuerst ein Wert zugeordnet
				- anti_first_fail: Der Variablen mit der größten Domäne wird zuerst ein Wert zugeordnet
				- smallest: Der Variablen mit dem kleinsten Wert in der Domäne wird zuerst ein Wert zugeordnet.
				- largest: Der Variablen mit dem größten Wert in der Domäne wird zuerst ein Wert zugeordnet.
				- most_constrained: Der Variablen mit dem kleinsten Wert in der Domäne wird zuerst ein Wert zugeordnet. Haben mehrere Variable dieselbe Domänengröße wird die Variable ausgewählt, die in den meisten Constraints vorkommt
				- max_regret: Die Variable mit der größten Differenz zwischen kleinstem und zweitkleinstem Wert in der Domäne wird ausgesucht. 
				- occurrence: TODO
		- Choice: eine built-in-Methode zur Werteauswahl: 
			- Beeinflusst die Priorität der Lösungen
			- indomain: Werte werden aus den Domänen in der gespeicherten Reihenfolge entnommen. Die Domänen selbst werden aber nicht verändert.
			- (indomain_min: wie indomain, [aber im Fall von BT wird der vorher verwendete Wert aus der Domäne entfernt.])
			- indomain_max: Werte werden aus den Domänen in umgekehrter gespeicherten Reihenfolge entnommen. 
			- indomain_middle: Werte werden aus den Domänen beginnend beim wertmässigmittleren Wert und dann abwechselnd darüber und darunter entnommen [Im Fall von BT wird der vorher verwendete Wert aus der Domäne entfernt.] 
			- indomain_median: Werte werden aus den Domänen beginnend beim anzahlmässig mittleren Wert und dann abwechselnd darüber und darunter entnommen. [Im Fall von BT wir.]
			- indomain_split: Die Domänen werden in Teile aufgespalten und aus jedem Teil Werte wie für indomainentnommen. [Im Fall  von BT wird der vorher verwendete Domänenteil aus der Domäne entfernt.]
			- indomain_random: Werte werden aus den Domänen in zufälliger Reihenfolge entnommen. [Im Fall von BT wird der vorher verwendete Wert aus der Domäne entfernt.] 
				- Durch Aufruf der Prozedur seed/1 kann das Ergebnis der Verwendung zufälliger Werte reproduzierbar gemacht werden. 
			- indomain_interval: Wenn die Domäne aus mehrerer Intervallen besteht, wird zuerst ein Intervall ausgewählt. Für das ausgewählte Intervall wird die Methode indomain_splitverwendet.
			- [Kann auch der Name  eines einstelligen Prädikats oder ein Term mit zwei Argumenten und denselben Funktor wie ein dreistelliges Prädikat sein.]
		- Method (Verfahren):
			- `complete`: Alle alternativen Möglichkeiten werden verwendet
			- `bbs(Steps)`: bounded backtracking search(anzahlbegrenztes Backtracking) erlaubt `Steps` Backtrackingschritte
			- `lds(Abweichung)`: limited discrepancy search. Die Methode erlaubt iterativ  0, 1, 2 .. `Abweichung`en gegenüber der Heuristik  (erster Wert). Typische Werte liegen zwischen 1 und 3.
			- `dbs(Level, bbs(Steps))`: depth bounded search(tiefenbegrenztes Backtracking). Der Suchbaum wird die angegebene Suchtiefenbegrenzung `Level` vollständig durchsuchte. Für tiefere Suchtiefen sind nur `Steps` Backtrackingschritte erlaubt. 
				- Level=1 --> Root Varible kann angepasst werden (so oft wie Steps erlaubt)
				- Level=2 --> Variablen Root und kinder von Root können angepasst werden (so oft wie Steps erlaubt)
			- `dbs(Level, lds(Abweichung))`: depth bounded search(tiefenbegrenztes Backtracking). Der Suchbaum wird die angegebene Suchtiefe `Level` vollständig durchsuchte. Für tiefere Suchtiefen wird die  lds-Methode angewendet. 
			- `credit(Credit, bbs(Steps))` oder `credit(Credit, lds(Abweichung))`: Kredit-basierte Suche. Soviele Backtrackingschritte für Knoten erlaubt wie hoch sein Kredit ist. Wenn der Kredit verbraucht ist, wird auf die alternativ angegebene Methode umgeschaltet. 	
				- `Steps=0` bedeutet deterministische Suche (ohne BT). Ein guter Wert für Steps ist häufig 5. Gute Werte für `Credit` sind entweder  N or N*N, wobei N die Anzahl der Variablen in L ist.
				- Knoten bekommen Kredite: Root -> Credit (z.b. 64); Kinder mit dem Linken beginnend jew halb so viel. Z.b. lc(Root) --> 32, 2ndLc(Root) --> 16

		- Option: Liste von Optionen, z.b. [backtrack(AnzBack)]: 
			- node(++Call)
			- backtrack(-N): liefert die Anzahl der für die Lösung verwendeten Backtrackingschritte.
			- nodes(++N): ist eine obere Schranke für die Anzahl der Knoten im Suchbaum, die durch die Strategie durchlaufen werden. Wenn die vorgegebene Schranke erreicht wird, stoppt die Suche
		- Übung dazu siehe nDamen_search_ueb.ecl
	
		
TODO: einsortieren

- `search(+L, ++Arg, ++Select, ++Choice, ++Method, ++Option)` : Like Labeling but with parameters
	- Select: welche variable zuerst gewählt werden soll
	- Method: wie ihr wert gewählt werden sll
	- labeling does input_order and indomain 
- `abs/1(Number)`: Betrag. Z.b. `abs(Y-X).` um Abstand zw. X und Y berechnen.
	- Angenommen abs ist nicht definiert 
		- V1. mit BacktrackingL in 2 verschiedenen Klauseln und Constraint, dass die mit positiven ergebnis gewählt wird
			```prolog
			abstand(X,Y,Z) :- Z#>0, X-Y #= Z.
			abstand(X,Y,Z) :- Z#>0, Y-x #= Z.
			```
		- V2 ohne Backtracking nmit reified Constraint #=/3 (3. Arg. gibt erfolg an)
			```prolog
			lib(fd).
			abstandV2(X,Y,Z) :-
				[B1, B2]::0..1,
				Z#>0,
				#=(X-Y, Z, B1),
				#=(Y-X, Z, B2),
				B1+B2 #= 1. 
			```
		- V3 ohne Backtracking, ohne reified Constraint mit Disjunction
			```prolog
			abstandV3(X,Y,Z) :-
				Z#>=0,
				(X-Y#=Z or Y-X#=Z). % bzw, (X-Y#=Z ; Y-X#=Z)
			```
- Variables start with Captial Letter or _
- if-else:
	```prolog
		( Bi > 400 ->
			print("Bi is greater than 400")
		;
			print("Bi is smaller or equal 400")
		)
	```
- Predicate definition:  
	```prolog
		functionName(X, X) :-
			... ,
			... ,
			... .
	```
- Read input from commandline:
	```prolog
	read(X)
	```

- Using .ecl files:
	- load and compile file: ['filename.ecl']
		- then the predicates/constraints defined in it can be used
	- Run Prolog program as a script
		```
		eclipse -b filename.ecl -e main
		```
		- `-b` before filename
		- `-e` : excecute predicate with name specified afterwards

- Arithmetic predicates can be used as functions. And use the last param as an output
	```prolog
	f(T, Y) :-
		Y is sqrt(abs(T)) + 5*T^3.
	```
- Meta Prozeduren: TODO
	- `not/1`
	- `Bedingung -> Then ; Else`
	- !/0
	- `findall/3(?Term, +Goal, -List)`: List ist die (möglicherweise leere) liste Aller Variablenkombinationen (Term), die Zum erfolg von Goal führen
		- Term: Prolog Term, usualy variable or computed term or list containing values
		- Goal: Callable term, z.b. Aufruf einer Regel
		- List: Liste oder Variable
- Tests:
	- `=/2`
	- `is/2`
	-`</2, =</2, >=/2, >/2`
	- `true/0`
	- `fail/0`
	- `var/1`
	- `integer/1`


## Constraints:
- Built in Prozeduren (Bip's) für einfache Terme
	- var/1: Variable? 
		- `?-var(X).` => Yes,  `?-var(a).` => No, `?-var([X]).` => No
	- nonvar/1: Keine Variable?
		- `?-nonvar(X)`  => No, 
	- number/1: Zahl?
		- `?-number(1).` => Yes; `?-X=1, number(X).` => Yes; `?-number(1.5).` => Yes
	- integer/1: Ganzzahl?
		-`?-integer(1.5).` => No; `?-integer(1).`→ => Yes
	- atom/1: Name?
		- `?-atom(a).` => Yes, `?-atom(1).` => No,  `atom(X).` => No, `?-atom([]).` => Yes
- Arithmetic constraints
	- #=, #>=, #=<, #>, #<, #/= (not equal)
- Contstraint Expression connectives
	- and: conjunction: `X #> 3 and X #< 8` oder `X #> 3, X #< 8`
	- or: Disjunction: `X #< 3 or X #> 8` oder `X #< 3 ; X #> 8`
	- ->: implication: `X #> 3 -> X #< 8`
		- Once it is known, that X#<= 3 cannot be true, X #<8  is enforced
		- Kann in kombi mit Diskunktion für if-else verwendet werden. Bsp für if A is a number then A1 is A, else A1 is 0:
			`number(A) -> A1=A; A1 = 0.`
	- neg: negation: `neg X #> 3`
- Einfache Constraints (Constraints über zwei Variablen)
	- Können wenn in Prozedur aufgerufen entweder sofort abgearbeitet oder zurückgesetellt werden
	- Zurückgesetellte Constraints bilden eine Dateinstrukutur- iein Constraintnetz-, das bei einer Domänenveränderung aktiviert wird
- Global Constraints
	- Verhalten sich wie Einfache Constraints. Mit Unterschied, dass sie komplexere Zusammenhqnge modellieren (z.b. Constraints über mehr als zwei Variablen)
	- `max(+List, ?Max)`: Speichert Max der liste in Max. Oder falls Max vorgegeben wird die Liste entsprechend Max eingeschränkt. 
	- `min(+List, ?Min)`
	- `sum(+List, ?Sum)`: Summe der Elemente der Liste ist Sum
		- List: Liste von Integer-Zahlen oder Domänen-Variablen
		- Sum: Variable oder Integer-Zahl
	- all_le / 2, all_gt / 2, all_ge / 2, all_lt / 2, all_ne / 2
		- Arithmetischer Vergleich für alle Elemente einer Liste.
		- 1. Param: Liste
		- 2. Param: INteger oder Dommänenvariable
		- `all_eq([1,1,1],1)` ==> Yes
		- `all_eq([1,X,Y],1)` ==> X=1, Y=1
		- `all_eq([1,1,1],X)` ==> X=1
	- `alldifferent/1([V1, ..., Vn])`: Allle Variablen der Liste haben verschiedene Werte
		- in fd, ic, ic_global, gfd. Um mögliche konflite zu vermeiden qualifiziert aufrufen, z.b. `ic:alldifferent([X,Y,Z]).`
	- `alldifferent/2(+Liste, ++Anzahl)`: Jedes Eleement kommt maximal `Anzahl`-mal vor.
	- `gfd:count/4(+Wert, ?Liste, +Relation, ?Anzahl)`: Einschränkung der Auftretenshäufigkeit (`Occurence`) von `Wert` in `Liste` durch Erfüllung der Bedingung Occurence Relation Anzahl (z.b. `Occurence #=< Anzahl)`) (va für Listen in denen manche Elemente mehrfach vorkommen)
		- `+Wert`: Integer oderDomänenvariable
		- `?Liste`: Liste von Constraintvariablen oder von Integer-Werten
		- `+Relation`: Relation aus `#>, #>0, #=, #< , #=<, #\=`
		- `?Anzahl`: Domänenvariable oder Integer
		- `count(2,[1,2,3,2,4], #=<, 2)` => Delayed goals, Yes
		- `count(2,[1,2,3,2,4], #=m, N)` => N=3   (Häufigkeit von Element in Liste)
		- Umsetzung von alldifferent/2 (in gfd nicht vorhanden) mit count/4:
			```prolog
			:-lib(gfd).
			
			alldifferent(L,N) :- % Hilfsprozedur, damit L zum einen Unverändert verfügbar ist und zum anderen Stück für stück abgearbeitet werden kann mit [A|R]
				alldifferent(L,L,N). 

			alldifferent(_,[],_).
			alldifferent(L,[A|R],N) :-
				count(A,L,#=<,N),
				alldifferent(L,R,N).
			```
	- `occurences/3(++Wert, +Liste, ?N)`: Angegebener `Wert` tritt `N` mal in `List` auf
		- fd_global, ic_global gfd
		- `N#>=5 ,occurrences(1,L,N).` Mindestens 5 mal 1 in L 
		- Code für X ist - oder 1 mal in L enthalten. Lösung also 0 oder 1, sonst scheitern
			```prolog
			enthalten(X ,L, Bool) :-
				Bool::0..1,
				occurrences(X, L, Bool)
			```
		- Code für: Jeder Wert aus LX genau einmal in L enthalten
			```prolog
			alle_enthalten([],_,[]).
			alle_enthalten([V|Vs],  WerteListe, [Bool|Rest]) :-
				enthalten(V, WerteListe, Bool), % siehe voriges bsp
				alle_enthalten(Vs, WerteListe, Rest).
			```
	- `element/3(?Index, +List, ?Value)`: Der Wert `Value` ist das `Index`-te Element von List
		- ?Index: Constraint-Variable oder Integer-Zahl .  
		- +List: Nicht-leere Liste von Integer-Zahlen.
		- ?Value: Constraint-Variable oder Integer-Zahl. 
		- `element(4, [1,2,3,2], V)` => V=2
		- `element(I, [1,2,3,2], 2)` => I={[2,4]}
		- Gibt es Tge mit gleichem Gewinn und welche sind das?
			```prolog
			gleicher_gewinn(L, I) :-
				maxlist(L,Max), G::1..Max,   %G ist eine Constraintvariable
				length(L,Len), [I,J]::1..Len, %Domänen angeben!
				I#\=J,element(I, L, G), element(J,L,G),
				labeling([G]).
			
			?-gleicher_gewinn([56,78,45,56,41,56,34],I).
				I = I{[1, 4, 6]} %Lösung ist eine Domäne mit Tagen gleichen Gewinns
				Yes(0.00s cpu, solution1, maybemore) ?
			```
	- `atmost/3(?N,+L,+V)`: Höchsens N Elemente in List haben Wert V. Scheitert wenn mehr als N Elemente in Liste den Wert V Haben. 
		- Nicht Backtrackbar, wenn Wert einer enthaltenen Domänenvariable sich ändert, wird ein suspendierender Aufruf von atmost/3 erneut überprüft ob er noch erfüllbar ist
	- `atleast/3(?N, +List, +V)`: Wenigstens N Elemente in List haben Wert V.
- Alternativen-Behandlung
	-`or/2` TODO
	- `disjuncttive/2` TODO
	- `cumulative/4` TODO
- Reified Constraints: mehr dazu siehe unten
	- `#\/ /3, =>/3, <=>/3 #</3, #=</3,  #>=/3, #>/3, #/\=3, #=/3`
- Optimierung:
	- `min/2(L, Min)` 
	- `max/2(L, Max`
	- `sum/2`
	- `bb_min/3/6`

- Soft Constraints: Kommen nicht ran
- Constraints weiteres
	- `,` : and. All constraints combined with `,` have to be true
	- `.` : excecute
		- If there are more than on solution press `;`
	- `=` : constraint to be equal. `X=2+2` --> `X =2+2`
	- `ic:$=` : = for non integer
	- `is` : equal and label. `X=2+2` --> `X=4`
		- Arithmetic, not relational. Can only work oneway (links ist immer output)
		- Variable is arithmeticExpression: zb. `X is 3+4.`
	- `length(List, Len)` : List der Länge Len erzeugen, oder Länge von Array berechnen
	- `labeling(X, Y,...).` : % assigns concrete values to X,Y...
		- simplest form of search. Chooses smallest value for first variable, ...
- Constraint Solver
	- Aufgabe: Abschwächung )Vereinfachung) der Problemstellung / besseren(einfacheren) Lösbarkeit. 
		- Z.b. durch Relaxionsverfahren: Streichung von Werten der Wertebereiche von Constraintvariablen die keinesfalls in einer Kösung vorkommen können. 
			Wird Variable durch Streichungen leer, ist das Constraint nicht erfüllbar und es erfolgt Backtracking. Anderenfalls als noch erfüllbar betrachtet bis zu späterer (Delay) weiterer Prüfung 
		- Simplexverfahren (Constraint solver über (unendliche Menge) der reellen Zahlen)
			1. Interface (preprocessing) -> 2. Solver for equations --> 3/4 nonlinear constraints (delayed) <--> solver for inequations (Simplex)
		- Finit-Domain (FD)-Solver:	
			- Ziel: Konsistenx, Widerspruchsfreihet des Systems gewährleisten. Streichen von Domänenelementen, die garantiert zu keiner Lösung führen koönnen (und daraus resultierende weitere Streichungen (Weitervermittlung der Änderungen ;  Propagation))
			- Wertebereich ller --> nicht erfüllbar --> Backtracking. Ansonsten hinzufügen des Constraints zum Constraintspeicher und aufruf als erfolgreich betrachtet
	- Arbeitsweise von Constraintlöser abhängig von verschiedenen Einflussgrößen. (Zusammen als Constraintsystem bezeichnet)
		- Form der Constraints (Signatur)
		- Art der Verarbeitung der Constraints (C.-Theorie)
		- Konkret verwendete Constraints (Constraints)
		- Constraintsystem: Tripel (Signatur, Constrainttheorie, Constraints)
			- Beispiele: Logische Programmierung, Constraintlösen über reellen Zahlen, Constraintlößen über ganzen Zahlen, Constraintlösen über Booleschen Zahlen (Voll-Addierer asu AND-, Or, XOR- Gattern) (Mehr Siehe VL03, S.8(37) ff.)
				- Voll-Addierer implementierung zur überorüfung de rfrage wie groß I3 ist (2. input von and und xor)
					```prolog
					%Lösung mit fd-CP-Ansatz:
					:-lib(gfd).  %lib enthält die passenden log. Constr.
					voll_add[X1,X2,X3,O1,O2,I3]) :-
						[I1,I2,I3]::0..1,
						[X1,X2,X3,O1,O2] :: 0..1, 
						I1::1, I2::1, O1::0, O2::1, 
						xor(I1, I2 )  #= X1,   
						xor(X1,I3)   #= O1,
						and(I1, I2)  #= X2,
						and(I3,X1)  #= X3,
						or(X2,X3)   #= O2.
					```
### Reified Constraints
- Könnnen verwendet werden um Zwischenschritte zu verdeutlichen: `X::1..10, (X#=7 or X#=11).` --> X = 7
	--> interne Zwischenberechnung: 
	```prolog
	#=(X{[1..10]}, 7, _625{[0, 1]})
	#=(X{[1..10]}, 11, _669{[0]})
	_625{[0, 1]} + _669{[0]}#>=1  % Bei oder ist einer der beiden Ausdrücke korrekt. Nur bei X#=7 mögll., daher wird der dazugehörige Bool auf 1 gesetzt und somit X=7
	-> _625{[1]} 
	```=
- Welche Funktionalität haben Reified COnstraints?:
	- Alternativenbehandlung vberuht au Verwendun von reified Constraints
	- Steuerung wird an Programierer übergeben. 
- lib fd und ic liefern mehr infos als gfd, außer bei definierter lsg
- Mit Prolog können z.b Diskunktionen mittels Reified Constraints unzerlegt (als Constraint) behandelt werden, worduch Entscheidungspunkt-Setzen und Backtracking vermieden werden, und stattdessen beide Möglichkeiten de rAlternative erhalten bleiben.
	- Anderenfalls in Nicht-Constraint-Programmierung von D1 \/ D2: Zerelgut Behandlung von D1 und erforderlichenfalls (im Backtracking) anschileßende Behandlung von D2
	- Größtes Problem der Traviersierungvon Suchrämen ist eine Zurücknahme getroffener Wertzuweisungen. Dieses Problem wird also mit Reified OCnstraints angegangen
- Reified Constraint: Constraint, dasss arithm. oder logischen Vergleich implizit oder explizit auf seinen Wahrheitswert abbildet. 
	- Pendant mit 1 weiteren Arg (Bool) zu entsprechenden Constraint.
	- Wahrheitswert damit sichtbarer Bestandteil des Constraints
- Scheitern nicht (außer bei fehlerhafter Anwendung), lediglich das Bool wird true, false oder noch nicht bestimmbar.
- In fd:  `#=</3, #\=, #>=/3, #>/3, #</3`
- In gfd: `=:=/3, #=/3, =</3, =\=/3, >=/3,  >/3,  </3` 
	- `=:=/3` gleich, `#=/3` integer gleich 
- Noch nicht lösbar: (Oder)
	```prolog
	Q::1..10,ic:Q#=1 or Q#=3.
		Q = Q{1 .. 10}
		Delayed goals:	
			#=(Q{1 .. 10}, 1, _891{[0, 1]})
			#=(Q{1 .. 10}, 3, _967{[0, 1]})
			-_967{[0, 1]}) -_891{[0, 1]} #=< -1
		Yes (0.00s cpu)
	```
	```prolog
	:-lib(fd).
	X::1..10,fd:(X#=6 #\/ X#=3).
	X = X{[1 .. 10]}
	Delayed goals:
			#=(X{[1 .. 10]}, 6, _676{[0, 1]})
			#=(X{[1 .. 10]}, 3, _720{[0, 1]})
			_676{[0, 1]} + _720{[0, 1]} #>= 1
	Yes (0.00s cpu)
	```
- Oder Lösbar
	```prolog
	X::1..10, fd: #\/(X#=0,X#=3).
		X = 3
	Yes (0.00s cpu)
- Oder, welches als Falsch ausgewertet werden soll
	```prolog
	X::1..11, fd: #\/(X#=6,X#=11,0). 

	X = X{[1 .. 5, 7 .. 10]}
	Yes (0.00s cpu)
	```
- Impliktion lösbar:
	```prolog
	X::1..10, fd: #=>(1,X#=7).
		Delayed goals:
			#=556{_}#=1
			#=(X{[1 .. 10]}, 7, _577{[0, 1]})
			_577{[0, 1]} -1 #>=0
			_577{_} #= 1 %imposed
		X = 7
		Yes (0.00s cpu)
	```
- gfd Reified Implikation (True, wenn Implikation erfolgreich und B 1 oder Implikation nicht erfolgreich und B 0)
	```prolog
	Q::1..10, gfd: =>(#=(Q,1),Q#=3,B).
		Q = Q{[1 .. 10]}
		B = B{[0, 1]}
		Delayed goals:
			gfd : gfd_do_propagate(gfd_prob(nvars(2)))
		Yes (0.00s cpu)
	```
- Berechnungsvorschriften der Bools:
	- Oder (Disjunktion): in ic: `-Bool2 - Bool1 =< -1`, in gfd, fd: `B1 + B2 >= 1`
	- XOR: `B1 + B2 = 1` 
	- Implikation: fd: `E2-E1 >= 0`, ic: `E1-E2 =< 0`
	- Genau-Dann-Wen `+E1 <=> _E2)`: `E1 = E2`
		- `X::1..10, gfd`/fd`: <=>(X#=12,X#=10,B).`
- Werkstattplanung Werkstattplanung (entweder zuerst A2, dann A1, oder zuerst A1, dann A2)
	- nicht reified, aber mit Flag
		```prolog
		ueberlappungs_frei1(Begin1, Dauer1, Begin2, Dauer2) :-
			% [Begin1, End1,Begin2,End2]::7..17,     
			% Begin1 #< End1, Begin2 #< End2,    
			%[Bool1,Bool2]::0..1, %Bool's müssen nicht def. werden,da sie in #>/3 sowieso als Boolsche Var. behandelt werden
			Begin1 #>= Begin2+Dauer2 <=> Bool1,  % A2 vor A1
			Begin2 #>= Begin1+Dauer1 <=> Bool2,  % A1 vor A2
			Bool1 + Bool2 #= 1.  % XOR
		```
	- reified
		```prolog
		Ueberlappungs_frei2(Begin1, Dauer1, Begin2,Dauer2) :-
			%[Bool1,Bool2]::0..1, %Bool's müssen nicht def. werden,da sie in #>/3 sowieso als Boolsche Var.  behandelt  werden 
			<=>(Begin1 #>= Begin2+Dauer2, Bool1), % A2 vor A1
			<=>(Begin2 #>= Begin1+Dauer1, Bool2),   % A1 vor A2
			Bool1 + Bool2 #= 1.

			% Alternativ
			% #>=(Begin1, Begin2+Dauer2, B1), 
			% #>=(Begin2, Begin1+Dauer1, B2),
			% B1 + B2 #= 1. -->

			% Diese prozedur ist als Anwenduer-Constraint definiert, d.h. Suche ist noch nicht spezifiziert. Für lösung: `?-[Beginn1,Beginn2]::7..17, ueberlappungs_frei2(Beginn1,8,Beginn2, 7), labeling([Beginn1,Beginn2])`
		```

## Resourcen

- ELearning course: https://eclipseclp.org/ELearning/index.html



## Konsistenzverfahren (siehe oben: Constraints>Constraint-Propagation)


## Modellierung von Constraint-Problemen & Anwendungen
- Siehe VL05
- Gute Modellierung: modelliertes Problem lässt sich effizient lösen und erweitern/anpassen
	- Dafür hilfreich: 
		- Hinzufügen optionaler Constraints, also Constraints, die zusätzlich zu den notwendigen Constraints hinzugefügt werden
		- Generalisierung
		- Aufbreachen von Symmetrien
			- Symetrien liegen Vor, wenn mehrere problemgrößen (Variablen) gleiche initiale Domänen und gleiche Constraints haben.
			- Nicht künstlich zw symmetrischen Variablen unterscheiden
			- Ausschließen Symmetrischer Lösungen zb durch zusätliche Constraints (z.b. Ordnungsrelationen)
			- Symmetrien Feststellen und Aufbrechen erfordert natürliche Intelligenz
			- <i>bsp: Summe Zweier Variablen X,Y aus gleichen Wertebereichen soll Z ergeben. Symmetrie: z.b. X=3, Y=4 entspricht Y=3, X=4. Zusätzliches Constraint: `X#<=Y`</i>
		- Verwendung redundanter Constraints
			- dienen zur Verbesserung der Propagation
			- kann, muss aber nicht Effizienz verbessern
			- Verringern evt. die Lösungsmenge
		- Teile-und-Herrsche
		- Verwendung globaler Contraints
- Magisches Quadrat: im N*N-Magischen-Quadrat werden Zahlen von 1-N^2 so eingetragen, dass die Summen in jeder Rehe, Spalte und den Hauptdiagonalen gleich sind.
	```prolog
	:-lib(gfd).
	mq3([X1,X2,X3,X4,X5,X6,X7,X8,X9]) :-
		[X1,X2,X3,X4,X5,X6,X7,X8,X9]::1..9, 
		N::1..45, 
		alldifferent([X1,X2,X3,X4,X5,X6,X7,X8,X9]),
		%Zeilensummen
		X1+X2+X3 #=N, 
		X4+X5+X6 #= N,
		X7+X8+X9 #= N,
		%Spaltensummen
		X1+X4+X7 #= N,    
		X2+X5+X8 #= N,
		X3+X6+X9 #= N,
		%Hauptdiagonalen
		X1+X5+X9 #= N,   
		X7+X5+X3 #= N,
		search([X1,X2,X3,X4,X5,X6,X7,X8,X9], 0, input_order, indomain, complete,[]).
	```
	- Kann X1 auch 1 sein?: `?- X1#=1, mq3([X1,X2,X3,X4,X5,X6,X7,X8,X9]).`
- (Nicht-X-)Sudoku: (Spalten z.b. A=[A1,A2,...])
	```prolog
	sudoku :-
		A=[A1,A2,...],
		...
		[A,B,...]::1..9,

		% Spalten ohne duplikate
		alldifferent(A),
		...
		% Zeilen ohne duplikate
		alldifferent([A1, B1,...]),
		...
		% Gruppen ohne Duplikate
		alldifferent([A1,A2,A3,B1,B2,...]),
		...
		flatten([A,B,C,D,E,F,G,H,I]), FL), labeling(FL). % bzw. search/6 statt labeling
	```
- Kartenfärbung
	```prolog
	kartenfaerbung(Laender, AnzFarben) :-
		% Variablendefinitionen
		Laender=[A, B, C,...],
		Laender::1..AnzFarben,
		% Für alle Nachbarn
		verschFarben(A, B),
		...
		% Suche
		labeling(Laender).
	
	verschFarben(A,B) :-
		A #/= B.
	```
	- So wenig farben wie möglich:
		```prolog
		... 
		gfd:max(Laender,Max),
		bb_min(labeling(L), Max, _).      
		```
	- So wenig Farben wie möglich, dann in verbleibenden Lösungen zusäzglich Farb-Codierungen für A und B maximal:
		```prolog
		... 
		gfd:max(Laender,Max),
		bb_min(labeling(Laender),Max,_,_,Max,_), % möglichst wenig Farben
		NegA#=(-1)*A, bb_min(labeling([A]),NegA,_,_,NegA,_), % Farbe A möglichst groß  
		NegB#=(-1)*B, bb_min(labeling(Laender),NegB,_). %Farbe B möglichst groß   
		```
- Hexagonal0Puzzle (siehe Vl05 S.28(32) ff.)
- Werkstattplaung / 3x3-Job-Shop-Scheduling, 
	```prolog
	% Daten
	auftraege('3x3',[J1,J2,J3],[M1,M2,M3]) :-
		J1=[T11,T12,T13],    M1=[T11,T22,T31],
		J2=[T21,T22,T23],    M2=[T13,T23,T32],
		J3=[T31,T32,T33],    M3=[T12,T21,T33].

	% Hauptprogramm
	schedule(Problem, Horizont) :-
		auftraege(Problem, Jobs, Maschinen_Task_Liste),  %Auftragsdaten
		maschinen_job_liste(Maschinen_Task_Liste, Jobs,MJL),
		Jobs::1..Horizont, %Domänen-Festlegung
		task_Reihenfolge(Jobs), %Reihenfolge-C.
		tasks_alldifferent(Maschinen_Task_Liste), %Kapaz.-Constraints
		flatten(Jobs, FJL), search(FJL, Backtracks). %,
		% draw_tasks(MJL,Jobs, Makespan,1).  %struktur. Ausgabe

	task_Reihenfolge([]). % task_Reihenfolge/1 zur Behandlung aller Jobs
	task_Reihenfolge([Job|Jobs]) :-
		task_Reihenfolge1(Job),
		task_Reihenfolge(Jobs).

	% task_Reihenfolge1 der sequentiellen Teilaufgaben eines Jobs festlegen
	task_Reihenfolge_1_job([_]). 
	task_Reihenfolge_1_job([T1,T2|Tasks])  :-
		T1 #< T2, %Reihenfolge-Constraint wird generiert
		task_Reihenfolge_1_job(T2|Tasks]).

	tasks_alldifferent([]). % Zeiten aller Auf Maschine Mi arbeitenden Teilaufgaben müssen verschieden sein
	tasks_alldifferent([Mi | Mrest]) :-
		alldifferent(Mi), %Kapazitäts-C. wird generiert                      tasks_alldifferent(Mrest).

	```

- Traveling-Salesman-Problem (TSP)
	- N städte mit jeder anderen durch Kante verbunden. Gesucht: Verbindung aller Städte, jede genau 1x erreicht (Hamiltonkreis) und bei Anfang endet, außerdem: Gesamt-Kilometerzahl gesucht.
	- Math. Hintergrund: Theorie der Hamiltonschen Kreise
		- Hamiltonkreis-Probleme, wie Königsberger Brückenproblem (Eulerkreisproblem) 
		- Graph G = (V,E) mit Kanten V und Knoten G heißt hamiltonsch, wenn es Kreis gibt, der alle Knoten aus V enthält
			- Existiert nur Hamiltonpfad, ist G seimhamiltonsch
			- Falls Graph vollständig und symmetrisch ex. immer H.-Kreis
				- vollständig: Jeder Knoten mit jedem verbunden
				- symmetrisch: Kosten zwischen Koten gleich für beide richtungen
			- Anz. H.-Kreise: Symmetrischer Graph mit N knoten --> (N-1)!/2, Asymetrisch (N-1)!
		- NP Schweres Problem (exponentieller Rechenaufwand)
	- Wie kürzesten Weg finden (TSP)?:
		- Branch-and-Bound-Verfahren (VL)
		- Dijkstra-Alg, Kruskal-Verfahren (minimal spannende Bäume) 
	- In Prolog:
		- `gfd:ham_path_g/6(?Start,?End,+Succ,++CostMatrix,+ArcCosts,?Cost)`
			- `StartAn`: integer or domain variable (Angabe einer Domäne nicht zwingend.)
			- `End`: An integer or domain variable (Angabe einer Domäne nicht zwingend.)
			- `SuccA`: collection of N different variables or integers. *Liste der (codiert.) Knotenbezeichnungen)
			- `CostMatrixA`: NxN matrix of integers. (Array der Kosten der einzelnen Kanten)
			- `ArcCosts`: collection of N variables or integers. (VariablenListe für die Kanten-Kosten)
			- `CostAn`L  domain variable or integer. (Angabe einer Domäne nicht zwingend.)
			- Indexing beginnt mit 0
			- Arbeitet auf Arrays
			- Hamilton-Pfad Prozedur:
				```prolog
				% Prozedur zum Daten auslesen/konvertieren
				distance_3([]([](  0, 593, 409),[](593,   0, 258),[](409, 258,   0)),[aachen, augsburg, baden_baden],[1, 2, 3]).

				tsp_hp(S,E,PathCosts,ArcCosts) :- %Version 0: Traveling Salesman mit Hamilton-Pfad
					distance_3(KmArray,Staedte,StaedteCodeL),writeln(Staedte), %Daten auslesen, 3-Städte-TSPlength(Staedte, Len), 
					length(ArcCostsC, Len), %Kantenkosten-Liste erzeugen
					ham_path_g(S,E, StaedteCodeL,KmArray, ArcCosts,PathCosts). %Hamiltonpfad-Kosten berechnen
				
				?-tsp_hp(Start, Ziel, PathCosts, ArcCosts). 
					Start = 0
					Ziel = 2
					PathCosts = 851
					ArcCosts = [593, 258, 0]
				```
			- Hamilton-Kreis Prozedur. In 1. Arg wird die Prozedur zum laden der Daten spezifiziert, z.b. distance_3 siehe oben
				```prolog
				%Ermittlung der Kanten-und Gesamt-Kosten für Hamilton-Kreis
				tsp_hk(Distance,S,E,TSPCosts,ArcCosts) :-
					%Transf. Des Problem-Namens in ausführb. Code 
					Call =.. [Distance, KmArray,Staedte,StaedteCodeL], call(Call),
					%Kantenkosten-Liste erzeugen
					length(Staedte, Len), length(ArcCosts1,Len),
					%Hamiltonpfad-Kosten berechnen
					ham_path_g(S,E, StaedteCodeL,KmArray, ArcCosts1,PathCosts),
					%Addition der Kosten der Teilstrecke E nach S
					subscript(KmArray,[E+1,S+1],Last), TSPCosts is PathCosts + Last,  
					%Korrektur der Gesamtkosten
					once(delete(0,ArcCosts1,ArcCosts2)),append(ArcCosts2,[Last],ArcCosts).

				?- tsp_hk(distance_3,Start,End,TSPCosts,ArcC).
					Start = 0
					End = 2
					TSPCosts = 1260
					ArcC = [593, 258, 409]
					Yes (0.00s cpu)
				```
		- (Basic Constraint=Programm ohne ham_path_g/6 und ohne Optimierung und automatischer Straßen-Erzeugung
			```prolog
			% Daten
			fahrbahn(r-b,227,_).
			fahrbahn(b-m,177,_).
			...
			strasse(A-B,Km) :-
				fahrbahn(A-B,Km,_).
			strasse(A-B,Km) :-
				fahrbahn(B-A,Km,_).


			verb(A-B, Accu, [B,A|Accu],Km) :- %Abbruchfall: benachbarte Städtestrasse
				(A-B,Km),
				not member(A,Accu),
				not member(B,Accu).
			verb(A-B, Accu, Erg, Km) :-%rekursiver Fall. Accu: Speicher für erreichte Städte
				strasse(A-C,Km1), % von Stadt A nach C gehen, aber ...
				not member(C, Accu), %... Stadt schon vorher mal erreicht (A im Accu)?
				verb(C-B,[A|Accu],Erg,Km2), %A in Accu geben. Route weiter von Stadt C zu B
				Km is Km1 + Km2. %Addieren der Km

			%Mögliche Aufrufe
			?-verb(r-ch, [], Weg, Km).   
			?-verb(r-ch, [], [S1, S2, S3, S4, S5], Km)
			?-verb(r-ch, [], Weg, 488).
			```
			)
		- Hamilton-Kreis ohne ham_path_g/6, optimiert
			```prolog
			:- lib(gfd).
			:- set_flag(print_depth, 100).   %Drucktiefe erhöhen

			%Codierung der Staedte
			codierung(aachen, 1).
			codierung(augsburg, 2).
			codierung(badenbaden, 3). 
			codierung(basel, 4).
			
			% Decodierung
			dekodiereList([],[]). 
			dekodiereList([A|Rest],[A1|Rest1]) :-
				codierung(A1,A),
				dekodiereList(Rest,Rest1).

			findeverb11b(A-B,VerbList, Km) :-
				Km :: 1..50000,  
				erzeugung(TSPverb, B), 
				setzeDomaene(TSPverb),
				alldifferent(TSPverb),
				verb11(A-B,TSPverb,Km), 
				verb11b(B-A,[A1,B1],Km0), Km is Km0+Km1, append(TSPverb,[B1],TSPverb1),
				dekodiereList(TSPverb1,VerbList). 

			setzeDomaene(Verb):-
				findall([X,Y],
				fahrbahn(X-Y,_,_),L),
				flatten(L,FlatL),
				sort(FlatL,SortL),
				length(SortL,StaedteAnzahlDiskurs),
				Verb::1..StaedteAnzahlDiskurs.

			erzeugung(TSPverb, Letzte_Stadt) :- 
				findall(S, (fahrbahn(S-_,_,_) ; fahrbahn(_-S,_,_)), L),
				sort(L,StaedteListe),  
				length(StaedteListe,StaedteAnzahl),
				length(TSPverb,StaedteAnzahl),  
				(var(Letzte_Stadt)->true;(codierung(Letzte_Stadt,B1),listut:last(B1,TSPverb))).
					

			verb11b(A-B, [A1,B1],Km) :- % Rekursionsanker (Nachbarstädte)
				codierung(A,A1),
				strasse(A-B,Km), 
				codierung(B,B1).
			verb11b(A-B,[A1,C1|Verb], Km) :-
				Km #= Km1 + Km2,
				codierung(A,A1), kuerzeste_strasse(A-C,Km1), codierung(C,C1),
				verb11b(C-B,[C1|Verb],Km2). 
			kuerzeste_strasse(A-C,Km) :-
				findall([Kmt,A-D],strasse(A-D,Kmt),List), %Liste aller Strassen  
				sort(List,SortList), %aufsteigend nach Km
				member([Km,A-C],SortList). %backtrackbar 

			% weitere Datei aachen_13.ecl in der die Fahrbahnen definiert werden
			fahrbahn(badenbaden -  aachen , 409 , _Info).
			fahrbahn( badenbaden -  augsburg , 258 , _Info ).

			?- [aachen_13], findeverb11b(aachen-X,L,Km).
				X = berlinL = [aachen, duesseldorf, dortmund, bielefeld, braunschweig, bremen, bonn, badenbaden, basel, augsburg, chemnitz, dresden, berlin, aachen]
				Km = 3142
				Yes (0.00s cpu)
			```

## Planung von Alternativen (VL09) (siehe auch Disjunctive Scheduling)
- Bei Task Scheduling mit n tasks, n! mögliche Reihen folge. --> Exponentiniell. Daher Vermeidung von Backdracking durch Verwendung globaler Constraints
	- or/2, discjunctive/2, cumulative/4, siehe unten
- Wenn z.b. Studnenplan mit Zeiten, Dozenten und Räumen als resourcen. 
	- Zuerst cumulative für Zeit (indem nur 1 raum verfügbar). A1 und A2 dürfen sich nicht überschneiden: `cumulative([A1,A2],[3,2],[1,1],1),`- `disjunctive/(+StartTimes, +Durations)`: N Tasks mit Ihren Startzeiten und Dauern überlappen zu keiner Zeit
		- StartTimes und Duratiosn sind Listen gleicher Länge N von FD-Variablen oder INtegerzahlen
		- bsp: `[X,Y,Z]::1..12,disjunctive([X,Y,Z],[6,13,2]).`
	- Dann Behandlung der Ressource Dozent. Kurse A1,A2 benötigen Lherer 1 als resource, A3 benöþigt 2. Es gibt 2 lehrer: `cumulative([A1,A2,A3],[3,2,1],[1,1,2],2).`
	- Am ende Lösung bestimen mit `labeling([A1,A2,A3])` 
		- bzw. Falls die Anzahl der Lehrer (4. Arg von 2. aufruf) statt fix auf 2, als Variable angegeben wird und dnaach optimiert werde soll: `bb_min(labeling([A1,A2,A3,A4, Anz]), Anz, _)`
- Bsp: Abstand zwier Punkte auf Jorordinate: `lib(gfd). abstand(X,Z,Y) :- | X-Y | #= Z.`


### Disjunctive scheduling
- `disjunctive/2(+StartTimes, +Durations)`: N Tasks mit Ihren Startzeiten und Dauern überlappen zu keiner Zeit
	- StartTimes und Duratiosn sind Listen gleicher Länge N von FD-Variablen oder INtegerzahlen
	- bsp: `[X,Y,Z]::1..12,disjunctive([X,Y,Z],[6,13,2]).`
- `fd:disjunction/5 (Start1, Duration1, Start2, Duration2, Flag)`
	- Flag: welche der beiden Tasks zu erst belegt werden soll. Oder unbelegt (disjunction/4)
	- Nur in fd, kann aber mit `fd:` Annotation in gfd ohne fd zu laden verwendet werden
	- Bsp.:`[X,Y]::1..10,disjunction(X,2,Y,20,F).`
- `ic_edge_finder:disjunctive(+StartTimes, +Durations)`
	- auch in gfd
	- StartTimes und Durations sind listen gleicher länge
	- Constraint, dass die durch StartTimes und Durations definierten Aufgaben sich nicht überschneiden dürfen
	- fd lib muss gelden sein
	- verweden, wenn sequentielle Reihenfolge nichtüberlappender Tasks mit Startzeiten und Dauern existiert. Für Probleme  bei der zu einem Zeitpunkt nur eine Aufgabe bearbeitet werden kann
	- bsp: 
		```prolog
		[x,Y]::1..10, disjunctive([X,Y],[6,11).
		X = X{[1..4]}
		Y = Y{[7..10]}
		```
- `gfd:cumulative/2`: TODO
	- auch in (ic_)edge_finder(3) 
- `cumulative/4(+StartTimes, +Durations, +Resources, ++ResourceLimit)`
	- `StartTimes`: Liste der Startzeiten der TAsks (FD-Variablen oder Integerzahlen)
	- `Durations`: Liste der Dauern der TAsks (FD-V., oder Ints)
	- `Resources`: Liste der von den Tasks benötigten Ressourcen (FD-V. oder Ints)
	- `ResourceLimit`: Maximale menge der zur Verfügung stehenden Ressourcen (Integerzahl)
		- Darf zu keinem Zeitpunkt überschritten werden. Bezieht sich also immer auf den jew zeitpunkt
		- Kann auch eine Variable sein, die dann angibt wieviele resourcen benötigt werden.
	- Verwendungszweck: Plane n Aufgaben mit Startzeiten Si und Dauer Di unter Verwendung von Ressourcen Ri, wobei L Ressourcen zu jedem Zeitpunkt verfügbar sind
	- Deklarative Bedeutung: In der Lösung des cumulative/4-Constraints übersteigt die Summe der Ressourcen-Verwendung aller Tasks zu keiner Zeit das Ressourcenlimit L
	- Bsp: ?-`A1::1..2,A2::1..2,A3::1..4,A1#<A2, cumulative([A1,A2,A3],[5,2,2],[1,1,1],2).`
		=> A1=1, A2=2, A3=4
	- Bsp: ?-`A1::1..2,A2::1..2,A3::1..4,A1#<A2, cumulative([A1,A2,A3],[5,3,2],[1,1,1],2).`
		=>  no
	- Übung:
		?-`A1::1..2, A2::1..2, A3::1..4, A1#=1, cumulative([A1, A2, A3],[5,2,2], [1,1,1], 2).`
			=>A1=1, A2=A2{[1,2]}, A3=A3{[3,4]} 

- Code disjunctive scheduling
	```prolog
	:- lib(gfd).
	main(Begins) :-
		Begins = [Beg_Work, Beg_Mail, Beg_Shop, Beg_Bank],
		Durations = [4, 1, 2, 1],
		Ends = [End_Work, End_Mail, End_Shop, End_Bank],
		Beg_Work::11..13,
		Beg_Shop #> End_Bank,
		Beg_Worl #> End_Mail,
		Limit = 1,
		Begins::9..17,
		Ends::9..17,
		cumulative(Begins, Durations, [1,1,1,1], 1)
		labeleing(Begins).
		
	one_thing_at_a_time(Begins, Durations) :-
		disjunctive(Begins, Durations).


	one_thing_at_a_time_v2(Begins, Durations) :-
		length(Begins, Len), % länge berechnen
		length(Ressourcen, Len), % liste mit länge len erzeugen
		Resources::1.  % alternativ: all_eq(Resources, 1).
		cumulative(Begins, Durations, Ressources, 1).
	```
	

# VL 11 Optimierung
- `bb_min(+Goal, ?Cost, ?Options)`
	- lib(branch_and_bound)
	- Suchen einer optimalen Lösung hinsiichtlich der Kostenvariablen (Domänenvariable)  unter Verwendung der Branch-and-Bound-Methode
	- `Goal`: ein nichtdeterministischer Prozeduraufruf
	- `Cost`: Variable, deren Wert durch Aufruf beeinflusst wird. Domänenvariable
		- Variablen aus Cost müssen im Goal vorkommen
	- Sobald Lösung gefunden wird diese gespeichert und Suchprozedur fortgesetzt oder erneut gestartet mit zusätzlichen Constraint für die Kosten, sodass nächste Lösung besser sein muss als vorhergehende (nicht möglich-> optimale lsg bereits erreicht)
	- `Options`:
		- Angabe der Optionen in Form `bb_options{Schlüsselwort: Eigenschaft, ...}`
			- z.b. `bb_options{strategy: continue, timeout: 0.6}`
		- Angabe einer max Zeitvorgabe (erreicht-> beste lsg genommen oder scheitern)
		- `stragtegy`:
			- `continue`: default. Fortsetzung der Suche ohne Neustart. Für neuen Kostenwert werden die Werte de rSchlüsselwörter delta und factor berücksichtigt
			- `restart`: Fortsetzung der Suche mit Neustart. delta & factor berücksichtigt
			- `dichotomic`: backtrackbares Aufspalten des verbleibenden Kostenbereichs und zunächst Verwendung des untereen Teils. delta & factor berücksichtigth
		- `from`: Zahl, default: -10000000: Anfangswert für die unteren Kostengrenze
		- `to`: Zahl: Default 10000000: Anfangswert für die obere Kostengrenze
		- `delta`: Zahl: Default 1.0: Min Verbesserungsgrad der Kosten in jedem Schritt (Schrittweite)
		- `factor`: Zahl:
			- für continue und restart stragegie:
				- Default 1.0
				- minimales Verbesserungs-Verhältnis. - Neue Kosten=floor(Zahl*(ermittelte Kosten / untere Kostengrenze))
				- 0<Zahl=<1
			-für dichotomic strategie: 
				- Aufspaltungsfaktor des Kostenbereichs = untererTeil/Gesamtbereich
				- Default 0.5
		- `timeout`: Zahl: Default keine Begrenzung
			- Maximale cpu-Zeit [sec]
		- `report_success`: Aufruf einer N+3-stelligen Prozedur
			- Bei finden besserer Lösung ausgabe einer Standardnachricht. 
			- Argumente der callback funktion: 
				- Zeit: wieviel zeit vergangen ist
				- Kostenvariable: Wert der z.b. gedruckt werden soll
				- _Handle
				- _Module
			- z.b. bb_options{report_success: mywrite(Zeit, Kosten, _Handle, _Module)}
		- `report_failure`: Aufruf einer N+3-stelligen Prozedur
			- Calback für fail
	- nicht backtrackbar.
	- Bsp: Spielplanung, suche frühsetmögliches Turnierände
	- für maximum den betrachteten wert (Cost) negieren.
	- basiert auf bb_min/6: 
		```prolog
		bb_min(Goal, Cost, Options) :-
	    	bb_min(Goal, Cost, Goal, Goal, Cost, Options).
		```
- `branch_and_bound:bb_min/6 (+Aufruf, ?Kosten, ?Muster, ?Lösung, ?Optimum, ?Options)`
	- Lösung für Aufruf so dass Maximum der Kosten minimiert wird. Optimierung unter Anwendung des Kriteriums Minimierung des Wertes von Kosten. Nicht Aufruf und Kosten werden mit optimalen Werten belegt, sondern Lösung und Optimum. 
	- Werden Lösung und Optimum nicht angegeben (anonyme Var.), sind die ermittelten entsprechenden optimalen Werte nur bb_min/6-intern)
	- Immer wennn bessere Lösung gefunden, druckt Standard-Druck-Handler die augenblicklichen Kosten
	- Args:
		- `Aufruf`: ausführbarer Term (auch ein bb_min/3 oder bb_min/6 Aufruf möglich)
		- `Kosten`: positive Domänenvariable
		- `Muster`: Term der alle oder einige Variablen aus Aufuf enthält
		- `Lösung`: unifiziert mit Muster bei erfolgreicher OPtimierung. Hier werden also die Werte der Variablen die für optimale Lösung zum einsatz kamen zurückgegeben
		- Optimum: unifiziert mit dem Optimum der Kosten. Hier wird also die optimale Cost-Variable zurückgegeben
		- Options: Optionen
	- Im gegensatz zu `bb_min/3` wo die ersten beiden Optimierung nach aussen sichtbar gemacht werden, werden bei `bbimn/6` das 4. & 5. Argument (Lösung und Optimum) sichtbar gemahct. 
	- Werden Lösung und Optimum nicht angegeben, sind die optimierten Kosten und die dazu passenden Werte der Variablen in Aufruf nur bb_min/6-intern, d.h. sie haben nach der Optimierung den gleichen Zustand wie vor der Optimierung. 
	- Gutes Bsp, Siehe VL12 Folie 11 (11): Bildung von Summe X1,...,X5, mit Resultat.., Auf minimalen wert für X3 optimieren. 
		- zb um optimalen Wert herauszufinden, aber die betroffenen Variablen nicht festzuschreiben. 
			`bb_min(labeling(List), NegX3, _, _, X3opt, bb_options{timeout:1})` 
		- List enthählt hier Variablen X1,...X5. Minimaler Wert für X3 wird berechnet, ausgegeben und in X3opt gespeichert. X1,...X5 werden nicht festgeschrieben. 
		- Lösung für X1,...,X5 nicht festschreiben/einschränken, aber alle Informationen erhalten: `bb_min(labeling(List),NegX3,result([X1,X2,X4,X5]),Resultat,X3opt,bb_options{timeout:1}),X3wert is (-1)*X3opt.` 
	- Kryptoarithmetisches Rätsel: 
		```
		B+ D =  E   
		+  +
		F+ B =  I 
		+ +
		A- G =  C 
		--------
		CG+CH =DDC
		``` 
		- Hilfsprozedur für Ausgabe:
			```prolog
			write_output([A,B,C,D,E,F,G,H,I]) :-
				write('   B+ D =  E  |  '),writeln(B + D = E),
				write('   '),writeln('+  +       |  + +'),
				write('   F+ B =  I  |  '),writeln(F + B = I),
				write('   '),writeln('+  +       |  + +'),
				write('   A- G =  C  |  '),writeln(A - G = C), 
				writeln('--------------|----------'),
				S1 is (C*10+G), S2 is (C*10+H), S3 is (D*100) + (D*10) + C,
				write('  CG*CH =DDC  | '),writeln(S1 * S2  =  S3),
				(S3 is S1*S2 -> true; writeln('FEHLER')),
				nl,
				gfd:sum([F,B,I],FBIsum), writeln(['FBIsum'= FBIsum, 'A' = A]),
				gfd:max([A,B,C,D,E,F,G,H,I],Max),
				(Max=<9 -> write('L = [A, B, C, D, E, F, G, H, I]')
						; write('L = [A,  B,  C,  D,  E,  F, G,  H,  I]')).
			```
		- Code für minimale Lösung für E
			```prolog
			raetsel_minE(L) :-%minimale Lösung für E
				L=[A,B,C,D,E,F,G,H,I],
				L::0..99,
				alldifferent(L),B + D #= E,
				F + B #=  I,
				A -G #= C,
				(C*10+G)*(C*10+H) #= (D*100)+(D*10)+C,
				B+F+A #= (C*10+G),
				D+B+G #= (C*10+H),
				bb_min(labeling(L),E,_),
				write_output([A, B, C, D, E, F, G, H, I]).
			```
		- Code für maximale Lösung für H
			```prolog
			raetsel_maxH(L) :- %max. bezg H
				...
				NegH #=(-1)*H,bb_min(labeling(L),NegH,_),
				write_output([A, B, C, D, E, F, G, H, I]).
			```
		- Code für max vorkommende Zahl  
			```prolog
			raetsel_maxElem(L) :- % max. Element
				...
				gfd:max(L,Max), NegMax #=(-1)*Max,
				bb_min(labeling(L),NegMax,_),  
				write_output([A, B, C, D, E, F, G, H, I]).
			```
		- Code für Summe (F+B+I) maximal
			```prolog
			raetsel_FBImax(L) :-%max. bezgl. Summe F,B,I
			 	... 
				Min1 #= F+B+I,  %oder: sumlist([F,B,I],Min1),
				NegMin1#=(-1)*Min1,
				bb_min(labeling(L),NegMin1,_),
				write_output([A, B, C, D, E, F, G, H, I]).
			```

- Kombinationen von bb_min/3 und bb_min/6
	- Max Wert der Liste minimal, zusätzlich zwei werte A,B der Liste maximal: Durch Sequentielles Abarbeiten: 
		```prolog
		Liste = [A,B,C]
		gfd:max(Liste,Max),
		bb_min(labeling(Lliste),Max,_,_,Max,_), % auf Max optimieren und Max festschreiben, den Rest nicht (Arg. 4 ist leer)
		NegA#=(-1)*A, bb_min(labeling([A]),NegA,_,_,NegA,_), % A möglichst groß
		NegB#=(-1)*B, bb_min(labeling(Terr),NegB,_).  % Farbe Q möglichst groß
		```
	- Sequentiell vs Hierarchisch:
		- Sequetiell: Nur eine richtige Lösung
		- Hierarchisch Im inneren bb_min aufruf bb_min/6 und nur teilmenge der gesuchten Variablen labeln. 
			- z.b. Summe A+B maximal, unter den möglichen Lösungen C minimal
				```prolog
				Summe #= A + B, NegSumme = (-1) * Summe,
				Liste #= [A,B,C,D]
				gfd:max(Liste,5),
				bb_min(
					(bb_min(labeling(Liste),NegSume,_,_,NegSumme,_), % Summe maximieren und festschreiben 
					, labeling(List)), C, _). % in äußerem min auf C optimieren und alle Veriablen labeln
					
				```
- bb_min Beispiele
	- Turnierplanung:  Minimal mögliche Spieldauer bei zwei Plätzen
		```prolog
		:-lib(gfd)
		% Programm zur Optimierungsaufgabe: Minimal mögliche Spieldauer bei zwei Plätzen
		hin_und_rueck_opt(Ts, Anz_der_Plaetze,Timeout) :-
			Ts = [   	 T12, T13, T14, T15,
					T21,      T23, T24, T25,
					T31, T32,      T34, T35, 
					T41, T42, T43,      T45,
					T51, T52, T53, T54         ], 
			length(Ts,LenTs), AnzSpiele is 2*LenTs, 
			Ts :: 1..AnzSpiele, 
			max_soviel_spiele_wie_plaetze(Ts, Anz_der_Plaetze), 
			mindestens_ein_spiel_pause([T12,T13,T14,T15, T21,T31,T41,T51]),
			mindestens_ein_spiel_pause([T12,T32,T42,T52,T21,T23,T24,T25]),
			mindestens_ein_spiel_pause([T13,T23,T43, T53,T31,T32,T34,T35]),
			mindestens_ein_spiel_pause([T14,T24,T34,T54,T41,T42,T43,T45]),
			mindestens_ein_spiel_pause([T15,T25,T35,T45,T51,T52,T53,T54]), 
			
			gfd:max([T12,T13,T14,T15,T23,T24,T25,T34,T35,T45], Max_Hin),
			gfd:min([T21,T31,T32,T41,T42,T43,T51,T52,T53,T54], Min_Rueck),
			Min_Rueck #> Max_Hin, gfd:max(Ts, Max),
			Time1 is cputime,
			bb_min(labeling(Ts), Max, bb_options{timeout:Timeout,report_success:write_success(Time1)}),
			writeturnier_mit_rueckspiel(Ts,5).
		
		% Alternative Prozedur zu gleichzeitige_spiele/2
		max_soviel_spiele_wie_plaetze(Ts,Anz_Plaetze) :-
			%Liste der Platzanzahl für jede Mannschaf
			max_soviel_spiele_wie_plaetze1(Ts, Anz_Plaetze, MaxPs),
			%Bestimmung der maximal mindestenserforderlichen Platzanzahl
			gfd:max(MaxPs,Anz_Plaetze).              

		max_soviel_spiele_wie_plaetze1([_],_,[]).
		max_soviel_spiele_wie_plaetze1([First|Rest], C,[MaxP|MaxPs]) :-
			max_soviel_spiele_wie_plaetze_work(First, Rest,C,[1],MaxP),
			max_soviel_spiele_wie_plaetze1(Rest,C,MaxPs).

		max_soviel_spiele_wie_plaetze_work1(_,[],Anz_Plaetze,BoolList,Sum) :-
			gfd:sum(BoolList,Sum), Sum#=<Anz_Plaetze#<=>1.
		max_soviel_spiele_wie_plaetze_work1(First,[S|Spiele],Anz_Plaetze,Acc,Sum) :- 
			B::0..1,  First #=S #<=> B,   
			max_soviel_spiele_wie_plaetze_work1(First,Spiele,Anz_Plaetze,[B|Acc],Sum).
		
		
			
		% minimal mögliche Spieldauer bei 1 Platz
		?- hin_und_rueck_opt(Ts,1, 5). 
			Costs -22 -in -0.17 -sec
			Costs -21 -in -0.17 -sec
			Costs -20 -in -3.78-sec
			Branch-and-bound timeout while searching for solution better than 20
			0  1  3  6  8
			12 0  7  4  10
			14 17 0  9  5
			16 19 11 0  2
			18 15 20 13 0
			Yes (5.00s cpu) 
		
		%  Minimal mögliche Spieldauer bei zwei Plätzen
		?- hin_und_rueck_opt(Ts,2, 0.5).
			Costs -23 -in -0.32 -sec
			Costs -21 -in -0.32 -sec
			Costs -19 -in -0.32 -sec
			Branch-and-bound timeout while searching for solution better than 19
			0  1  3  5  7
			11 0  5  7  9
			13 15 0  9  1
			15 17 19 0  3
			17 19 11 13 0
			Yes (0.52s cpu)

		% Mindestens notwendige Platzanzahl
		?- hin_und_rueck_opt(Ts,P, 0.5).
		```
	- Turnierplanung: Minimale Zeitdauer bei maximal notwendiger Platzanzahl  
		```prolog
		:-lib(gfd)
		hin_und_rueck_maxPlatz(Ts, Anz_der_Plaetze) :-
			Ts = [T12,T13,T14,T15,T21,T23,T24,T25,T31,T32,T34,T35, T41,T42,T43,T45,T51,T52,T53,T54], 
			length(Ts,LenTs), AnzSpiele is 2*LenTs, Ts :: 1..AnzSpiele, 
			max_soviel_spiele_wie_plaetze(Ts, Anz_der_Plaetze),
			mindestens_ein_spiel_pause([T12,T13,T14,T15, T21,T31,T41,T51]),
			mindestens_ein_spiel_pause([T12,T32,T42,T52,T21,T23,T24,T25]),
			mindestens_ein_spiel_pause([T13,T23,T43, T53,T31,T32,T34,T35]),
			mindestens_ein_spiel_pause([T14,T24,T34,T54,T41,T42,T43,T45]),
			mindestens_ein_spiel_pause([T15,T25,T35,T45,T51,T52,T53,T54]), 
			gfd:max([T12,T13,T14,T15,T23,T24,T25,T34,T35,T45], Max_Hin),
			gfd:min([T21,T31,T32,T41,T42,T43,T51,T52,T53,T54], Min_Rueck),
			Min_Rueck #> Max_Hin,  
			gfd:max(Ts,Max),
			bb_min(labeling(Ts),Max,bb_options{timeout:Timeout}),
			TD::0,  writeturnier_mit_rueckspiel([TD,T12,T13, T14, T15, T21,TD,T23, T24,T25,T31,T32,TD,T34,T35, T41, T42, T43,TD, T45, T51, T52, T53, T54,TD],5)

		?- hin_und_rueck_maxPlatz(Ts,P).
			Costs --2-in-2.84-sec
			Branch-and-bound timeout!
			Costs-23-in-2.85-sec
			Costs-21-in-2.85-sec
			Costs-19-in-2.87-sec
			Branch-and-bound timeout!
			0  1  3  5  7
			11 0  5  7  9
			13 15 0  9  1
			15 17 19 0  3
			17 19 11 13 0
			
			Ts = [1, 3, 5, 7, 11, 5, 7, 9, 13, 15, 9, 1, 15, 17, 19, 3, 17, 19, 11, 13]
			P = 2
			Yes (9.94s cpu)
		```
	- Turnierplanung: Minimal mögliche Platz anzahl, dannn minimale Zeitdauer 
		```prolog
		hin_und_rueck_minPlatz(Ts, Anz_der_Plaetze) :-
			Ts = [T12,T13,T14,T15,T21,T23,T24,T25,T31,T32,T34,T35, T41,T42,T43,T45,T51,T52,T53,T54], 
			length(Ts,LenTs), AnzSpiele is 2*LenTs, Ts :: 1..AnzSpiele, 
			max_soviel_spiele_wie_plaetze(Ts, Anz_der_Plaetze),
			mindestens_ein_spiel_pause([T12,T13,T14,T15, T21,T31,T41,T51]),
			mindestens_ein_spiel_pause([T12,T32,T42,T52,T21,T23,T24,T25]),
			mindestens_ein_spiel_pause([T13,T23,T43, T53,T31,T32,T34,T35]),
			mindestens_ein_spiel_pause([T14,T24,T34,T54,T41,T42,T43,T45]),
			mindestens_ein_spiel_pause([T15,T25,T35,T45,T51,T52,T53,T54]), 
			gfd:max([T12,T13,T14,T15,T23,T24,T25,T34,T35,T45], Max_Hin),
			gfd:min([T21,T31,T32,T41,T42,T43,T51,T52,T53,T54], Min_Rueck),
			Min_Rueck #> Max_Hin,  
			gfd:(Ts,Max),
			bb_min(labeling(Ts),Anz_der_Plaetze, _, _, Anz_der_Plaetze, bb_options{timeout:Timeout}),
			bb_min(labeling(Ts),Max, bb_options{timeout:Timeout}),
			TD::0,  writeturnier_mit_rueckspiel([TD,T12,T13, T14, T15,    T21,TD,T23, T24,T25,T31,T32,TD,T34,T35, T41, T42, T43,TD, T45, T51, T52, T53, T54,TD],5). 

		?- hin_und_rueck_minPlatz(Ts,P,1). 
			Costs-2-in-0.46-sec
			Costs-1-in-0.46-sec
			Branch-and-bound timeout!
			Costs-22-in-0.46-sec
			Costs-21-in-0.46-sec
			Costs-20-in-0.67-sec
			Branch-and-bound timeout!
			0  1  3  6  8
			12 0  7  4  10
			14 17 0  9  5
			16 19 11 0  2
			18 15 20 13 0

			P = 1
			Yes (1.49s cpu)

		```
	- Turnierplanung-pausen: Minimierung der Gesamt-Pausenzeit der 5 Manschaften. Nur Hinspiele. Dann Turnierdauer minimal. 
		```prolog
		hinspiele_PD_mi_opt(Ts, Anz_der_Plaetze,Timeout) :-
			Ts = [T12, T13, T14,T15, T23, T24, T25, T34, T35, T45],
			Ts :: 1..100,  
			max_soviel_spiele_wie_plaetze(Ts, Anz_der_Plaetze), 
			mindestens_ein_spiel_pause([/**/  T12,T13,T14,T15]), 
			mindestens_ein_spiel_pause([T12,/**/ T23, T24,T25]),
			mindestens_ein_spiel_pause([T13, T23, /**/T34,T35]),
			mindestens_ein_spiel_pause([T14,T24, T34, /**/T45]),
			mindestens_ein_spiel_pause([T15,T25,T35, T45 /**/]),   
			gfd:max(Ts,MaxTs),    
			pausen([/**/  T12,T13,T14,T15],D1,Ges1), 
			pausen([T12,/**/ T23, T24,T25],D2,Ges2), 
			pausen([T13, T23, /**/T34,T35],D3,Ges3), 
			pausen([T14,T24, T34, /**/T45],D4,Ges4), 
			pausen([T15,T25,T35, T45 /**/],D5,Ges5), 
			gfd:max([Ges1,Ges2,Ges3,Ges4,Ges5],MaxG),
			bb_min(labeling(Ts),MaxG,_,_,MaxG,bb_options{timeout:Timeout}), % pausen minimieren
			bb_min(labeling(Ts),MaxTs,bb_options{timeout:Timeout}), % Turnierdauer minimieren
			writeln('Einzel-Pausen der Mannschaften:'), writeln(D1-D2-D3-D4-D5), 
			writeln('Gesamt-Pausen der Mannschaften':Ges1-Ges2-Ges3-Ges4-Ges5),
			writeturnier(Ts,5,0).
		```
	- Turnierplanung-pausen: Gesamt-Pausenzeit aller Manschaften möglichst gleich. Dann Gesamt-Turnierdauer minimal
		```prolog
		hinspiele_PD_eq_opt(Ts, Anz_der_Plaetze,Timeout) :-
			...
			gfd:max([Ges1,Ges2,Ges3,Ges4,Ges5],MaxG),
			gfd:min([Ges1,Ges2,Ges3,Ges4,Ges5],MinG), 
			NegMinG #=(-1)*MinG,
			bb_min(labeling(Ts),NegMinG,_,_,NegMinG,bb_options{timeout:Timeout}),  
			bb_min(labeling(Ts),MaxG,_,_,MaxG,bb_options{timeout:Timeout}),  
			bb_min(labeling(Ts),MaxTs,bb_options{timeout:Timeout}), 
			writeturnier(Ts,5,0),!, writeln(D1:D2:D3:D4:D5), writeln(Ges1-Ges2-Ges3-Ges4-Ges5).
		```
	- Coin-Problem: 1, 2, 5, 10, 20, 50 Cent Münzen. Jew Münzart von Betrag C max 4. Gesucht: Kombi für 99Cent.
		- Ohne Optimierung:
			```prolog
			cent(Betrag, Menge, GesamtMuenzenAnzahl) :-% 
				Betrag::0..99,
				Betrag #= C1 + 2*C2+5*C5+10*C10+20*C20+50*C50,
				Menge = [C1,C2,C5,C10,C20,C50],
				Menge :: 0..4,
				gfd:sum[C1,C2,C5,C10,C20,C50], GesamtMuenzenAnzahl),
				labeling(Menge).
			```
		- Nax abz, Nünzen, maximal möglicher Betrag:
			```prolog
			cent_opt(Betrag, Menge, GesamtMuenzenAnzahl) :-
				...
				sumlist(Menge, GesamtMuenzenAnzahl),
				NegAnz#=(-1)*GesamtMuenzenAnzahl,
				NegBetrag#=(-1)*Betrag,
				bb_min(labeling(Menge),NegAnz,_,_,NegAnz,_),
				bb_min(labeling(Menge),NegBetrag,_).
			```
	- Netzplan: Für größþmögliche X4=X5=X6 kleinstes Vend bestimmen
		```prolog
		netzplan(Horizont, Vs) :-
			Vs=[V1,V2,V3,V4,V5,V6,V7,V8,Vend],
			Vs::0..Horizont,
			V1+7#=<V2, V1+7#=<V3,
			V2+5#=<V4,
			V3+6#=<V5, V3+6#=<V6,
			V4+4#=<V7,
			V5+8#=<V7,
			V6+3#=<V8,
			V7+4#=<Vend,
			V8+6#=<Vend,      
			V4#=V5,V5#=V6, NegV4#=(-1)*V4, 

			bb_min(labeling(Vs),NegV4,V4,V4,_,_),
			bb_min(labeling(Vs),Vend,_). 

			% Für Zeiten des Kritischen Pfades statt obigen bb_min aufrufen
			bb_min(labeling([Vend]),Vend,_). %Bestimmung des kritischen Pfades
			% Die festen Werte in Lösung sind Kritischer Pfad
		?-netzplan(30,Ts).
		```
- <i>Optimierungs-Bsp: Wippe, beide seiten jew 5 plätze, 1 platz in mitte. Ziel Wippe in Gleichgewicht. 3 Personen wiegen A=50,B=40,C=30kg so platzieren, das mindestabstand zw. 2 personen 2 plätze. Positionen der Wippe S::-5..5 , geben jew Faktor an mit dem Gewicht zu multiplizieren ist.</i>
	```prolog
	Wippe(Platzwahl) :- 
		Platzwahl = [A,B,C], 
		[A,B,C}::(-5)..5, % Wertebereich möglichen Sitzpläþze für A,B,C
		50*A + 40*B + 30*C #= 0, % Gleichgewicht sicherstellen
		abs(A-B) #> 2, abs(A-C) #> 2, abs(B-C) #> 2, % Mindestabstände
		labeling(Platzwahl).
	```
	- die abs/1(...) #> 2 Aufrufe können ersetzt werden durch `(A-B #> 2 ; A-B #< -2), % Abstand A,B > 2`
	- Alternativ umsetzung der Abstände mittels cumulative:
		- Die Plätze werden als Dauer (3, da 2 Plätze frei) modelliert; 
		- Die Tatsache, dass nur eine Person auf einem Platz sitzen kann wird durch Ressourcen modelliert.
		- `cumulative([A,B,C],[3,3,3],[1,1,1],1),`
	- Alternativ Umsetzung der Abstände ohne cumulative: `disjunctive([A,B,C], [3,3,3])`
	 	- Da Resourcen ja sowieso alle 1 sind
	- Sollen möglichst weit innen liegende Plätze besetzt werden:
		```prolog
		gfd:max(Platzwahl, Max),
		gfd:min(Platzwahl, Min), NegMin #= (-1) * Min,
		bb_min(labeling(Platzwahl), Max, _, _, Max, _), % Nur Max minimieren und festschreiben
		bb_min(labeling(Platzwahl), NegMin, _). % Min maximieren und alle Variablen Festschreiben
		```
	- 



## Prolog Recursion
- Erste Klausel dient als Abbruchbedingung, nachfolgende Regel(n) führen Operationen aus
- Beispiel: on_route zum prüfen ob man von rome zu einem bestimmten Platz reisen kann. 
	```prolog
	on_route(rome). % Abbruchbedingung

	on_route(Place):- 
		move(Place,Method,NewPlace), % Check if move to somewhere new is possible
		on_route(NewPlace). % Recursively coall on_route. If NewPlace is rome, first clausel will sopp it

	% Possible moves
	move(home,taxi,halifax).
	move(halifax,train,gatwick).
	move(gatwick,plane,rome).
	```
	- Wenn dann Rom bzw die Abbruchbedinugng erreicht wird (welche ja immer als True ausgewertet wird, da da keine Anweisungen die scheidtern könnten enthalten sind), werden alle vorigen Aufrüfe auch als Truae ausgewertet
- Bsp: Z.b. zum überprüfen ob bob größer als Geogre ist. 
	```prolog
	check_is_taller(jim, george). % Abbruchbedingung

	check_is_taller(X,Y):-  
		is_taller(X, Z), % Belegt Z mit dem Name, der kleiner ist als X
		check_is_taller(Z, Y). % Rekursiver aufruf, checkt ob Z größer ist als george  

	is_taller(bob, mike).
	is_taller(mike, jim).
	is_taller(jim, george).
	```

## Hilfreiche Links
- https://gki.informatik.uni-freiburg.de/teaching/ws1415/csp/csp11.pdf 