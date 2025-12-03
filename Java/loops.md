### Поток выполнения цикла

- break завершает работу цикла
- оператор continue пропускает текущуую итерацию цикла и происходит переход к следующей

break и continue во внутреннем цикле влияют только на него, внешний продолжает работу.

```java
if (secretKeyCandidate <= 1) {
    System.out.println("NO");
    return;
}

boolean isPrime = true;

// Проверяем делители от 2 до sqrt(n). i <= n / i эквивалентно i*i <= n, без риска переполнения int.
for (int i = 2; i <= secretKeyCandidate / i; i++) {
    if (secretKeyCandidate % i == 0) { // Нашли делитель — число составное
        isPrime = false;
        break;
    }
}
System.out.println(isPrime ? "YES" : "NO");
```
