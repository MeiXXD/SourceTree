学习C++，不免会学到指针，但是当指针的语句中有const的时候，你分的清楚吗？

这里教大家的一个记忆诀窍：
>按照英文中姓名的翻译习惯，从右向左翻译就一目了然了

请看下面示例：

```c
int *                     //pointer to int
int const *               //pointer to const int
int * const               //const pointer to int
int const * const         //const pointer to const int
```

####注意
>现在语句中的第一个const，位置在左或者位置在右都可以

因此：
* const int * == int const *
* const int * const == int const * const

再看下面几个稍微复杂的示例：

```c
int **                   //pointer to pointer to int
int ** const             //a const pointer to a pointer to an int
int * const *            //a pointer to a const pointer to an int
int const **             //a pointer to a pointer to a const int
int * const * const      //a const pointer to a const pointer to an int
```

现在，是不是像我说的，对加入const的指针语句一目了然了呢？