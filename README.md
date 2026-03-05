## Uputstvo

Podaci korišćeni u ovom projektu potiču sa UCI Machine Learning repozitorijuma (Swarm Behaviour Dataset). Zbog ograničenja veličine fajlova na GitHub-u, podaci se ne nalaze u samom repozitorijumu i moraju se preuzeti ručno.

**Uputstvo za preuzimanje i postavljanje podataka:**

1. Preuzmite dataset u ZIP formatu putem ovog linka: [Swarm Behaviour Dataset (ZIP)](https://archive.ics.uci.edu/static/public/524/swarm+behaviour.zip)
2. Otpakujte preuzetu arhivu na svom računaru.
3. U glavnom folderu ovog projekta napravite novi folder i nazovite ga tačno **`data`**.
4. Prekopirajte sve otpakovane `.csv` fajlove u taj `data` folder.

Vaša struktura fajlova pre pokretanja koda bi trebalo da izgleda ovako:
```text
Swarm_Behaviour/
├── data/                  # <-- Ovde idu preuzeti CSV fajlovi
│   ├── Aligned.csv
│   ├── Flocking.csv
│   └── Grouped.csv
├── src/                 
│   ├── aligned.ipynb
│   ├── flocking.ipynb
│   └── grouped.ipynb
├── requirements.txt
└── .gitignore
```
**Koraci za instalaciju i pokretanje:**

Za pokretanje ovog projekta potrebno je da imate instaliran **Python** na svom računaru.

1. **Klonirajte repozitorijum** i pozicionirajte se u glavni folder projekta:
   ```bash
   git clone https://github.com/ndanilo02/Swarm_Behaviour.git
   cd Swarm_Behaviour
   
2. **Instalacija potrebne biblioteke**:
   pip install -r requirements.txt