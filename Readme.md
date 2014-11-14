Bazy danych Microsoft SQL server

--1.1
SELECT * FROM pracownik
--1.2
SELECT imie FROM pracownik
--1.3
SELECT imie, nazwisko, dzial FROM pracownik

--2.1
SELECT imie, nazwisko, pensja FROM pracownik ORDER BY pensja DESC
--2.2
SELECT imie, nazwisko FROM pracownik ORDER BY nazwisko, imie
--2.3
SELECT nazwisko, dzial, stanowisko FROM pracownik ORDER BY dzial ASC, stanowisko DESC

--3.1
SELECT DISTINCT dzial FROM pracownik
--3.2
SELECT DISTINCT dzial, stanowisko FROM pracownik
--3.3
SELECT DISTINCT dzial, stanowisko FROM pracownik ORDER BY dzial DESC, stanowisko

--4.1
SELECT imie, nazwisko FROM pracownik WHERE imie='JAN'
--4.2
SELECT imie, nazwisko FROM pracownik WHERE stanowisko='sprzedawca'
--4.3
SELECT imie, nazwisko, pensja FROM pracownik WHERE pensja>1500 ORDER BY pensja DESC

--5.1
SELECT imie, nazwisko, dzial, stanowisko FROM pracownik WHERE dzial='obsługa klienta' AND stanowisko='sprzedawca'
--5.2
SELECT imie, nazwisko, dzial, stanowisko FROM pracownik WHERE dzial='techniczny' AND (stanowisko='kierownik' OR stanowisko='sprzedawca')
--5.3
SELECT * FROM samochod WHERE marka!='fiat' AND marka!='ford'

--6.1
SELECT * FROM samochod WHERE marka IN ('mercedes', 'seat', 'opel')
--6.2
SELECT imie, nazwisko, data_zatr FROM pracownik WHERE imie IN ('Anna', 'Marzena', 'Alicja')
--6.3
SELECT imie, nazwisko, miasto FROM klient WHERE miasto NOT IN ('Wrocław', 'Warszawa')

--7.1
SELECT imie, nazwisko FROM klient WHERE nazwisko LIKE 'K%'
--7.2
SELECT imie, nazwisko FROM klient WHERE nazwisko LIKE 'D%SKI'
--7.3
SELECT imie, nazwisko FROM klient WHERE nazwisko LIKE '_[OA]%'

--8.1
SELECT * FROM samochod WHERE poj_silnika BETWEEN 1100 AND 1600
--8.2
SELECT * FROM pracownik WHERE data_zatr BETWEEN '1997-01-01' AND '1997-12-31'
--8.3
SELECT * FROM samochod WHERE przebieg BETWEEN 10000 AND 20000 OR przebieg BETWEEN 30000 AND 40000

--9.1
SELECT * FROM pracownik WHERE dodatek IS NULL
--9.2
SELECT * FROM klient WHERE nr_karty_kredyt IS NOT NULL
--9.3
SELECT imie, nazwisko, COALESCE(dodatek,'0') AS dodatek FROM pracownik

--10.1
SELECT imie, nazwisko, pensja, COALESCE(dodatek,'0') AS dodatek, COALESCE(dodatek,'0')+pensja AS do_zaplaty FROM pracownik
--10.2
SELECT imie, nazwisko, CAST(pensja*1.5 AS DECIMAL(8,2)) AS nowa_pensja FROM pracownik
--10.3
SELECT imie, nazwisko, CAST((COALESCE(dodatek,0)+pensja)*0.01 AS DECIMAL(8,2)) AS procent FROM pracownik ORDER BY procent

--11.1
SELECT TOP 1 imie, nazwisko FROM pracownik ORDER BY data_zatr
--11.2
SELECT TOP 5 nazwisko, imie FROM pracownik ORDER BY nazwisko, imie
--11.3
SELECT TOP 1 * FROM wypozyczenie ORDER BY data_wyp DESC

--12.1
SELECT imie, nazwisko, data_zatr FROM pracownik WHERE MONTH(data_zatr)=5 ORDER BY nazwisko, imie
--12.2
SELECT imie, nazwisko, DATEDIFF(d,data_zatr,GETDATE()) AS przepracowane FROM pracownik ORDER BY przepracowane DESC
--12.3
SELECT marka, typ, DATEDIFF(yy,data_prod,GETDATE()) AS lata FROM samochod ORDER BY lata DESC

--13.1
SELECT imie, nazwisko, LEFT(imie,1)+LEFT(nazwisko,1) AS inicjaly FROM klient ORDER BY inicjaly, nazwisko, imie
--13.2
SELECT UPPER(LEFT(imie,1))+LOWER(SUBSTRING(imie,2,LEN(imie)-1)), UPPER(LEFT(nazwisko,1))+LOWER(SUBSTRING(nazwisko,2,LEN(nazwisko)-1)) FROM klient
--13.3
SELECT imie, nazwisko, STUFF(nr_karty_kredyt,LEN(nr_karty_kredyt)-5,6,'xxxxxx') FROM klient

--14.1.
UPDATE pracownik
SET dodatek=50
WHERE dodatek IS NULL;
--14.2
UPDATE klient
SET imie='Jerzy', nazwisko='Nowak'
WHERE id_klient=10;
--14.3
UPDATE pracownik
SET dodatek=dodatek+100
WHERE pensja<1500
