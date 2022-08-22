Тестирование и исправление  

Что делает платформа с базой на MSSQL  


# 1 Реиндексация

Выполняются запросы на ребилд индексов:  
```ALTER INDEX ALL ON [mislight-dev-sql].dbo._Const2694 REBUILD```

План запроса достаточно простой - скан и инсерт:  

  Index Insert(OBJECT:([mislight-dev-sql].[dbo].[_Const2694].[_Const2694_1]))

    |--Clustered Index Scan(OBJECT:([mislight-dev-sql].[dbo].[_Const2694].[_Const2694_1]), ORDERED FORWARD) 


# 2 Пересчет итогов


Все  уникальные запросы из профайлера не выбирал, то, что попалось навскидку.  

drop index _AccumRgTn2320_2 on dbo._AccumRgTn2320

TRUNCATE TABLE _AccumRgTn2320

SELECT
MAX(T1._Period)
FROM dbo._AccumRg2267 T1

CREATE INDEX _AccumRgTn2320_4 ON dbo._AccumRgTn2320 (_Fld2271RRef, _Period) WITH (SORT_IN_TEMPDB = ON, MAXDOP = 0)


insert bulk #tt1([_IDRRef] binary(16),[_EDCount] numeric(2,0))with(TABLOCK, ROWS_PER_BATCH=261)

-- регистр накопления

TRUNCATE TABLE _AccumRgT2266

-- регистр сведений

DELETE FROM T1
FROM dbo._InfoRgSL505 T1

INSERT INTO dbo._InfoRgSL505 (_Period, _Fld501RRef, _Fld502, _Fld503) SELECT
T3._Period,
T3._Fld501RRef,
T3._Fld502,
T3._Fld503
FROM (SELECT
T2._Fld501RRef AS Fld501RRef,
MAX(T2._Period) AS MINMAX_PERIOD_
FROM dbo._InfoRg500 T2
GROUP BY T2._Fld501RRef) T1
INNER JOIN dbo._InfoRg500 T3
ON T3._Fld501RRef = T1.Fld501RRef AND T3._Period = T1.MINMAX_PERIOD_









# 3 Реструктуризация таблиц