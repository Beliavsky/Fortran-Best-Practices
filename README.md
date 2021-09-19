# Fortran Best Practices

#### Use format strings instead of format statements
It is easier for the reader to understand a ```write``` statement if the format string is on the same line, rather than in a separate numbered format statement.

#### Explicitly leave spaces between numbers in formatted output
Otherwise the numbers may be "mushed" together if they are larger than expected. Instead of 

```write (*,"('coeff:',100f12.6)") x(1:n)```

write

```write (*,"('coeff:',100(1x,f12.6))") x(1:n)```
