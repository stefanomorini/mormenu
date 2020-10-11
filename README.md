Versione 1.3 stable


Ormai dimenticato e oggi riscoperto lo ripropongo con alcune istruzioni per l'uso e la configurazione del file .mormenu .

Il programma era stato ideato anni fa per lanciare programmi di sistema con l'agevolazione di un menu semigrafico ricorsivo.
Veniva creato uno utente su Unix e nel .profile l'ultimo comando era mormenu dove nella home dell'utente c'era il file di configurazione $HOME/.mormenu .

Programma partendo da una configurazione precisa di simboli crea la semigrafica per i terminali vt100,ansi e xterm (La variabile TERM dev'essere settata per ottenere la semigrafica) per il lancio di programmi di sistema operativo.

   Schema di conversione
       =  &
   !+-----+-----+@
   ||     |     |
    |     |+    |
   ^+-----+-----+*
    |     |     |
    |     |     |
   #+-----+-----+$
          %


Il file di menu è strutturato in tre sezioni:
	- la prima parte con la semigrafica del menu, 
	- una seconda parte per l' HELP
	- una terza parte per il lancio dei comandi: singolo comando, comando con domande  

La parte comandi include è un record di cinque campi delimitati dai due punti:
		Campi:  Descrizione
		1    :  selezione del menu si singola lettera o numero
		2    : Tipo di azine da intraprendere possono essere sommate tramite il simbolo + sh+stop
			sh:	Shell
			stop:   Effettua una richiesta
			rs:    	Reset del terminale
			>:	Menu successivo
			<:   	menu precedente 
			Ulteriori azioni sono richiamabili ovunque con queste lettere riservate (unliteral).
			H = HELP; Q = Quit; P = Menu precedente 
		3    : riservato per uso futuro
		4    : riservato per uso futuro
		5    : Domand da effettuare es: Instroduci l'indirizzo ip
		6    : Shell lanciata  in caso di una richiesta il %s carica in quella posizione il dato immesso
		Esempio:  2:sh+stop:::Host remoto:ping %s
				qunado l'utente batte il 2 viene chiamato questo comando.
				viene richiesto a video di immettere un dato alla domand "Host remoto" es 127.0.0.1
				viene seguito il comando ping 127.0.0.1 

** Segue l'sempio di menu da collocare in $HOME/.mormenu

### $HOME/.mormenu (Esempio di file di configurazione di mormenu).
### I commenti per essere validi devono avere i '#' da colonna 1 a 3 come qui 
###
###   Schema di conversione
###       =  &
###   !+-----+-----+@
###   ||     |     |
###    |     |+    |
###   ^+-----+-----+*
###    |     |     |
###    |     |     |
###   #+-----+-----+$
###          %

)MainMenu
Menu Principale
!===================================@
| M&M Computers       Rel 1.0       |
^===================================+======================================@
|  Applicazioni                     |         Sottomenu                    | 
^=========================&=========+======================================*
|  0 . shell utente       |         |  S. Gestione Spooler                 |
|  1 . Gest. copie        | H. Help |                                      |
|    .                    |         |  U. Utility                          |
|    .                    #=========*                                      | 
|  Q . Quit                         |                                      | 
#===================================%======================================$
))
0 - Lancia una Shell.
1 - Lancio menu copie (user copie su - copie).
S - Gestione delle code di stampa. Lista delle code, rimozione delle code con
	svuotamento automatico buffer emulex. Gestione e amministrazione delle 
	stampanti.
U - Utility (top, ping, netstat )
)))
0:sh::::/bin/sh
S:sh::::/usr/smc/etc/spooler.sh
1:sh::::su - copie
U:>::::Utility
Q:Q::::
))))

)Utility
Menu Utility
!===================================@
| M&M Computers (BO)                |
^===================================+======================================@
|  1 . top                          |                                      |
|  2 . ping                         |                                      |
|  3 . netstat                      |                                      |
|  4 . tavole di route              |                                      |
|  5 . sysadm                       |                                      |
|  P . Menu precedente              |                                      | 
|  Q . Quit                         |                                      | 
#===================================%======================================$
))
1 - Loadaverrage 
2 - ping su host predefinito 
4 - stato dell' interfaccia ethernet (netstat -i). 
4 - tavole di instradamento tcp/ip   (netstat -r). 
5 - sysadm - utility dell' amminitratore (su root -c "sysadm")
)))
1:sh::::top
2:sh+stop:::Host remoto:ping %s
3:sh+stop::::netstat -i
4:sh+stop::::netstat -r
5:sh::::su root -c "sysadm"
Q:Q::::
))))
