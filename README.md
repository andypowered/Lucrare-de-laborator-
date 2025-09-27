# Fișă de Laborator: Testarea Aplicațiilor Web (E-commerce)

**Tema:** Obiectivele, Principiile și Axiomele Testării
**Scenariu:** Aplicație web de tip e-commerce pentru vânzări online.

## Cuprins
1.  [Sarcina 1 – Obiectivele Testării](#sarcina-1--obiectivele-testării)
2.  [Sarcina 2 – Principiile Testării](#sarcina-2--principiile-testării)
3.  [Sarcina 3 – Axiomele Testării](#sarcina-3--axiomele-testării)
4.  [Sarcina 4 – Mini-Caz Practic (Eroare 500)](#sarcina-4--mini-caz-practic-eroare-500)
5.  [Sarcina 5 – Crearea Planului de Testare](#sarcina-5--crearea-planului-de-testare)
6.  [Sarcina 6 – Teste Pozitive și Negative](#sarcina-6--teste-pozitive-și-negative)
7.  [Sarcina 7 – Clasificarea Testelor](#sarcina-7--clasificarea-testelor)
8.  [Sarcina 8 – Analiza Riscurilor](#sarcina-8--analiza-riscurilor)

---

## Sarcina 1 – Obiectivele Testării
**Sarcină:** Formulează 5 obiective concrete de testare pentru aplicația e-commerce.

**Răspuns:**
1.  **Verificarea funcționalității cheie:** Să se confirme că utilizatorii se pot înregistra, autentifica, căuta produse, adăuga produse în coș și finaliza comanda fără erori.
2.  **Asigurarea securității datelor:** Să se identifice vulnerabilități (precum SQL Injection sau XSS) care ar putea compromite datele utilizatorilor (date personale, parole, detalii de plată).
3.  **Validarea performanței și a răspunsului:** Să se testeze că aplicația răspunde rapid (sub 3 secunde) chiar și în condiții de sarcină mare (ex: sute de utilizatori simultan care adaugă produse în coș).
4.  **Verificarea integrității datelor:** Să se confirme că informațiile despre produse (preț, stoc, descriere) sunt afișate corect și că comenzile sunt procesate și salvate exact în baza de date.
5.  **Asigurarea compatibilității:** Să se testeze că aplicația funcționează corect pe diferite browsere (Chrome, Firefox, Safari) și dispozitive (desktop, tabletă, mobil).

## Sarcina 2 – Principiile Testării
**Sarcină:** Asociază fiecare principiu cu o situație practică din scenariul e-commerce.

**Răspuns:**

| Principiu | Situație Practică | Explicație |
| :--- | :--- | :--- |
| 1. Testarea arată prezența defectelor, nu absența lor. | Toate testele trec, dar un client descoperă că dacă adaugă un produs cu un simbol special (e.g., `@`) în coș, cantitatea nu se actualizează. | Testele noastre nu au acoperit acest caz specific. Găsirea unui defect dovedește existența problemelor, dar trecerea testelor nu garantează că nu mai există niciunul. |
| 2. Testarea completă este imposibilă. | Este imposibil să testezi toate combinațiile de căutare (fiecare cuvânt posibil), toate comenzile cu toate produsele și toate variantele de date de plată. | Numărul de scenarii de testare este infinit. Ne concentrăm pe teste bazate pe risc și pe cazurile cele mai probabile de utilizare. |
| 3. Defectele se grupează. | Majoritatea problemelor sunt găsite în modulul de procesare a plăților și în funcția complexă de filtrare a produselor. | Modulele complexe sau modificate recent au o probabilitate mai mare de a conține defecte. Testarea trebuie să se concentreze pe aceste "puncte fierbinți". |
| 4. Paradoxul pesticidei. | Dacă rulăm mereu același set de teste pentru login, acestea vor trece, dar nu vor găsi noi vulnerabilități de securitate introduse prin alte funcții. | Asemenea pesticidelor care devin inutile, testele repetitive devin ineficiente în găsirea unor defecte noi. Setul de teste trebuie actualizat și diversificat regulat. |

## Sarcina 3 – Axiomele Testării
**Sarcină:** Explică următoarele axiome în contextul aplicației.

**Răspuns:**
1.  **„Testarea trebuie planificată.” – ce riscuri apar dacă testarea se face haotic, după ce aplicația a fost deja lansată?**
    *   **Riscuri:** Aplicația este lansată cu defecte critice (de ex., pierdere de comenzi, scurgeri de date) care ar fi putut fi prevenite. Lipsa unui plan duce la:
        *   **Acoperire incompletă:** Funcții importante (plăți, securitate) sunt neglijate.
        *   **Imposibilitatea de a estima efortul:** Nu știm când am terminat de testat.
        *   **Costuri majore de remediere:** Corectarea unui bug în producție este mult mai costisitoare decât în faza de dezvoltare.

2.  **„Testarea nu poate dovedi că sistemul este fără defecte.” – dă un exemplu de situație unde, deși testele trec, utilizatorii găsesc bug-uri.**
    *   **Exemplu:** Testele de căutare verifică cuvinte cheie normale. Un utilizator încearcă să caute un produs folosind un șir lung de 1000 de caractere. Aplicația nu afișează o eroare, dar în fundal, interogarea SQL încearcă să proceseze acest șir lung, cauzând o cădere a performanței pentru alți utilizatori sau chiar o eroare internă. Acest caz "de limită" nu a fost acoperit de teste.

3.  **„Testarea este dependentă de context.” – explică diferența de abordare între testarea aplicației e-commerce și a unui forum educațional.**
    *   **E-commerce:** Testarea se concentrează **maxim** pe **securitate** (protecția datelor cu carduri, prevenirea fraudelor), **performanță** (încărcare rapidă pentru a nu pierde clienți) și **fiabilitatea tranzacțiilor** (nicio comandă nu trebuie pierdută).
    *   **Forum educațional:** Testarea se concentrează mai mult pe **ușurința în utilizare**, **funcționalitatea de colaborare** (atașarea fișierelor, formatarea textului) și **managementul conținutului**. Securitatea este importantă, dar riscul financiar direct este mai mic.

## Sarcina 4 – Mini-Caz Practic (Eroare 500)
**Situație:** Aplicația returnează eroarea `500 Internal Server Error` când utilizatorul introduce `DROP TABLE` în câmpul „nume produs”.

1.  **Identifică ce principiu și ce axiomă se aplică aici.**
    *   **Principiu:** **Testarea arată prezența defectelor.** Am descoperit un defect de securitate critic (Vulnerabilitate SQL Injection) prin testare.
    *   **Axiomă:** **Testarea este dependentă de context.** Într-o aplicație e-commerce care procesează date sensibile, testarea de securitate (inclusiv a prevenției SQL Injection) este absolut esențială și prioritară.

2.  **Propune două metode de testare suplimentară pentru prevenirea acestui tip de erori.**
    *   **Testare de securitate cu instrumente automate:** Utilizarea unui scanner de securitate (e.g., OWASP ZAP, SQLMap) pentru a scana automat toate câmpurile de intrare ale aplicației pentru vulnerabilități SQL Injection și XSS.
    *   **Testare de pălărie neagră (Penetration Testing):** Simularea atacurilor unui hacker pentru a încerca să exploateze câmpurile de input, nu doar cu `DROP TABLE`, ci și cu alte comenzi malitioase (e.g., `' OR '1'='1`).

## Sarcina 5 – Crearea Planului de Testare
**Mini-plan de testare pentru aplicația e-commerce:**

*   **Obiectivele Testării:** (Vezi Sarcina 1)
*   **Tipurile de Teste:**
    *   **Teste Funcționale:** Înregistrare, login, căutare, adăugare în coș, checkout, gestionare cont.
    *   **Teste de Securitate:** SQL Injection, XSS, autentificare și autorizare, securitatea plăților.
    *   **Teste de Performanță:** Teste de încărcare și stres cu până la 1000 de utilizatori simultan.
    *   **Teste de Compatibilitate:** Testare pe Chrome, Firefox, Safari, Edge și pe dispozitive mobile.
*   **Criterii de Intrare (Când începe testarea):**
    *   Toate funcționalitățile majore sunt implementate și considerate "gata" de dezvoltatori.
    *   Este disponibil un mediu de testare identic cu cel de producție.
*   **Criterii de Ieșire (Când se termină testarea):**
    *   Toate testele critice și de prioritate înaltă au fost rulate și au trecut.
    *   Toate defectele critice și majore au fost rezolvate și retestate.
    *   Managerul de proiect și product owner-ul aprobă lansarea pe baza raportului de testare.

## Sarcina 6 – Teste Pozitive și Negative
**Modulul: „Adăugare produs în coș”**

| Tip Test | Descriere Test | Rezultat Așteptat |
| :--- | :--- | :--- |
| **Pozitiv 1** | Utilizatorul autentificat adaugă un produs care este în stoc. | Produsul apare în coș cu cantitatea 1. Totalul coșului se actualizează corect. |
| **Pozitiv 2** | Utilizatorul adaugă același produs de două ori. | Cantitatea produsului în coș devine 2. Totalul se calculează corect (preț x 2). |
| **Negativ 1** | Utilizatorul încearcă să adauge un produs care nu este în stoc (stoc=0). | Sistemul afișează mesajul "Produs indisponibil" și nu permite adăugarea în coș. |
| **Negativ 2** | Utilizatorul neautentificat încearcă să adauge un produs în coș. | Sistemul redirecționează utilizatorul către pagina de login. După logare, produsul este adăugat în coș. |

## Sarcina 7 – Clasificarea Testelor
**Modulul: „Căutare produse”**

| Test | Clasificare | Explicație |
| :--- | :--- | :--- |
| 1. Căutarea după „Laptop Dell”. | **Pozitiv** | Este un caz de utilizare normal și așteptat. Sistemul ar trebui să afișeze produse relevante. |
| 2. Căutarea după un număr foarte mare: „999999999999”. | **Negativ** | Este un caz de limită. Sistemul ar trebui să gestioneze acest input fie prin afișarea unui mesaj "Niciun rezultat", fie prin afișarea corectă a listei goale, fără să se defecteze. |
| 3. Căutarea după șir gol („”). | **Negativ** | Câmpul este gol. Sistemul ar trebui să afișeze toate produsele sau un mesaj care să îndrume utilizatorul, dar nu să genereze o eroare. |
| 4. Căutarea după „Mouse wireless”. | **Pozitiv** | La fel ca testul 1, este un scenariu de utilizare valid și așteptat. |

## Sarcina 8 – Analiza Riscurilor
**Riscuri majore și strategii de testare:**

| Riscuri | Strategie de Testare |
| :--- | :--- |
| **1. Funcționalitate: Procesarea incorectă a comenzilor.** (Risc: Pierdere de venituri, clienți nemulțumiți) | **Testare end-to-end riguroasă.** Se vor crea teste automate care simulează întregul flux de la adăugarea în coș până la primirea confirmării comenzii și a emailului. Se vor verifica în baza de date că comanda este salvată cu statusul corect. |
| **2. Securitate: Breșă de securitate care expune datele clienților.** (Risc: Pierdere de încredere, amenzi legale) | **Testare intensivă de securitate.** Combinarea testării manuale (pentru logica de business) cu scanarea automată a vulnerabilităților (OWASP ZAP). Testarea va include atacuri SQL Injection, XSS și verificarea controlului accesului la resurse. |
| **3. Performanță: Aplicația devine lentă sau indisponibilă în perioadele de vârf (ex: Black Friday).** (Risc: Pierdere de clienți, venituri nefinalizate) | **Testare de încărcare și stres.** Simularea unui număr realist de utilizatori simultani care efectuează acțiuni critice (căutare, adăugare în coș, checkout). Monitorizarea timpului de răspuns și a consumului de resurse pentru a identifica punctele slabe.
