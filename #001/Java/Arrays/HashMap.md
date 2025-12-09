подсчёт количества повторов слов в тексте. Это один из самых ярких примеров, зачем вообще нужен HashMap.
```java
String text = "java java core java";
HashMap<String, Integer> freq = new HashMap<String, Integer>();

for (String w : text.split(" "))
{
    Integer old = freq.get(w);
    freq.put(w, (old == null) ? 1 : old + 1);
}

System.out.println(freq); // {core=1, java=3}
```

Мы разделили строку "java java core java" на слова.
Для каждого слова смотрим, есть ли оно уже в словаре (freq.get(w)).
Если его нет (null), значит, это первое появление слова → кладём 1.
Если есть, значит, слово уже встречалось → увеличиваем значение на единицу.
