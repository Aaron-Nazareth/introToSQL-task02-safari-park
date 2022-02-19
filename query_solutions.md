# Safari Task Query Solutions

## MVP

- The names of the animals in a given enclosure

```sql
SELECT animals.name, animals.enclosure_id FROM animals
INNER JOIN enclosure
ON animals.enclosure_id = enclosure.id
WHERE animals.enclosure_id = 1;
```

- The names of the staff working in a given enclosure

```sql
SELECT staff.name, assignment.enclosureid, assignment.day FROM staff
INNER JOIN assignment
ON staff.id = assignment.staffid
WHERE assignment.enclosureid = 2;
```

## Extensions

- The names of staff working in enclosures which are closed for maintenance

```sql
SELECT staff.name, assignment.enclosureid, enclosure.closedformaintenance FROM staff
INNER JOIN assignment
ON staff.id = assignment.staffid
INNER JOIN enclosure
ON assignment.enclosureid = enclosure.id
WHERE enclosure.closedformaintenance = true;
```

- The name of the enclosure where the oldest animal lives. If there are two animals who are the same age choose the first one alphabetically.

```sql
SELECT enclosure.name FROM animals
INNER JOIN enclosure
ON animals.enclosure_id = enclosure.id
ORDER BY animals.age DESC
LIMIT 1;
```

- The number of different animal types a given keeper has been assigned to work with.

```sql
SELECT COUNT(DISTINCT animals.type) FROM animals
INNER JOIN enclosure
ON animals.enclosure_id = enclosure.id
INNER JOIN assignment
ON enclosure.id = assignment.enclosureid
WHERE assignment.staffid = 1;
```

- The number of different keepers who have been assigned to work in a given enclosure

```sql
SELECT COUNT(DISTINCT assignment.staffid) FROM enclosure
INNER JOIN assignment
ON enclosure.id = assignment.enclosureid
WHERE enclosure.id = 1;
--
-- Alternative solution provided by BNTA trainers - both solutions work
SELECT COUNT (DISTINCT staff.name) FROM staff
INNER JOIN assignment
ON staff.id = assignment.staffid
WHERE assignment.enclosureid = 1;
```

- The names of the other animals sharing an enclosure with a given animal (eg. find the names of all the animals sharing the big cat field with Tony)

```sql
SELECT roommates.name FROM animals
INNER JOIN enclosure
ON animals.enclosure_id = enclosure.id
INNER JOIN animals AS roommates	-- Using "roommates" as an alias for "animals" so we can join using the same table again
ON enclosure.id = roommates.enclosure_id
WHERE animals.id = 1;
-- This process lets us join "animals" to "enclosure" and find the enclosure for a given animal. Then we rejoin "animals" in alias form as "roommates" to now search for which animals share an enclosure id with the one we've just found.
```