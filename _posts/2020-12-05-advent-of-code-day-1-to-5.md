---
layout: post
title: Advent of Code 2020
categories: [podcast, how2robabank]
comments: false
---
This is the first year I am doing the [Advent of Code](https://adventofcode.com/2020)! Advent of Code is an Advent calendar with small programming puzzles that can be solved in any programming language. It's a great way to practise programming and develop problem solving skills, plus the problems form a story to make things more interesting! Here's my attempt at the first few days of the Advent of Code.

#### Day 1: Report Repair
The first problem consisted of an expense report as a list of numbers. The task is to find the two entries that sum to 2020 and multiply them together.

{% highlight python %}
def read_input(file):
    with open(file, 'r') as f:
        data = f.readlines()
    f.close()
    return data

def convert_to_integers(data):
    data = [int(i) for i in data]
    return data

def find_sum(data, target):
    for i in data:
        remainder = target - i
        if remainder in data:
            return i, remainder
        
def day_one_main(target):
    data = convert_to_integers(read_input("Day01_input.txt"))
    i, j = find_sum(data, target)
    return i * j
{% endhighlight %}

#### Day 2: Password Philosophy
The task invovled an input file with each line containing the a password policy followed by the password, for example "1-3 a: abcde". The password policy indicates the lowest and highest number of times a given letter must appear for the password to be valid. For example, "1-3 a" means that the password must contain a at least 1 time and at most 3 times.

{% highlight python %}
def read_input(file):
    with open(file, 'r') as f:
        data = f.readlines()
    f.close()
    return data

def get_validity_count(data):
    count = 0
    for line in data:
        if validate_password(line):
            count += 1
    return count

def validate_password(line):
    limit, letter, password = line.split()
    lower, upper = limit.split("-")
    count = password.count(letter[0])
    if count >= int(lower) and count <= int(upper):
        return True
    return False

def day_two_main(f):
    data = read_input(f)
    return get_validity_count(data)
{% endhighlight %}

#### Day 3: Toboggan Trajectory
I found this problem quite unique and fun to solve as I was given an input similar to the below.

    ..##.......
    #...#...#..
    .#....#..#.
    ..#.#...#.#
    .#...##..#.
    ..#.##.....

Where trees grow on exact integer coordinates in a grid. The puzzle input consists of a map of the open squares (.) and trees (#) for the toboggan to travel through. The pattern of each line repeats infinitely. Starting in the top-left corner and following a specific travelling pattern of right x and down y, the problem asked how many trees would be encountered following a travelling pattern of right 3 and down 1.

{% highlight python %}
def read_input(file):
    with open(file, 'r') as f:
        data = f.readlines()
    f.close()
    return data

def count_trees(right, down, data):
    count = 0
    x = 0
    y = 0
    bottom = len(data)
    mod = len(data[0]) - 1 # get the length of first line
    while(y < bottom - 1):
        x += right
        x = x % mod # account for repeating patterns
        y += down
        if data[y][x] == "#":
            count += 1
    return count

def day_three_main(f):
    data = read_input(f)
    return count_trees(3, 1, data)
{% endhighlight %}

#### Day 4: Passport Processing
The problem for day 4 involved detecting which passports contain all the required fields and then report the number of valid passports.

{% highlight python %}
import csv
import re

def read_input(file):
    with open(file, 'r') as f:
        data = f.read().split('\n\n') # identify individual passports by an empty line
        data = [x.replace('\n', ' ').split() for x in data]
    return data

def scan_passports(data):
    passports = []
    for passport in data:
        passports.append(dict(p.split(':') for p in passport))
    return passports
        
def validate_passports(passports): # Part One
    count = 0
    for passport in passports:
        try:
            if ((passport['byr'] != "") and 
                (passport['iyr'] != "") and 
                (passport['eyr'] != "") and
                (passport['hgt'] != "") and
                (passport['hcl'] != "") and
                (passport['ecl'] != "") and
                (passport['pid'] != "") ):
                count += 1
        except KeyError:
            print("Invalid passport")
    return count

def strict_validate_passorts(passports): # Part Two
    count = 0
    for passport in passports:
        invalid = False
        message = "Invalid fields: "
        try:
            if not (1920 <= int(passport['byr']) <= 2002):
                invalid = True
                message += " byr"
            if not (2010 <= int(passport['iyr']) <= 2020):
                invalid = True
                message += " iyr"
            if not (2020 <= int(passport['eyr']) <= 2030):
                invalid = True
                message += " eyr"
            if not (((passport['hgt'][-2:] == "cm") and (150 <= int(passport['hgt'][:-2]) <= 193)) or 
                   ((passport['hgt'][-2:] == "in") and  (59 <= int(passport['hgt'][:-2]) <= 76))):
                invalid = True
                message += ' hgt'
            if not ((re.match('#[\da-f]{6}$', passport['hcl']))):
                invalid = True
                message += ' hcl'
            if not (passport['ecl'] in ['amb', 'blu', 'brn', 'gry', 'grn', 'hzl', 'oth']):
                invalid = True
                message += ' ecl'
            if not (re.match(r'\d{9}$', passport['pid'])):
                invalid = True
                message += ' pid'
                
            if not invalid:
                count += 1
        except KeyError:
            print("Invalid passport")
    return count
{% endhighlight %}

<!-- #### Day 5: Binary Boarding
This problem described a scenario where an airline uses binary space partitioning to seat people. For example, a seat might be specified as FBFBBFFRLR, where F means "front", B means "back", L means "left", and R means "right". The first 7 characters are either F or B and specify one of the 128 rows on the plane. Each letter tells you which half of a region the given seat is in. The last 3 characters are either L or R and similarly define which half of a region the seat is in. The seat ID is then defined as row * 8 + column.

The problem gave an input file containing a list of boarding passes. Part one asked to find the highest seat ID on a boarding pass. Part two of the problem asked to find the ID of the missing seat.

{% highlight python %}

{% endhighlight %} -->