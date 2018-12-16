# HIVE

-------------------------------------SCHEMA ON READ ------------------------------------------------

CREATE TABLE ratings ( userID INT, mvoieID INT, rating INT, time INT)
ROW FORMAT DELIMTED FIELD TERMINATED BY '\t'  
STORED AS TEXTFILE;

LOAD DATA LOCAL INPATH '${env:HOME}/ml-100k/u.data'
OVERWRITE INTO TABLE ratings;


--------------------------------------EXTERNAL TABLE--------------------------------------------------

CREATE EXTERNAL TABLE IF NOT EXISTS ratings (
          userID   INT,
          movieID  INT,
          rating   INT,
          time     INT)
ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
LOCATION '/data/ml-100k/u.data;:

------------------------------------Partitioning ------------------------------------------------

CREATE TABLE customers(
      name STRING,
      address STRUCT<street:STRING, city:STRING, state:STRING, zip:INT>
      )
      PARTITIONED BY (country STRING);
      
                          EXP:
                          .../customers/country=CA/
                          .../customers/country=GB/
                          
-------------------------------------HIGHEST AVG RATING WHERE RATING IS > 10 -------------------------

CREATE VIEW IF NOT EXISTS avgRatings AS
SELECT movieID, avg(rating) as avgRating, COUNT(movieID) as ratingCount
FROM ratings
GROUP BY movieID
ORDER BY avgRating DESC;

SELECT n.title, avgRating
FROM avgRatings t JOIN names n ON t.movieID = n.movieID
WHERE ratingCount > 10;



-------------------------------------------ASSGINMENT ------------------------------------------------
exchange, stock_symbol,date, stock_price_open, stock_price_high, stock_price_low, stock_price,_close, stock_volume, stock_price_adj_close


CREATE EXTERNAL TABLE IF NOT EXISTS dailyPrices (
          exchange               STRING,
          stock_symblol          STRING,
          date                   DATE,
          stock_price_open       DOUBLE,
          stock_price_high       DOUBLE,
          stock_price_low        DOUBLE,
          stock_price_close      DOUBLE,
          stock_price_volume     INT,
          stock_price_adj_close  DOUBLE)
ROW FORMAT DELIMITED FIELDS TERMINATED BY'\t'
LOCATION '/data/ml-100k/u.data';
