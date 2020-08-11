# Bash references

## conditionals
```bash
if [ "foo" = "foo" ]; then
   echo expression evaluated as true
fi
```

## for loop
```bash
for i in "$(seq 1 2 20)"
do
   echo "Welcome $i times"
done
```

## while loop
https://tldp.org/LDP/Bash-Beginners-Guide/html/sect_09_02.html

```bash
while (true); do
    echo "Hello!"
done
```

```bash
i="0"
while [ $i -lt 4 ]
do
    i=$[$i+1]
    echo "$i"
done
```