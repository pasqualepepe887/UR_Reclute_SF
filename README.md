# UR_Reclute_SF
Progetto Reclute Unina Rocket - Software

# Shield Monitoraggio Batteria 12V e GPS GNSS 
Questo progetto consiste in un sistema di monitoraggio basato su STM32F401RE progettato per acquisire dati di posizione globale e monitorare la tensione di una sorgente esterna a 12V. Il sistema integra un modulo Teseo-LIV4F e una logica di gestione energetica basata su stati.

## Caratteristiche Principali
* Monitoraggio Tensione: Lettura costante della batteria tramite ADC con partitore di tensione.

* Posizionamento GNSS: Ricezione di stringhe NMEA 0183 dal modulo Teseo-LIV4F.

* Efficienza Energetica: Gestione dello standby software del modulo GPS quando non in uso.

* Interfaccia Utente: Switch fisico per il cambio modalità e LED diagnostico di stato.

## Configurazione Hardware (Pinout)
Lo Shield utilizza la seguente mappatura dei pin sulla scheda Nucleo-F401RE:

<img width="640" height="587" alt="image" src="https://github.com/user-attachments/assets/718ab38d-3d69-4b0e-ba6b-0cbefa771d21" />

## Logica di Funzionamento
Il firmware implementa una macchina a stati finiti gestita dall'interruttore su PB3:
1) **Modalità WAIT**:

* Il LED su PA6 rimane fisso.

* Viene inviato il comando $PSTMSTBY al Teseo per ridurre i consumi.

* La console stampa solo il voltaggio della batteria.

---------------------------------------------------------
  
2) **Modalità RECEIVE**:

* Il LED su PA6 lampeggia.

* Il modulo Teseo viene risvegliato e i dati NMEA vengono acquisiti via USART1.

* La console mostra sia il voltaggio che i dati geografici.

## Schema del Partitore di Tensione
Il calcolo del voltaggio reale nel codice è basato su un partitore con rapporto 1:5:
* R1: 40kΩ (collegato ai 12V).
* R2: 10kΩ (collegato a GND).
* V_out: Collegato al pin PA7.
---
$$V_{reale} = V_{letto} \cdot \frac{R1 + R2}{R2} = V_{letto} \cdot 5$$
