---
title: C language
date: 2024-10-05 01:24:00 +0200
categories: [University]
tags: [basics]
description: An introduction to data types and format specifiers in C language
---

As I started studying in university, I had to learn C. These are my notes, just in case I forget something, so I can quickly come back and review them.

## Data types
## Data types
| Data type     | Size           | Value range                               |
| ------------- | -------------- | ----------------------------------------- |
| signed int    | 2 or 4 bytes    | -32,768 to 32,767                         |
| unsigned int  | 2 or 4 bytes    | 0 to 65,535                               |
| float         | 4 bytes         | 1.2E-38 to 3.4E+38 (6 decimal places)     |
| double        | 8 bytes         | 2.3E-308 to 1.7E+308 (15 decimal places)  |
| char          | 1 byte          | -128 to 127                               |
| unsigned char | 1 byte          | 0 to 255                                  |

## Format specifiers
| Name     | Usage                                                               |
| -------- | ------------------------------------------------------------------- |
| `%d`, `%i` | Signed decimal integer                                             |
| `%u`     | Unsigned decimal integer                                             |
| `%f`     | Decimal floating point                                               |
| `%e`, `%E` | Scientific notation                                                |
| `%g`, `%G` | Automatically chooses `%f` or `%e`/`%E` depending on the value     |
| `%c`     | Single character                                                     |


## Input/output

C language uses functions like `printf()` and `scanf()` for input and output:

- `printf(const char *format, ...);` — Outputs formatted data.
- `scanf(const char *format, ...);` — Reads formatted input.