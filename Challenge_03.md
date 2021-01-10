## First Question
```
select 100.0 * count(*) / (select count(*) from transactions) from transactions where input_total_usd == 0 and output_total_usd > 0
```
## Second Question
```
select avg(count_block)
from (select count(*) as count_block 
from transactions
group by block_id)
```
## Third Question
```
select recipient from (
select recipient, count(recipient) as n
from inputs
where recipient not in (select recipient from outputs)
group by recipient
order by recipient )
where n > 20
```
## Fourth Question
```
WITH row_number_block_id AS (
  SELECT *, ROW_NUMBER() OVER (PARTITION BY block_id ORDER BY input_total_usd) AS rn
  FROM transactions
)
SELECT block_id, hash
FROM row_number_block_id
WHERE rn < 4
```
## Fifth Question
```
select o2.recipient as end
from inputs as i inner join outputs as o inner join inputs as i2 inner join outputs as o2
where i.recipient = '1LsMxZRJuRxshvCzNZtLkV71gXm62mWvR5' and
i.transaction_hash = o.transaction_hash and
o.recipient = i2.recipient and
i2.transaction_hash = o2.transaction_hash
```