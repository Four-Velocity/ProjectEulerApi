# ProjectEulerApi

Api for retrieving ProjectEuler problems and verify solutions

## Short specs

### API

API must include endpoints for retrieving
[ProjectEuler](https://projecteuler.net/) problems and checking
solutions.  
For retrieving problems we can use:

```rest
https://example.com/problems/
LIST
{
    "data": [
        {"id": XXX, "text": "Lorem ipsum ...", "link": "https://projecteuler.net/problem=xxx"},
        {"id": XXX, "text": "... dolor sit amet...", "link": "https://projecteuler.net/problem=xxx"},
        ...
    ],
    "meta": {
        "lang": "en",
        "offset": 0,
        "limit": XXX,
    }
}
```

> Allow `offset` and `limit` params in LIST

```rest
https://example.com/problems/{id}/
GET
{
    "id": XXX,
    "text": "Lorem ipsum...",
    "link": "https://projecteuler.net/problem=xxx",
    "lang": "en"
}
```

For checking solutions we can use POST

> We **must never** show the solution itself.

```rest
https://example.com/problems/{id}/check/
POST | REQUEST
{
    "solution": "some_solution"
}
```

```rest
https://example.com/problems/{id}/check/
POST | RESPONSE
{
    "id": XXX,
    "result": true/false
}
```

> The responses are `camelCase` the pydantic model fields are
> `snake_case` chek this out to understand the principles:
> [Medium](https://medium.com/analytics-vidhya/camel-case-models-with-fast-api-and-pydantic-5a8acb6c0eee)

### Translations

The text of the problems must be translatable, and the translations must
be provided in human-readable format to make translation-contributions
easy.

For example, we can use toml, but not sure which way will be better
(problem based or translation based)

#### Problem based way:

**Problem001.toml**

```toml
en = """
If we list all the natural numbers below 10 that are multiples of 3 or 5, we get 3, 5, 6 and 9. The sum of these multiples is 23.

Find the sum of all the multiples of 3 or 5 below 1000."""
ru = """
Если выписать все натуральные числа меньше 10, кратные 3 или 5, то получим 3, 5, 6 и 9. Сумма этих чисел равна 23.

Найдите сумму всех чисел меньше 1000, кратных 3 или 5."""
```

**Problem002.toml**

```toml
en = """
Each new term in the Fibonacci sequence is generated by adding the previous two terms. By starting with 1 and 2, the first 10 terms will be:

1, 2, 3, 5, 8, 13, 21, 34, 55, 89, ...

By considering the terms in the Fibonacci sequence whose values do not exceed four million, find the sum of the even-valued terms."""

ru = """
Каждый следующий элемент ряда Фибоначчи получается при сложении двух предыдущих. Начиная с 1 и 2, первые 10 элементов будут:

1, 2, 3, 5, 8, 13, 21, 34, 55, 89, ...

Найдите сумму всех четных элементов ряда Фибоначчи, которые не превышают четыре миллиона."""
```

#### Translation based way:

**en.toml**

```toml
problem1 = """
If we list all the natural numbers below 10 that are multiples of 3 or 5, we get 3, 5, 6 and 9. The sum of these multiples is 23.

Find the sum of all the multiples of 3 or 5 below 1000."""

problem2 = """
Each new term in the Fibonacci sequence is generated by adding the previous two terms. By starting with 1 and 2, the first 10 terms will be:

1, 2, 3, 5, 8, 13, 21, 34, 55, 89, ...

By considering the terms in the Fibonacci sequence whose values do not exceed four million, find the sum of the even-valued terms."""
```

**ru.toml**

```toml
problem1 = """
Если выписать все натуральные числа меньше 10, кратные 3 или 5, то получим 3, 5, 6 и 9. Сумма этих чисел равна 23.

Найдите сумму всех чисел меньше 1000, кратных 3 или 5."""

problem2 = """
Каждый следующий элемент ряда Фибоначчи получается при сложении двух предыдущих. Начиная с 1 и 2, первые 10 элементов будут:

1, 2, 3, 5, 8, 13, 21, 34, 55, 89, ...

Найдите сумму всех четных элементов ряда Фибоначчи, которые не превышают четыре миллиона."""
```

#### Translation mechanism

On every deploy the translation must be updated from that file to the
database.

The preferred way to do it is:
1. Try to get the problem text from the DB
2. If the problem translation does not exist create the new one
3. If it exist: compare the text hashes from db and from translation
   files. Update only if hashes aren't match

### DB models

![DB_SCHEME](/docs/puml_rendered/db_scheme.png?raw=true)

### Workflow requirements

* All code are covered with unit tests (use pytest pls)
* All linters checks are passed
* All code must be reformatted with black and isort
* Use [gitmoji](https://gitmoji.dev/) in commits
* To make a contribution:
  * Create new branch and make a PR for it. Set its
    status to WIP by adding 🚧 emoji to the beginning of the PR name. (Use
    emoji, not emoji code)
  * Do everything you have to do
  * After all checks are passes merge you commit to the `main` branch


### Good to have in the future

* Registration with captcha to avoid multiple request from users or add some throtling mechanism
* Token auth
* Docker image to use project locally
