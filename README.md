# Fortran-Best-Practices

####Explicitly leave spaces between numbers in formatted output. Otherwise the numbers may be "mushed" together if they are larger than expected.

Instead of 

```write (*,"('coeff:',100f12.6)") x(1:n)```

write

```write (*,"('coeff:',100(1x,f12.6))") x```
