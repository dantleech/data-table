Data Table
==========

[![Build Status](https://travis-ci.org/dantleech/data-table.svg?branch=master)](https://travis-ci.org/dantleech/data-table)

The data table library provides an object oriented way of representing and analyzing tabular data.

Features:

- Supports aggregate `sum`, `avg`, `min`, `max` and `median`.
- Aggreate on `Table`, `Row` or `Column`.
- Supports cell groups.
- Fluent buildern

Note that this library has no release and should not be considered stable.

Creating
--------

Col 1 | Col 2 | Col 3
----- | ----- | -----
12    | 14    | 4
12    | 14    | 4

Would be created as follows:

````php
$table = Table::createBuilder()
    ->row()
        ->cell(12)
        ->cell(14)
        ->cell(4)
    ->end()
    ->row()
        ->cell(12)
        ->cell(14)
        ->cell(4)
    ->end()
    ->getTable();
````

Or without the builder:

````php
$table = new Table(
     new Row(array(
         new Cell(12),
         new Cell(14),
         new Cell(4),
     )),
     new Row(array(
         new Cell(12),
         new Cell(14),
         new Cell(4),
     )),
 );
````

Resolving aggregate values
--------------------------

All elements implement an aggregateable interface, allowing the following:

````php
echo $table->sum(); // sum of table
echo $table->avg(); // average value of table

foreach ($table as $row) {
    echo $row->sum(); // average value of the row
    echo $row->avg(); // min value of row
}

echo $table->getColumn(0)->sum(); // sum of column
````

Assigning groups and accessing group data
-----------------------------------------

Groups can be used to analyze only certain cells:

````php
 $table = new Table(
     new Row(array(
         new Cell(12, ['group1']),
         new Cell(14, ['group1']),
         new Cell(4, ['group2']),
     )),
 );

 echo $table->sum(['group1']); // 26
 echo $table->sum(['group2']); // 4
````

Aggregating/grouping table data
-------------------------------

You can aggregate the values in a table based on one or more unique cell
values in a given column.

````php
$table = new Table(
    new Row(array(
        new Cell('beer'),
        new Cell(14),
        new Cell(4),
    )),
    new Row(array(
        new Cell('beer'),
        new Cell(14),
        new Cell(4),
    )),
    new Row(array(
        new Cell('snitzel'),
        new Cell(14),
        new Cell(4),
    )),
);

$newInstance = $table->aggregate([0]); // aggregate on the zeroeth column
$newInstance->getRow(0)->getCell(1); // 28 -- the values have been aggregated
````
