# Solve equations with different unknown

```python
#!/usr/bin/env python3

"""Solve

<JAVA>
char c = charArray[0];
char c2 = charArray[3];
char c3 = charArray[6];
if (c + c2 + c3 == 154) {
char c4 = charArray[1];
char c5 = charArray[4];
char c6 = charArray[7];
if (c4 + c5 + c6 == 159) {
char c7 = charArray[2];
char c8 = charArray[5];
char c9 = charArray[8];
if (c7 + c8 + c9 == 156) {
char c10 = charArray[9];
char c11 = charArray[12];
char c12 = charArray[15];
if (c10 + c11 + c12 == 169) {
char c13 = charArray[10];
char c14 = charArray[13];
char c15 = charArray[16];
if (c13 + c14 + c15 == 152) {
char c16 = charArray[11];
char c17 = charArray[14];
char c18 = charArray[17];
if (c16 + c17 + c18 == 155) {
char c19 = charArray[18];
char c20 = charArray[19];
bool = false;
if (c19 + c20 == 107 && c + c5 + c9 == 156 && c7 + c5 + c3 == 159 && c10 + c14 + c18 == 155 && c16 + c14 + c12 == 155 && c + c10 + c19 == 163 && c2 + c11 + c20 == 159 && c3 + c12 == 108 && c4 + c + c7 == 152 && c2 + c5 + c8 == 166 && c3 + c6 + c9 == 151 && c13 + c10 + c16 == 154 && c11 + c14 + c17 == 161 && c12 + c15 + c18 == 161 && c + '\n' == 59 && c6 + '\n' == 60 && c10 + '\n' == 67 && c19 + '\n' == 67 && c8 + c6 == 105 && c17 + c18 == 106)
<\JAVA>
"""

from sympy import symbols, Eq, solve

code = ['x']*20

# Déclaration des variables
c1, c2, c3, c4, c5, c6, c7, c8, c9, c10, c11, c12, c13, c14, c15, c16,  c17, c18, c19, c20 = symbols('c1 c2 c3 c4 c5 c6 c7 c8 c9 c10 c11 c12 c13 c14 c15 c16  c17 c18 c19 c20')

code_map = {
    c1: 0,
    c2: 3,
    c3: 6,
    c4: 1,
    c5: 4,
    c6: 7,
    c7: 2,
    c8: 5,
    c9: 8,
    c10: 9,
    c11: 12,
    c12: 15,
    c13: 10,
    c14: 13,
    c15: 16,
    c16: 11,
    c17: 14,
    c18: 17,
    c19: 18,
    c20: 19
}

# equation list
equations = [
    Eq(c7 + c8 + c9, 156),
    Eq(c10 + c11 + c12, 169),
    Eq(c13 + c14 + c15, 152),
    Eq(c16 + c17 + c18, 155),
    Eq(c4 + c5 + c6, 159),
    Eq(c1 + c2 + c3, 154),
    Eq(c19 + c20, 107 ),
    Eq(c1 + c5 + c9, 156 ),
    Eq(c7 + c5 + c3, 159 ),
    Eq(c10 + c14 + c18, 155 ),
    Eq(c16 + c14 + c12, 155 ),
    Eq(c1 + c10 + c19, 163 ),
    Eq(c2 + c11 + c20, 159 ),
    Eq(c3 + c12, 108 ),
    Eq(c4 + c1 + c7, 152 ),
    Eq(c2 + c5 + c8, 166 ),
    Eq(c3 + c6 + c9, 151 ),
    Eq(c13 + c10 + c16, 154 ),
    Eq(c11 + c14 + c17, 161 ),
    Eq(c12 + c15 + c18, 161 ),
    Eq(c1 + 10, 59 ),
    Eq(c6 + 10, 60 ),
    Eq(c10 + 10, 67 ),
    Eq(c19 + 10, 67 ),
    Eq(c8 + c6, 105 ),
    Eq(c17 + c18, 106),
]

# solve system
solutions = solve(equations)

print(f"Solution => {solutions}")
```