# Luhn Project

## Overview

The purpose of this project is create tools to manipulate hash-based tables trough POSIX shell. In particular, we will start to implement a way to read YAML files from POSIX shell.

The name for this project will be Luhn, inspired on Hans Peter Luhn (1896 - 1964). Lunh was a german researcher who invented the first hash function (1953).

## Tenets

### Short term

* Define a consistent syntax to have read access to a YAML files in POSIX shell.
* Parser YAML files trough a C++11 program using a library to read YAML file.
* Generate a hash-based table to be interpreted in POSIX shell.
* Create functionality to access to some properties of the interpreted data:
  * Lengths of a nested list
  * Keys of a nested hash-based table
  * Types of value of one specific key

### Long term
* Evaluate the feasibility based-on performance to create a purely POSIX shell interpreter of YAML files.
* Manipulate other kinds of serialized data trough POSIX shell.

## Syntax

```
luhn [options] [filename]
```

### Examples

#### Reading a YAML from POSIX shell script

Following is the content of a YAML file named orders.yml:

```yaml
---
date: 2014-07-31
customer:
  first-name: Ulises
  last-name: Tirado Zatarain
  tax-id: ZTU321608RLS

items:
  - id: A795
    description: Eraser
    price: 4.95
    quantity: 2

  - id: M493
    description: Marker
    size: 3
    price: 1.99
    quantity: 17

ship-to: &shipping-address
  street: |
    Edificio Isabela 201
    Fracc. San Javier, 36020
  city: Guanjuato
  country: MX

ship-to: *shipping-address

notes: >
  If nobody is there, please
  deliver to the neighbours.
...
```

To read that file with Luhn and manipulate it trough POSIX shell, we can run following commands:

```sh
# Create a POSIX shell hash-based table with the content of a file.
order=$(luhn -y orders.yml)

# Access to specific key.
date=`$order/date`

# Display the value of the key accessed.
echo $date

# Get a reference to nested map.
customer=`$order/customer`
echo $customer/first-name	# Ulises

# Get the length of the nested list.
echo `$mydata/items:length`

# Get a list with the keys of the nested hash-based table.
echo `$mydata/customer:keys`

# Iterate over the values in a nested list.
for item in `$mydata/items`; do
	echo -e "`$item/id` \t `$item/description` \t `$item/quantity` \t `$item/price`"
done

# Get the type of value in specific key
echo `$mydata/customer:type`		# map
echo `$mydata/items:type`			# list
echo `$mydata/customer/tax-id:type`	# scalar

```