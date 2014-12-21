--1
SELECT * FROM pracownik

SELECT imie FROM pracownik

SELECT imie, nazwisko, dzial FROM pracownik

--2
SELECT imie, nazwisko, pensja FROM pracownik
ORDER BY pensja DESC

SELECT imie, nazwisko FROM pracownik
ORDER BY nazwisko, imie

SELECT nazwisko, dzial, stanowisko FROM pracownik
ORDER BY dzial, stanowisko DESC

--3
SELECT DISTINCT dzial FROM pracownik

SELECT DISTINCT dzial, stanowisko FROM pracownik

SELECT DISTINCT dzial, stanowisko FROM pracownik
ORDER BY dzial DESC, stanowisko DESC

--4
SELECT imie, nazwisko FROM pracownik
WHERE imie='Jan'

SELECT imie, nazwisko FROM pracownik
WHERE stanowisko='sprzedawca'

SELECT imie, nazwisko, pensja FROM pracownik
WHERE pensja>1500
ORDER BY pensja DESC

--5
SELECT imie, nazwisko, dzial, stanowisko FROM pracownik
WHERE dzial='obsługa klienta' AND stanowisko='sprzedawca'

SELECT imie, nazwisko, dzial, stanowisko FROM pracownik
WHERE dzial='techniczny' AND (stanowisko='kierownik' OR stanowisko='sprzedawca')

SELECT * FROM samochod
WHERE NOT marka='fiat' AND NOT marka='ford'

--6
SELECT * FROM samochod 
WHERE marka IN ('mercedes', 'seat', 'opel')

SELECT imie, nazwisko, data_zatr FROM pracownik
WHERE imie IN ('Anna', 'Marzena', 'Alicja')

SELECT imie, nazwisko, miasto FROM klient
WHERE miasto NOT IN('Warszawa', 'Wrocław')

--7

SELECT imie, nazwisko FROM klient
WHERE nazwisko LIKE 'k%'

SELECT imie, nazwisko FROM klient
WHERE nazwisko LIKE 'D%ski'

SELECT imie, nazwisko FROM klient
WHERE nazwisko LIKE '_o%' OR nazwisko LIKE '_a%'

--8
SELECT * FROM samochod
WHERE poj_silnika BETWEEN 1100 AND 1600

SELECT * FROM pracownik
WHERE data_zatr BETWEEN '1977-01-01' AND '1997-12-31'

SELECT * FROM samochod
WHERE przebieg BETWEEN 10000 AND 20000 OR przebieg BETWEEN 30000 AND 40000

--9
SELECT * FROM pracownik
WHERE dodatek IS NULL

SELECT * FROM klient
WHERE nr_karty_kredyt IS NOT NULL

SELECT imie, nazwisko, COALESCE(dodatek,0) AS dodatek FROM pracownik

--10
SELECT imie, nazwisko, pensja, COALESCE(dodatek,0) AS dodatek, pensja+(COALESCE(dodatek,0)) AS do_zaplaty FROM pracownik

SELECT imie, nazwisko, pensja+0.5*pensja AS nowa_pensja FROM pracownik

SELECT imie, nazwisko, 0.01*(pensja+COALESCE(dodatek,0)) AS procent FROM pracownik
ORDER BY procent

--11
SELECT TOP 1 imie, nazwisko FROM pracownik
ORDER BY data_zatr

SELECT TOP 5 imie, nazwisko FROM pracownik
ORDER BY nazwisko, imie

SELECT TOP 1 * FROM wypozyczenie
ORDER BY data_wyp DESC

--12
SELECT imie, nazwisko, data_zatr FROM pracownik
WHERE MONTH(data_zatr)=5

SELECT imie, nazwisko, DATEDIFF(d,data_zatr,GETDATE()) AS dni FROM pracownik
ORDER BY dni DESC

SELECT *, DATEDIFF(yy,data_prod,GETDATE()) AS lata FROM samochod
ORDER BY lata DESC

--13
SELECT imie, nazwisko, LEFT(imie,1)+LEFT(nazwisko,1) AS inicjał FROM klient
ORDER BY inicjał, nazwisko, imie

SELECT (UPPER(LEFT(imie,1)))+(LOWER(RIGHT(imie,LEN(imie)-1))) AS imie,(UPPER(LEFT(nazwisko,1)))+(LOWER(RIGHT(nazwisko,LEN(nazwisko)-1))) AS nazwisko FROM klient

SELECT imie, nazwisko, STUFF(nr_karty_kredyt,LEN(nr_karty_kredyt)-5,6,'xxxxxx') FROM klient
SELECT imie, nazwisko, nr_karty_kredyt FROM klient

--14
UPDATE pracownik
SET dodatek=50
WHERE dodatek IS NULL

UPDATE klient
SET imie='Jerzy', nazwisko='Nowak'
WHERE id_klient=10

UPDATE pracownik
SET dodatek=dodatek+100
WHERE pensja<1500

--15
DELETE FROM klient
WHERE id_klient=17

DELETE FROM wypozyczenie
WHERE id_klient=17

DELETE FROM samochod
WHERE przebieg>60000

--16
INSERT INTO klient(id_klient,imie,nazwisko,ulica,numer,miasto,kod,telefon)
VALUES (121,'Adam','Cichy','Korzenna','12','Warszawa','00-950','123-454-321')

INSERT INTO samochod
VALUES (50,'Skoda','Octavia','2012-09-01','srebrny',1896,5000)

INSERT INTO pracownik(id_pracownik,imie,nazwisko,data_zatr,dzial,stanowisko,pensja,dodatek,id_miejsce,telefon)
VALUES (12,'Alojzy','Mikos','2010-08-11','zaopatrzenie','magazynier',3000,50,1,'501-501-501')

--17
SELECT s.id_samochod, s.marka, s.typ, w.data_wyp, w.data_odd FROM samochod s INNER JOIN wypozyczenie w ON s.id_samochod=w.id_samochod
WHERE w.data_odd IS NULL

SELECT k.imie, k.nazwisko, w.id_samochod, w.data_wyp FROM klient k INNER JOIN wypozyczenie w ON k.id_klient=w.id_klient
WHERE w.data_odd IS NULL
ORDER BY k.nazwisko, k.imie

SELECT k.imie, k.nazwisko, w.data_wyp, w.kaucja FROM klient k INNER JOIN wypozyczenie w ON k.id_klient=w.id_klient
WHERE w.kaucja IS NOT NULL

--18
SELECT k.imie, k.nazwisko, w.data_wyp, s.marka, s.typ FROM klient k INNER JOIN wypozyczenie w ON k.id_klient=w.id_klient INNER JOIN samochod s ON w.id_samochod=s.id_samochod
ORDER BY k.nazwisko, k.imie, s.marka, s.typ

SELECT DISTINCT m.ulica, m.numer, s.marka, s.typ FROM miejsce m INNER JOIN wypozyczenie w ON m.id_miejsce=w.id_miejsca_wyp INNER JOIN samochod s ON w.id_samochod=s.id_samochod

SELECT DISTINCT s.id_samochod, s.marka, s.typ, k.imie, k.nazwisko FROM samochod s INNER JOIN wypozyczenie w ON s.id_samochod=w.id_samochod INNER JOIN klient k ON w.id_klient=k.id_klient
ORDER BY s.id_samochod, k.nazwisko, k.imie

--19
SELECT MAX(pensja) FROM pracownik

SELECT AVG(pensja) FROM pracownik

SELECT MIN(data_prod) FROM samochod

--20
SELECT k.imie, k.nazwisko, COUNT(w.data_wyp) AS wypozyczenia FROM klient k LEFT JOIN wypozyczenie w ON k.id_klient=w.id_klient
GROUP BY k.imie, k.nazwisko, k.id_klient
ORDER BY wypozyczenia DESC

SELECT s.id_samochod, s.marka, s.typ, COUNT(w.data_wyp) AS wypozyczenia FROM samochod s LEFT JOIN wypozyczenie w ON s.id_samochod=w.id_samochod
GROUP BY s.id_samochod, s.marka, s.typ
ORDER BY wypozyczenia

SELECT p.imie, p.nazwisko, COUNT(w.data_wyp) AS wypozyczenia FROM pracownik p LEFT JOIN wypozyczenie w ON p.id_pracownik=w.id_pracow_wyp
GROUP BY p.imie, p.nazwisko, p.id_pracownik
ORDER BY wypozyczenia DESC

--21
SELECT k.imie, k.nazwisko, COUNT(w.data_wyp) AS wypozyczenia FROM klient k JOIN wypozyczenie w ON k.id_klient=w.id_klient
GROUP BY k.imie, k.nazwisko, k.id_klient
HAVING COUNT(w.data_wyp)>1
ORDER BY k.nazwisko, k.imie

SELECT s.id_samochod, s.marka, s.typ, COUNT(w.data_wyp) AS wypozyczenia FROM samochod s LEFT JOIN wypozyczenie w ON s.id_samochod=w.id_samochod
GROUP BY s.id_samochod, s.marka, s.typ
HAVING COUNT(w.data_wyp)>=5
ORDER BY marka, typ

SELECT p.imie, p.nazwisko, COUNT(w.data_wyp) AS wypozyczenia FROM pracownik p LEFT JOIN wypozyczenie w ON p.id_pracownik=w.id_pracow_wyp
GROUP BY p.imie, p.nazwisko, p.id_pracownik
HAVING COUNT(w.data_wyp)<=20
ORDER BY wypozyczenia, nazwisko, imie

--22
SELECT imie, nazwisko, pensja FROM pracownik
WHERE pensja=(SELECT MAX(pensja) FROM PRACOWNIK)

SELECT imie, nazwisko, pensja FROM pracownik
WHERE pensja>(SELECT AVG(pensja) FROM PRACOWNIK)

SELECT marka, typ, data_prod FROM samochod
WHERE data_prod=(SELECT MIN(data_prod) FROM samochod)

--23
SELECT marka, typ, data_prod FROM samochod
WHERE id_samochod NOT IN (SELECT DISTINCT id_samochod FROM wypozyczenie)

SELECT imie, nazwisko FROM klient
WHERE id_klient NOT IN (SELECT DISTINCT id_klient FROM wypozyczenie)
ORDER BY nazwisko, imie

SELECT imie, nazwisko FROM pracownik
WHERE id_pracownik NOT IN (SELECT id_pracow_wyp FROM wypozyczenie)

--24
SELECT s.id_samochod, s.marka, s.typ FROM samochod s INNER JOIN wypozyczenie w ON s.id_samochod=w.id_samochod
GROUP BY s.id_samochod, s.marka, s.typ
HAVING COUNT(w.id_samochod)=(SELECT TOP 1 WITH TIES COUNT(w.id_samochod) AS ilosc FROM wypozyczenie w
GROUP BY w.id_samochod
ORDER BY ilosc DESC)
ORDER BY marka, typ

SELECT k.id_klient, k.imie, k.nazwisko FROM klient k INNER JOIN wypozyczenie w ON k.id_klient=w.id_klient
GROUP BY k.id_klient, k.imie, k.nazwisko
HAVING COUNT(w.id_klient)=(SELECT TOP 1 WITH TIES COUNT(w.id_klient) AS ilosc FROM wypozyczenie w
GROUP BY w.id_klient
ORDER BY ilosc)
ORDER BY nazwisko, imie

SELECT p.id_pracownik, p.imie, p.nazwisko FROM pracownik p INNER JOIN wypozyczenie w ON p.id_pracownik=w.id_pracow_wyp
GROUP BY p.id_pracownik, p.imie, p.nazwisko
HAVING COUNT(w.id_pracow_wyp)=(SELECT TOP 1 WITH TIES COUNT(w.id_pracow_wyp) AS ilosc FROM wypozyczenie w
GROUP BY w.id_pracow_wyp
ORDER BY w.id_pracow_wyp)
ORDER BY nazwisko, imie


--25
UPDATE pracownik
SET pensja=(pensja+0.1*pensja)
WHERE pensja<(SELECT AVG(pensja) FROM pracownik)

UPDATE pracownik
SET dodatek=(dodatek+10)
WHERE id_pracownik IN (SELECT DISTINCT id_pracow_wyp FROM wypozyczenie WHERE MONTH(data_wyp)=05)
