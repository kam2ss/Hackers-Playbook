```
years = [2020, 2021]
chars = ['!', '@', '#', '$', '%', '^', '&', '*', '(', ')']

with open('passwords.txt', 'w') as f:
    for year in years:
        for char in chars:
            f.write(f"Fall{year}{char}\n")
```