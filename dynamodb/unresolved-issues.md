# Unresolved Issues

## Increment  Id Key

I haven't found a way to make the id be a auto increment key just like MySQL. So I pass the 10-digit timestamp from font-end to the back-end, save it as a integer id.

## Scan in Order or Sorted Query All&#x20;

I haven't found a way to get all items in table while get then in order at the same time. Tour of the Hero have a top list view, so the list should be pre-sorted.

However, if I want to get all items, there isn't a sort API in scan way.

Maybe the reason why I can't achieve the function is that I didn't define a Global Secondary Index (GSI). I have to scan the table and sort the result set in Java code. That's not clean.

```
    /**
     * List Table Hero
     *
     * @return Hero list
     */
    public List<Hero> findAll() {
        // TODO Find out a way to scan in order, or query all.
        return dynamoDBMapper.scan(Hero.class, new DynamoDBScanExpression())
                .stream()
                .sorted(Comparator.comparing(Hero::getId))
                .collect(Collectors.toList());
    }
```
