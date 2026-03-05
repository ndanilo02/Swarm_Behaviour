# ZAPISNIK O RADU NA PROJEKTU

**Student:** Danilo Nikolaš
**Predmet:** Istraživanje podataka 2
**Tema:** Klasifikacija ponašanja rojeva (Swarm Behavior)

---

## FAZA 1: Učitavanje i preprocesiranje podataka
Rad na projektu započet je preuzimanjem tri CSV fajla (*flocking*, *aligned*, *grouped*). U početnoj analizi primećeno je da podaci nisu potpuno čisti, jer su sadržali prazna mesta u imenima kolona, kao i određene nevidljive karaktere. 

Zbog toga su primenjene tehnike čišćenja teksta (`str.strip()`), a sve vrednosti su forsirano konvertovane u numeričke formate pomoću funkcije `pd.to_numeric(errors='coerce')`. Nakon toga, obrisane su sve `NaN` vrednosti, čime je skup podataka uspešno pripremljen za dalji rad.

## FAZA 2: Otkrivanje disbalansa klasa i donošenje ključne odluke
Ispitivanjem ciljne promenljive (`Class`), primećeno je da je fajl *flocking.csv* savršeno balansiran (50:50), dok *grouped* i *aligned* fajlovi imaju blagi disbalans u korist jedne klase (npr. 69:31).

Prvobitna ideja bila je primena **SMOTE** algoritma kako bi se izbalansirale klase generisanjem veštačkih podataka. Međutim, uzimajući u obzir fizičku prirodu ovih podataka, zaključeno je da bi kreiranje "veštačkih" vektora brzina i pozicija za 200 ptica moglo da unese neprirodan šum i naruši stvarna fizička pravila roja. Zbog toga je doneta svesna odluka da se SMOTE ne koristi. Problem disbalansa je rešen primenom stratifikovane podele (`stratify=y`) i oslanjanjem na robusne evaluacione metrike (F1-score, Precision i Matrice konfuzije) umesto na običnu tačnost (Accuracy).

## FAZA 3: Skaliranje i problem različitih mernih opsega
Pre primene modela i redukcije dimenzionalnosti, uočeno je da kolone koje predstavljaju X i Y pozicije imaju potpuno drugačiji opseg vrednosti u poređenju sa kolonama koje predstavljaju vektore brzina (xVel, yVel). S obzirom na to da algoritmi koji mere rastojanje (poput KNN i SVM), kao i sama PCA metoda, zahtevaju uniformne podatke kako ne bi favorizovali atribute sa većim apsolutnim vrednostima, bilo je neophodno izvršiti standardizaciju.

Skaliranje je odrađeno korišćenjem `StandardScaler` klase. Na ovaj način su sve varijable transformisane tako da imaju srednju vrednost nula i standardnu devijaciju jedan, čime je obezbeđen ravnopravan uticaj svih atributa u daljim analizama.

## FAZA 4: Prvi rezultati i uticaj determinističke prirode podataka
Implementirano je pet algoritama (Logistička regresija, KNN, SVM, Random Forest, LightGBM) koji su obučeni na celokupnom skupu od 2400 atributa. Modeli su dali gotovo savršene rezultate sa tačnošću blizu 1.00. 

Inicijalna sumnja u ovakve rezultate je odbačena, jer je zaključeno da je takvo ponašanje logično, s obzirom na to da su podaci generisani iz determinističke kompjuterske simulacije (Boids), kompleksni algoritmi su lako prepoznali i usvojili ta pravila.

## FAZA 5: Dizajniranje PCA "Stres testa"
Kako bi se objektivno procenilo koji algoritam je najrobusniji, dizajniran je i sproveden "Stres test". Pored standardnog zadržavanja 90% varijanse koji daje iste rezultate kao celokupan skup, 2400 kolona je namerno drastično kompresovano na fiksne, ekstremno male brojeve komponenti: 20, 10, 5 i na kraju samo 3 komponente.

Ovaj korak je omogućio sagledavanje pravih razlika između modela. Logistička regresija je značajno izgubila na performansama na 3 komponente jer nije mogla da razdvoji nelinearne podatke u tako malom prostoru. Sa druge strane, modeli poput LightGBM i Random Forest uspeli su da sačuvaju izuzetno visoku moć generalizacije, dokazujući svoju superiornost za modelovanje ove vrste kompleksnih obrazaca.

## FAZA 6: Vizuelizacija rezultata i metrika
U završnoj fazi evaluacije, pristupilo se detaljnoj vizuelizaciji različitih metrika kako bi se omogućilo adekvatno poređenje modela i kako bi se dobijeni grafici lakše integrisali u tekstualni deo izveštaja. Kreirani su bar-plotovi koji upoređuju metrike poput Precision, Recall i F1-score za sve algoritme. 

Takođe, iscrtane su ROC krive koje vizuelno potvrđuju pad performansi Logističke regresije pri ekstremnoj redukciji. Za najbolji model (LightGBM) generisane su matrice konfuzije koje su poslužile kao dokaz da model usled disbalansa ne favorizuje većinsku klasu, potvrđujući time odluku da SMOTE algoritam zaista nije bio potreban.

## FAZA 7: Izrada tekstualne dokumentacije i izveštaja
Nakon tehničke implementacije, uspešno je kreirana finalna tekstualna verzija seminarskog rada pomoću LaTeX-a. Rad je strukturiran tako da logički prati sve faze projekta: od apstrakta i uvoda, preko detaljnog objašnjenja preprocesiranja i PCA redukcije, do analize performansi modela. Svi ključni zaključci, dobijeni iz prethodno generisanih grafičkih vizuelizacija i metrika (ROC krive, matrice konfuzije, bar-plotovi), adekvatno su tekstualno objašnjeni i integrisani u rad.

## FAZA 8: Analiza vremenske kompleksnosti i faktora ubrzanja
Kako bi se dodatno opravdala upotreba PCA kompresije, sprovedena je analiza vremenske kompleksnosti algoritama. Generisani su grafici faktora ubrzanja (Speedup plots) koji upoređuju vreme treniranja na celokupnom skupu (2400 atributa) sa vremenom na redukovanom PCA skupu (455 atributa). Kako je ovo radjeno naknadno na samom kraju, podaci za grafik su uneti ručno, kako se ne bi opet pokretao gornji deo projekta i promenio parametre za modele koji su već bili ubačeni u pdf.
