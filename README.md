# LampaReglabila - Variator de lumină (Dimmer PWM) cu STM32 și FreeRTOS

Proiect realizat pentru disciplina de sisteme de timp real / microcontrollere.

## Descriere
Proiectul constă în realizarea unui variator inteligent de lumină (dimmer) bazat pe microcontrollerul **STM32F401RE (arhitectură ARM Cortex-M4)** și sistemul de operare de timp real **FreeRTOS**.

Sistemul permite reglarea lină a intensității luminoase a unui LED în două moduri:
1. **Control local (manual):** Citirea poziției unui potențiometru analogic.
2. **Control la distanță:** Trimitere de comenzi și recepție de date prin modulul Bluetooth HC-05.

---

## Arhitectură Software (FreeRTOS)
Aplicația utilizează două task-uri paralele sincronizate:

* **Task 1 (Timp real - Control local):** Eșantionează periodic intrarea analogică (potențiometru) prin intermediul **ADC-ului intern** și actualizează factorul de umplere pentru semnalul **PWM** generat de un **timer intern**, modificând luminozitatea LED-ului în timp real.
* **Task 2 (Comunicație - Bluetooth):** Gestionează interfața **USART internă** conectată la modulul Bluetooth HC-05. Transmite starea curentă a luminii către telefon și preia comenzile de suprascriere trimise de utilizator.

### Sincronizare și Resurse Partajate
* **xQueue:** Transferă datele despre intensitatea luminoasă între Task 1 și Task 2.
* **Mutex:** Protejează starea sistemului pentru a preveni condițiile de concurență între comanda manuală și comanda de la distanță.

---

## Periferice STM32 Utilizate
* **ADC (Analog-to-Digital Converter):** Conversia tensiunii analogice de la potențiometru.
* **TIM (Timer în mod PWM):** Generarea semnalului modulat în durată pentru controlul LED-ului.
* **USART (Universal Synchronous/Asynchronous Receiver-Transmitter):** Comunicația serială cu modulul Bluetooth.

---

## Componente Hardware
* Placă de dezvoltare **STM32 Nucleo-F401RE**
* Potențiometru 10k $\Omega$
* Modul Bluetooth **HC-05**
* LED (integrat pe placă `PA5` sau extern)
* Fire de conexiune (Dupont)

---

## Structura Repozitoriului
* `Core/Src/` - Codul sursă `main.c` și task-urile FreeRTOS.
* `Core/Inc/` - Fișierele header ale proiectului.
* `README.md` - Documentația proiectului.
