---
icon: pen-to-square
---

# Kuasai Dasar Python

Langkah-langkah untuk menguasai dasar PythoN

## Data Types

Pelajari tipe data dasar Python berikut ini:

* **String**: Teks diapit dengan tanda kutip (`' '`, `" "`, atau `""" """`)

{% code lineNumbers="true" %}
```python
text = "Hello, Zia!"
print(text.upper())  # Output: HELLO, ZIA!
```
{% endcode %}

* **List**: Koleksi elemen yang dapat diubah (mutable) dan memiliki indeks.

{% code lineNumbers="true" %}
```python
fruits = ["apple", "banana", "cherry"]
fruits.append("orange")  # Menambah elemen ke list
print(fruits[1])         # Output: banana
```
{% endcode %}

* **Tuple**: Koleksi elemen yang tidak dapat diubah (immutable)

{% code lineNumbers="true" %}
```python
coordinates = (10, 20)
print(coordinates[0])  # Output: 10
```
{% endcode %}

* **Dictionary**: Koleksi pasangan kunci-nilai

{% code lineNumbers="true" %}
```python
user = {"name": "Zia", "age": 30}
print(user["name"])  # Output: Zia
```
{% endcode %}

* **Set**: Koleksi elemen unik.

{% code lineNumbers="true" %}
```
numbers = {1, 2, 3, 4, 4}
print(numbers)  # Output: {1, 2, 3, 4}
```
{% endcode %}
