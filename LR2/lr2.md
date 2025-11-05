## Комп'ютерні системи імітаційного моделювання
## СПм-24-4, **Чижов Борис Олександрович**
### Лабораторна робота №**2**. Редагування імітаційних моделей у середовищі NetLogo
<br>

### Варіант 7, модель у середовищі NetLogo:
[Wolf Sheep Predation](https://www.netlogoweb.org/launch#https://www.netlogoweb.org/assets/modelslib/Sample%20Models/Biology/Wolf%20Sheep%20Predation.nlogox)

Прибрати "зграйність" вовків - тепер перед початком свого ходу вовки повинні "оглядатися", перевіряючи оточення, та обирати напрямок руху виходячи з наявності вівець та відсутності інших вовків. Якщо немає іншої можливості – переміщається випадково. При знаходженні на одній ділянці поля двох вовків залишається лише один з них. Вівці переміщаються випадковим чином, але при виявленні вовка на одній із клітин поруч змінюють напрямок на протилежний.

<br>

### Внесені зміни у вихідну логіку моделі за варіантом:

Виправлення викликів у to **go**, а саме заміна одного універсального правила **move** на окремі **move-sheep** та **move-wolf** оскільки раніше і вовки, і вівці викликали однакову move, тепер робимо дві окремі.



Замість move

<pre>
to move  ; turtle procedure
  rt random 50
  lt random 50
  fd 1
end
</pre>

Робимо окремі правила для овець:

<pre>
to move-sheep
  
  ifelse any? wolves-on neighbors [
    rt 180
    fd 1
  ]
  [
    rt random 50
    lt random 50
    fd 1
  ]
end
</pre>

Та вовків, але добавимо умову, що вовк може пересуватися лише коли одн на клітинці. Якщо вовк один на клітинці то ходить нормально. Якщо в клітинці вже є інший вовк, інший вовк пропускає хід, стоїть і чекає Як тільки один із них піде — другий може знову ходити

<pre>
to move-wolf
  
  ifelse count wolves-here > 1 [
    stop     ]
[
  
  rt random 50
  lt random 50
  fd 1
  ]
end
</pre>

Добавимо функцію, коли вовк перед початком свого ходу буде оглядатися.

<pre>
to look-around-wolf
  let candidate-patches patches in-radius vision-radius with [ any? sheep-here and not any? wolves-here ]
  if any? candidate-patches [
    
    let nearest-min min [ distance myself ] of candidate-patches
    let nearest-patches candidate-patches with [ distance myself = nearest-min ]
    face one-of nearest-patches
  ]
end
</pre>

Також у інтерфейс моделі було додано слайдер із глобальною параметром vision-radius, який буде визначати скільки вовк буде бачити клітинок, коли оглядається.

![Покращення інтерфейсу моделі](im3.png)


